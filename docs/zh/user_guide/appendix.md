# 附录<a name="ZH-CN_TOPIC_0000001578489824"></a>

## 命令参考<a name="ZH-CN_TOPIC_0000001628610453"></a>

### 命令说明<a name="ZH-CN_TOPIC_0000001791845429"></a>

本章节提供对OM SDK的维护命令，具体说明请参见[表1 命令说明](#命令说明table)。

**表 1**  命令说明<a id="命令说明table"></a>

|命令名称|命令功能|操作参考|
|--|--|--|
|manual_kmc_update.sh|用于更新KMC主密钥和根密钥（预留接口）。|具体操作请参见[manual_kmc_update命令](#manual_kmc_update命令)。|
|update_alg.sh|用于配置OM SDK服务进程间通信时使用的加密算法（预留接口）。|具体操作请参见[update_alg命令](#update_alg命令)。|
|om_ability_policy.sh|用于设置能力项开关。|具体操作请参见[om_ability_policy.sh命令](#om_ability_policysh命令)。|
|mount_white_path|用于向挂载路径白名单中导入用户自定义挂载路径。|具体操作请参见[mount_white_path命令](#mount_white_path命令)。|
|certs_manage.sh命令|用于管理相关证书，包括Web证书、FusionDirector证书。|具体操作请参见[certs_manage.sh命令](#certs_managesh命令)。|
|install.sh命令|用于指导用户安装OM SDK软件。|具体操作请参见[install.sh命令](#installsh命令)。|
|uninstall.sh命令|用于指导用户卸载OM SDK软件。|具体操作请参见[uninstall.sh命令](#uninstallsh命令)。|

### manual\_kmc\_update命令<a id="manual_kmc_update命令"></a>

**命令功能<a name="zh-cn_topic_0000001578761946_zh-cn_topic_0000001396921402_section392012387229"></a>**

该命令用于手动更新KMC主密钥和根密钥。

**命令格式<a name="zh-cn_topic_0000001578761946_zh-cn_topic_0000001396921402_section1946125634619"></a>**

**manual\_kmc\_update.sh**

**使用指南<a name="zh-cn_topic_0000001578761946_zh-cn_topic_0000001396921402_section2959155684618"></a>**

当用户需要更新KMC组件加密使用的主密钥、根密钥时，可以使用此命令进行更新。

**使用实例<a name="zh-cn_topic_0000001578761946_zh-cn_topic_0000001396921402_section1421313419471"></a>**

进入“/usr/local/mindx/MindXOM/tools/”路径，执行以下命令更新KMC根密钥、主密钥。

```bash
./manual_kmc_update.sh
```

> [!NOTE] 说明   
> - 密钥更新最小间隔时间为60秒。
> - 若回显中有su: warning: cannot change directory to /home/MindXOM: No such file or directory告警提示，属于正常现象，不影响操作的执行。
> - manual_kmc_update.sh具体功能已消减，属于预留接口，由用户自行实现底层接口具体功能。

### update\_alg命令<a id="update_alg命令"></a>

**命令功能<a name="zh-cn_topic_0000001578921370_zh-cn_topic_0000001549174733_section392012387229"></a>**

该命令用于配置边缘管理系统服务进程间通信时使用的加密算法。

**命令格式<a name="zh-cn_topic_0000001578921370_zh-cn_topic_0000001549174733_section1946125634619"></a>**

**update\_alg.sh -s [sdp\_alg\_id] -h [hmac\_alg\_id\]**

**使用指南<a name="zh-cn_topic_0000001578921370_zh-cn_topic_0000001549174733_section2959155684618"></a>**

加密算法配置有数据加密算法和完整性保护算法两个配置项，如果用户不添加相应配置则使用默认配置。用户可根据实际需求使用该命令进行修改默认配置。

**使用实例<a name="zh-cn_topic_0000001578921370_zh-cn_topic_0000001549174733_section1421313419471"></a>**

1. 执行如下命令，进入命令脚本所在路径。

    ```bash
    cd /usr/local/mindx/MindXOM/tools
    ```

2. 执行如下命令，修改加密算法配置项。

    ```bash
    ./update_alg.sh -s 9 -h 2054
    ```

    > [!NOTE] 说明  
    > 在执行脚本前，请确保指定的目标文件存在且可读写。
    > 该命令将指定配置文件中的sdp\_alg\_id参数的值修改为9，hmac\_alg\_id参数的值修改为2054。

    **表 1**  参数说明

    |参数|说明|
    |--|--|
    |-s|指定数据加密算法，只能为8或9，分别对应AES_GCM_128、AES_GCM_256。|
    |-h|指定完整性保护算法，只能为2053或2054，分别对应SHA384、SHA512。|

> [!NOTE] 说明   
> update_alg.sh具体功能已消减，属于预留接口，由用户自行实现底层接口具体功能。

### om\_ability\_policy.sh命令<a id="om_ability_policysh命令"></a>

**命令功能<a name="zh-cn_topic_0000001495038116_zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section54612047124317"></a>**

该命令用于设置能力项开关。

用户通过Web界面进行操作时，只能使用已经开启的能力项。如果对应的能力项未开启，那么需要通过命令进行添加，并重启系统使服务配置生效。

**能力项列表<a name="zh-cn_topic_0000001495038116_zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section6547202513147"></a>**

容器应用管理能力项。该能力项默认开启，若用户想要进行其他配置，可参见本章节进行操作。

**命令格式<a name="zh-cn_topic_0000001495038116_zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section89802574437"></a>**

1. 执行命令：**cd /usr/local/mindx/MindXOM/tools**进入脚本文件所在路径。
2. 根据需要使用以下命令：

    - **./om\_ability\_policy.sh disable\_all --on**禁止能力项总开关打开，禁止所有能力项。
    - **./om\_ability\_policy.sh allow**  ***op***开启能力项。可选参数请参见[表1 allow子命令参数说明](#allow子命令参数说明)。
    - **./om\_ability\_policy.sh -h**（或--h，-help，--help）显示主命令帮助信息。
    - **./om\_ability\_policy.sh disable\_all -h**（或--help）显示disable\_all子命令帮助信息。
    - **./om\_ability\_policy.sh allow -h**（或--help）显示allow子命令帮助信息。

    **表 1**  allow子命令参数说明<a id="allow子命令参数说明"></a>

   |参数|说明|
   |--|--|
   |--mef_config|开启容器应用管理对接能力项。|

**使用指南<a name="zh-cn_topic_0000001495038116_zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section6323125112440"></a>**

无

**使用实例<a name="zh-cn_topic_0000001495038116_zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section52840564514"></a>**

执行命令：**cd /usr/local/mindx/MindXOM/tools**进入脚本文件所在路径。

- 示例1：关闭所有能力项

    ```text
    $ Euler1:/usr/local/mindx/MindXOM/tools # ./om_ability_policy.sh disable_all --on
    ```

    回显示例如下：

    ```text
    Write to config file success
    {
        "disable_all": true,
        "allow": {
            "mef_config": false
        }
    }
    ```

    执行reboot命令使配置生效。

- 示例2：开启容器应用管理对接能力项

    ```text
    Euler1:/usr/local/mindx/MindXOM/tools # ./om_ability_policy.sh allow --mef_config
    ```

    回显示例如下：

    ```text
    Write to config file success
    {
        "disable_all": false,
        "allow": {
            "mef_config": true
        }
    }
    ```

    执行reboot命令使配置生效。

### mount\_white\_path命令<a id="mount_white_path命令"></a>

**命令功能<a name="zh-cn_topic_0000001507647805_section494485624615"></a>**

该命令用于向挂载路径白名单中导入用户自定义挂载路径。

用户通过Web进行磁盘挂载时，只能将目录挂载到白名单已有的路径中。如果需要的路径不在白名单，那么通过该命令进行添加。

**命令格式<a name="zh-cn_topic_0000001507647805_section1946125634619"></a>**

1. 执行命令：cd /usr/local/mindx/MindXOM/tools/进入脚本文件所在路径。
2. 根据需求使用以下命令。
    - **./mount\_white\_path add** <i>Absolute\_Path</i>  向白名单中添加一个路径。单次最多可添加64个绝对路径到白名单中。添加多条白名单路径时，如果中间存在某条路径添加失败，后续路径不会继续添加。
    - **./mount\_white\_path delete** <i>Absolute\_Path</i>从白名单中删除一个路径，并且单次只能删除一个路径。已挂载路径不允许从白名单中删除。
    - **./mount\_white\_path display** 显示当前白名单所有的路径。
    - **./mount\_white\_path check**  _Absolute\_Path_  检查路径是否在白名单中，并且单次只能检查一个路径。
    - **./mount\_white\_path -h**_，_**--h**_，_**-help**或 **--help**  显示帮助信息。

> [!NOTE] 说明  
> 
>- 白名单中的路径可以直接进行磁盘挂载。
>- 白名单中默认的路径为“/opt/mount/”和“/var/lib/docker”。
>- 白名单中任意两个路径不允许存在父子目录关系。
>- 白名单列表中最大支持64个绝对路径。
>- 白名单路径只能是“/home”和“/opt”目录的子目录或为“/var/lib/docker”，且不能是：“/home/data/cert\_backup”、“/home/data/”、“/home/log/”、“/home/admin/”、“/home/AppUser/”、“/home/MindXOM/”、“/home/MEFEdge/”、“/home/package/”、“/home/HwHiAiUser/”及以上所有路径的子路径。

**参数说明<a name="zh-cn_topic_0000001507647805_section39511656144617"></a>**

|参数|参数说明|取值|
|--|--|--|
|Absolute_Path|白名单的绝对路径|绝对路径信息，路径最大长度为255。|

**使用指南<a name="zh-cn_topic_0000001507647805_section2959155684618"></a>**

无

**使用实例<a name="zh-cn_topic_0000001507647805_section3959115612466"></a>**

执行命令：cd /usr/local/mindx/MindXOM/tools/进入脚本文件所在路径。

- 示例1：添加白名单路径。

    ```text
    Euler:/usr/local/mindx/MindXOM/tools # ./mount_white_path add /home/test/ 
    ```

    回显示例如下：

    ```text
    /home/test/ added successfully
    ```

- 示例2：删除白名单路径。

    ```text
    Euler:/usr/local/mindx/MindXOM/tools # ./mount_white_path delete /home/test/
    ```

    回显示例如下：

    ```text
    /home/test/ deleted successfully
    ```

- 示例3：查看当前白名单中所有的路径。

    ```text
    Euler:/usr/local/mindx/MindXOM/tools # ./mount_white_path display
    ```

    回显示例如下：

    ```text
    /opt/mount/
    ```

- 示例4：检查路径是否在白名单中。

    ```text
    Euler:/usr/local/mindx/MindXOM/tools # ./mount_white_path check /home/test/
    ```

    回显示例如下：

    ```text
    /home/test/ does not exist in the list
    ```

### certs\_manage.sh命令<a id="certs_managesh命令"></a>

**命令功能<a name="zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section54612047124317"></a>**

该命令用于管理相关证书，包括Web证书、FusionDirector证书，可以实现未使用证书的查询和删除；恢复上一份证书。

**命令格式<a name="zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section89802574437"></a>**

**certs\_manage.sh** _[ACTION] [COMPONENT] [OPTION]_

**表 1**  子命令参数说明

|参数|是否可选|说明|
|--|--|--|
|ACTION|必选|对证书的操作类型。可选值如下：<li>getunusedcert：查询未使用证书</li><li>deletecert：删除未使用证书</li><li>restorecert：恢复上一份证书 </li>> [!NOTE] 说明 <ul><li>删除FusionDirector证书时，需要指定删除的证书名，如果上一份证书和当前某个未使用证书同名，优先删除当前未使用证书。</li><li>删除证书时，需要用户输入yes或no，确认是否进行删除操作。</li><li>恢复Web证书，会强制使用上一份证书覆盖当前证书；FusionDirector证书，如果证书正在使用或在当前证书列表中已存在，则恢复失败。</li></ul>|
|COMPONENT|必选|证书类型。可选值如下：<li>fd-ccae：FusionDirector证书或CCAE证书</li><li>web：Web证书</li>|
|OPTION|可选|删除FusionDirector证书时，需要额外指定证书名称。取值为字符串，取值长度为4~64位，支持数字，大小写字母，下划线，点，且不包含“..”。|

**使用指南<a name="zh-cn_topic_0000001389273124_zh-cn_topic_0000001263081386_section6323125112440"></a>**

certs\_manage.sh命令必须使用root账号执行。

**使用实例<a name="section0170143175412"></a>**

1. 执行以下命令，进入脚本文件所在路径。

    ```bash
    cd /usr/local/mindx/MindXOM/tools
    ```

2. 根据实际情况，对证书进行管理，以FusionDirector证书为例。
    - 查询未使用证书。

        ```bash
        ./certs_manage.sh getunusedcert web
        ```

    - 删除未使用证书。

        ```bash
        ./certs_manage.sh deletecert fd-ccae a200_fd.crt
        ```

    - 恢复上一份证书。

        ```bash
        ./certs_manage.sh restorecert fd-ccae
        ```

### install.sh命令<a id="installsh命令"></a>

**命令功能<a name="zh-cn_topic_0000001527210329_zh-cn_topic_0000001472609157_section392012387229"></a>**

该命令用于指导用户安装OM SDK软件。

OM SDK部分进程以非root账号运行，账号名称为MindXOM，默认组ID和用户ID均为1224。当用户组ID或者用户ID被占用时，依次顺延。OM SDK会在安装时创建需要的运行账号。

**命令格式<a name="zh-cn_topic_0000001527210329_zh-cn_topic_0000001472609157_section1946125634619"></a>**

**install\.sh**

**参数说明<a name="zh-cn_topic_0000001527210329_zh-cn_topic_0000001472609157_section39511656144617"></a>**

该命令不需要额外参数

**使用指南<a name="zh-cn_topic_0000001527210329_zh-cn_topic_0000001472609157_section120252875011"></a>**

执行该命令将OM SDK软件安装到“/usr/local/mindx”目录下，并且该目录需要root属组权限。

**使用实例<a name="section13412589527"></a>**

无，请参考[通过命令行安装](./installation_guide.md#通过命令行安装)章节。

### uninstall.sh命令<a id="uninstallsh命令"></a>

**命令功能<a name="zh-cn_topic_0000001476250784_zh-cn_topic_0000001472288941_section392012387229"></a>**

该命令用于指导用户卸载OM SDK软件。

**命令格式<a name="zh-cn_topic_0000001476250784_zh-cn_topic_0000001472288941_section1946125634619"></a>**

**uninstall\.sh**

**参数说明<a name="zh-cn_topic_0000001476250784_zh-cn_topic_0000001472288941_section39511656144617"></a>**

该命令不需要额外参数。

**使用实例<a name="section9690143315315"></a>**

无，请参考[卸载](./installation_guide.md#卸载)章节。

## 公网地址<a name="ZH-CN_TOPIC_0000001586797856"></a>

开源代码包含的公网地址请参考《[OM SDK 公网地址.xlsx](../resource/OM%20SDK%20公网地址.xlsx)》。

## 用户信息列表<a name="ZH-CN_TOPIC_0000001628849881"></a>

**表 1**  用户信息列表

|用户名|描述|初始密码|密码修改方法|
|--|--|--|--|
|root|登录Atlas 200I A2 加速模块用户。|默认密码请参见<a href="https://support.huawei.com/enterprise/zh/doc/EDOC1100235027/13819a2d?idPath=23710424">账户清单</a>中的“Atlas 200I A2 加速模块”章节获取。|以root用户执行passwd \<username> 命令修改。|
|admin|登录边缘管理系统Web界面或命令行。|Edge@12#$|仅限于首次登录时使用该默认密码，成功登录后系统会要求强制修改默认密码。修改密码成功后使用修改后的密码登录。修改密码方式：<li>使用admin用户登录，使用passwd命令修改密码。</li><li>登录边缘管理系统Web界面，点击页面右上角“修改密码”，设置新的用户密码。</li>|
|nobody|匿名账户，nobody账户分派软件进程时不需要任何特殊的权限。|无初始密码。|-|
|MindXOM|边缘管理系统业务及管理进程用户。|无初始密码。|-|

## 缩略语<a name="ZH-CN_TOPIC_0000001578449880"></a>

**表 1**  缩略语

|**缩略语**|**全称**|**含义**|
|--|--|--|
|ISV|Independent Software Vendor|独立软件供应商。|
|DEVM|DeviceManager|设备管理模块，支持二次开发过程中自定义模组。|
|NPU|Neural-network Processing Unit|神经网络处理器。主要是指昇腾AI处理器。|
|PAM|Pluggable Authentication Modules|一种允许系统管理员设置身份验证策略的系统安全工具。|
|TLV|Type-Length-Value|一种简单实用的数据传输方案。|
|KMC|key management CBB|密钥管理组件。实现了密钥的加密保存、加密解密等基本功能。|
