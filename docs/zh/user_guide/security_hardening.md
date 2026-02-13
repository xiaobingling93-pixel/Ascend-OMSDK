# 安全配置和加固<a name="ZH-CN_TOPIC_0000001578489796"></a>

## 系统能力设置<a name="ZH-CN_TOPIC_0000001628849865"></a>

**界面系统能力项设置<a name="section1881753116512"></a>**

在Web界面上进行系统能力设置请参见[安全策略](./usage.md#安全策略)章节进行操作。

**能力项<a name="zh-cn_topic_0000001447161177_section182160324717"></a>**

在以低权限的Nobody用户运行Nginx的过程中，由于需要绑定443端口，因此需要配置Nginx能力；另外在以低权限的HwHiAiUser用户运行discovery进程时，由于需要使用原始套接字进行组播报文发送，因此需要配置discovery能力。详情请参见[表1 能力配置说明](#能力配置说明)。

**表 1**  能力配置说明<a id="能力配置说明"></a>

|能力|含义|使用原因|
|--|--|--|
|cap_net_bind_service|允许Nginx绑定到小于1024的端口。|Nginx需要使用443端口，所以需配置绑定443端口的能力。|
|cap_net_raw=+ep|允许discovery使用原始套接字。|discovery需要使用原始套接字收发组播报文。|

## Nginx IP侦听列表<a name="ZH-CN_TOPIC_0000001578449864"></a>

- 边缘管理系统启动Nginx进程时，会获取IP配置中的用途属性，只会将web用途的IP添加到Nginx IP侦听列表。
- 不支持无线网络、WiFi等浮动IP的侦听。

**IP相关配置文件<a name="zh-cn_topic_0000001447161121_section174321318217"></a>**

- IPv4地址配置文件：
    - openEuler：/etc/sysconfig/network-scripts/ifcfg-_xxx_（<i>xxx</i>为eth0、eth1）
    - Ubuntu：/etc/netplan/01-netcfg.yaml

- IPv4用途配置文件：/home/data/ies/tag.ini

> [!NOTE] 说明 
> 
>- IP配置文件路径默认支持以下操作系统。
>    - openEuler  22.03
>    - Ubuntu  22.04
>- IP地址仅支持本地管理。
>- IPv6地址用于SmartKit自发现设备。
>- Nginx侦听的IPv4地址和IPv6地址须为同一网卡的，如果已侦听的IPv4网卡不存在IPv6地址，则不侦听IPv6地址。
>- 保证当前Nginx侦听的最多只能是1个IPv4地址和一个IPv6地址，如果存在多个用途为“web”的IP，系统会根据网卡连接状态，选择网卡顺序（网口0 \> 网口1\~4）优先级高的一个IP侦听。

## 关键服务资源限制<a name="ZH-CN_TOPIC_0000001651243070"></a>

- 对系统关键服务使用资源进行限制，实现对服务内存资源的监测，当内存占用大于设定阈值时，会自动重启服务。
- 限制关键服务CPU的最大使用率，使服务不会持续超过该CPU使用率限值，保障该服务的CPU使用，不影响其他服务进程。

> [!NOTE] 说明
> 
>- 关键服务内存资源限制和CPU资源限制默认已配置，开启oom-killer机制即可生效。
>- 系统默认没有开启oom-killer机制，执行**echo 1 \> /proc/sys/vm/enable\_oom\_killer**命令可以开启该机制。
