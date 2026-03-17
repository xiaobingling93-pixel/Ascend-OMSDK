# 安装部署<a name="ZH-CN_TOPIC_0000001577530696"></a>

## 获取软件包<a name="ZH-CN_TOPIC_0000001633390689"></a>

**表 1**  获取软件包

|软件包名称|说明|获取方式|
|--|--|--|
|Ascend-mindxedge-omsdk_{version}_linux-aarch64.zip|边缘管理系统软件包。|[获取链接](https://gitcode.com/Ascend/OMSDK/releases)|

## 准备安装环境<a name="ZH-CN_TOPIC_0000001632751317"></a>

OM SDK提供安装脚本，通过install.sh命令安装软件包。

**前提准备<a name="section3824145117349"></a>**

- 已经获取安装软件包。
- 已登录需要安装软件包的边缘设备。
- 检查磁盘空间，确保磁盘空间充足，磁盘空间至少大于获取到的安装包的2倍以上。
- 检查边缘设备是否正常运行。

**依赖工具<a name="section143281824192813"></a>**

在进行安装软件之前，需要参考[表1 安装工具](#安装工具)完成安装前准备。

**表 1**  安装工具<a id="安装工具"></a>

|依赖|说明|安装命令或步骤|
|--|--|--|
|net-tools|配置管理网络功能的工具。|执行以下命令，安装net-tools。<li>openEuler<ul>`dnf install ethtool -y`</ul></li><li>Ubuntu<ul>`apt-get install net-tools -y`</ul></li>|
|ntp|一种基于NTP协议的服务器，能够在分布式时间服务器和客户端之间进行时间同步，使网络中的计算机时间都与国际标准时间（UTC）同步。|执行以下命令，安装ntp。<li>openEuler<ul>`dnf install ntp -y`</ul></li><li>Ubuntu<ul>`apt-get install ntp -y`</ul></li>|
|smartmontools|开源的磁盘控制，监测工具。|执行以下命令，安装smartmontools。<li>openEuler<ul>`dnf install smartmontools -y`</ul></li><li>Ubuntu<ul>`apt-get install smartmontools -y`</ul></li>|
|nfs|网络文件系统，允许网络中的计算机之间共享资源。|执行以下命令，安装nfs。<li>openEuler<ul>`dnf install nfs-utils -y`</ul></li><li>Ubuntu<ul>`apt-get install nfs-common -y`</ul></li>|
|ethtool|查询及设置网卡参数的时候需要用到的命令。|执行以下命令，安装ethtool。<li>openEuler<ul>`dnf install ethtool -y`</ul></li><li>Ubuntu<ul>`apt-get install ethtool -y`</ul></li>|
|parted|硬盘分区或调整分区大小的工具。|执行以下命令，安装parted。<li>openEuler<ul>`dnf install parted -y`</ul></li><li>Ubuntu<ul>`apt-get install parted -y`</ul></li>|
|arping|用于发送ARP请求报文。|执行以下命令，安装arping。<li>openEuler<ul>`dnf install arping -y`</ul></li><li>Ubuntu<ul>`apt-get install arping -y`</ul></li><br> > [!NOTE] 说明<br>用户需要确保安装的arping的版本在2.19以上。|
|inotify-tools|用于监测文件系统事件的工具集，它可以在文件或目录发生变化时自动执行指定的命令或脚本。|执行以下命令，安装inotify-tools。<li>openEuler<ul>`dnf install inotify-tools -y`</ul></li><li>Ubuntu<ul>`apt-get install inotify-tools -y`</ul></li>|

## 通过命令行安装<a id="ZH-CN_TOPIC_0000001582431578"></a>

**安装前必读<a name="section1309123514117"></a>**

- 安装过程中请不要关闭电源，以免损坏设备。
- 安装过程中请不要对OM SDK进行业务配置及其它维护类操作，以免用户数据与配置丢失或安装失败。
- 如果用户需要边缘管理系统侦听开启了DHCP功能的网卡，需要将DHCP功能设置在eth0（网口0）上或者eth0不插网线，eth1（网口1\~4）打开DHCP功能。

**通过命令行安装<a name="section7621141414618"></a>**

1. 将软件包上传到环境任意目录下（如“/home”）。
2. 在软件包目录下，执行以下命令，创建临时目录om\_install。

    ```bash
    mkdir om_install
    ```

3. 执行以下命令，解压OMSDK软件包。

    ```bash
    unzip Ascend-mindxedge-omsdk_{version}_linux-aarch64.zip
    tar -zxf Ascend-mindxedge-omsdk_{version}_linux-aarch64.tar.gz -C om_install
    ```

4. 执行以下命令，为安装脚本添加可执行权限。

    ```bash
    chmod +x om_install/install.sh
    ```

5. 执行以下命令，安装软件包。

    ```bash
    om_install/install.sh
    ```

    回显示例如下，表示安装完成。

    ```text
    check install environment success
    prepare service file success
    executing install success
    start service success
    Install MindXOM success, MindXOM service is ready.
    ```

    > [!NOTE] 说明 
    >- 第一次登录成功后，需要修改admin账号。
    >- 安装操作的运行日志可以通过**tail -f /var/plog/upgrade.log**查看。
    >- install.sh命令说明请参见[install.sh命令](./appendix.md#installsh命令)。

# 升级<a name="ZH-CN_TOPIC_0000001634859225"></a>

## 升级前必读<a name="ZH-CN_TOPIC_0000001634738585"></a>

**升级影响<a name="section289964514262"></a>**

- 对业务的影响：升级过程涉及到系统重启，用户可自己选择复位重启时间，重启会中断约5分钟。
- 对网络通信的影响：FusionDirector和边缘管理系统的连接会中断约5分钟。

**注意事项<a name="section24641058112720"></a>**

- 在进行升级操作之前，请仔细阅读本文档，确定已经理解全部内容。如果您对文档有任何意见或建议，请联系华为技术支持解决。
- 升级过程禁止进行其他维护操作，升级时间约15分钟。
- 为了减少对系统的影响，请选择在业务量低时进行版本升级操作。
- 若当前升级环境空间不足，可参考[清理磁盘空间](./common_operations.md#清理磁盘空间)章节进行清理。

**升级流程<a name="section1838323114914"></a>**

1. 获取软件包。

    > [!NOTE] 说明  
    > 若用户想要升级Atlas 200I A2 加速模块的驱动，可以参见《[Atlas 200I A2 加速模块 驱动开发指南](https://support.huawei.com/enterprise/zh/doc/EDOC1100541055/426cffd9?idPath=23710424|251366513|22892968|252309141|254411267)》的“升级驱动”章节进行操作。

2. 升级固件。支持通过边缘管理系统的Web界面升级或者通过FusionDirector实现升级操作。
3. 升级后检查。检查升级操作是否成功。

**升级前检查<a name="section1176972311510"></a>**

1. 检查软件版本。

    登录边缘管理系统的Web界面，在主菜单中选择“管理 \> 系统信息”，查看当前固件版本。

2. 检查有线网络中的IP用途。
    - 有线网络中存在“web”用途的IP，可直接升级。

    - 有线网络中不存在“web”用途的IP，请先设置一个用途为“web”的IP，再进行升级，否则会升级失败。

## 升级场景<a name="ZH-CN_TOPIC_0000001584637368"></a>

### 通过边缘管理系统升级<a name="ZH-CN_TOPIC_0000001584299540"></a>

**升级前准备<a name="section131331915189"></a>**

- 已登录边缘管理系统的Web界面。
- 边缘管理系统中无一般、严重或紧急告警。

**操作步骤<a name="section186434362417"></a>**

请参见[系统升级](./usage.md#系统升级)章节进行操作。

**升级后检查<a name="section1092321532713"></a>**

登录边缘管理系统的Web界面，在主菜单中选择“管理 \> 系统信息”，查看当前固件版本是否为升级后版本。

### 通过FusionDirector升级<a name="ZH-CN_TOPIC_0000001584618736"></a>

**升级前准备<a name="section3824145117349"></a>**

- 已购买FusionDirector管理软件。
- 已登录FusionDirector  Web界面。
- FusionDirector系统中无一般、严重或紧急告警。

**操作步骤<a name="section169701747193914"></a>**

整体升级的操作指导请参见《[FusionDirector 操作指南](https://support.huawei.com/enterprise/zh/doc/EDOC1100317179/426cffd9?idPath=23710424|251364417|251364851|252309137|23015464)》中的“升级管理 \> 固件/驱动升级”章节。

> [!NOTE] 说明  
> 通过FusionDirector升级固件的流程中包含固件包的下载。固件包的下载支持断点续传功能，即下载过程中如果出现网络中断或异常，会导致下载失败。如果用户再次执行升级操作，则固件包会从上次下载失败的位置继续下载，无需重新完整的下载，节省下载时间。

**升级后检查<a name="section1092321532713"></a>**

登录边缘管理系统的Web界面，在主菜单中选择“管理 \> 系统信息”，查看当前固件版本是否为升级后版本。

# 卸载<a id="ZH-CN_TOPIC_0000001628849833"></a>

卸载操作会将OM SDK软件相关文件、用户数据与配置清除。若卸载过程出现卸载失败，可能会导致文件残留。

**通过命令行卸载<a name="section4882716123316"></a>**

1. 以root用户登录边缘设备后台环境。
2. 执行以下命令，进入边缘管理系统软件的所在路径。

    ```bash
    cd /usr/local/mindx/MindXOM
    ```

3. 执行以下命令，卸载边缘管理系统软件。

    ```bash
    ./uninstall.sh
    ```

    回显示例如下，表示卸载成功。

    ```text
    Uninstall MindXOM start.
    ...
    Clear config task success.
    Uninstall MindXOM success.
    ```

    > [!NOTE] 说明  
    > 卸载运行日志可以通过**tail -f /var/plog/upgrade.log**查看。  
    > uninstall.sh命令说明请参见[uninstall.sh命令](./appendix.md#uninstallsh命令)。
