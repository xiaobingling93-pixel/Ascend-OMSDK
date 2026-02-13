# 云边协同接口<a name="ZH-CN_TOPIC_0000001583388006"></a>

## 云边协同接口格式<a name="ZH-CN_TOPIC_0000001577530712"></a>

云边协同消息主要包含接口：Atlas IES与FusionDirector之间通过WebSocket消息通信，分为上行消息与下行消息。

### 上行消息接口格式<a id="上行消息接口格式"></a>

OM向中心网管发送的消息格式定义如下：

```json
{
    "header": {
        "msg_id": "584a2d50-11d1-4d60-83ce-f0cc48a45348",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false,
        "resourceversion": ""
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "query",
        "resource": "websocket/profile_effect"
    },
    "content": {
    }
}
```

各个字段的定义如下：

|属性|类型|解释|
|--|--|--|
|msg_id|string|对该消息的唯一标识，以便消息处理方进行去重、幂等性等操作，格式为UUID。|
|parent_msg_id|string|该消息的父消息，格式为UUID，可以此表示request/response关系。<li>对于请求消息request，该字段为空。</li><li>对于应答消息response，该字段为请求消息的msg_id。</li>|
|timestamp|int|该消息的时间戳。|
|sync|bool|表示同步还是异步消息，取值为：<li>false：表示异步消息，不回应答。默认为false。</li><li>true：表示同步消息，要求边缘回应答。</li>|
|resourceversion|string|空字符串或不传。|
|source|string|消息源，即发送消息的模块，从ESPManager上报的消息，统一填写hardware。<br> [!NOTE] 说明</br>预留字段，取值可为EdgeManager和controller。|
|group|string|消息分类，参考消息分组定义。<br> [!NOTE] 说明</br>预留字段，取值可为resource，twin，hardware，function和user。|
|operation|string|消息操作类型，字符串格式。<br> [!NOTE] 说明</br>预留字段，取值可为insert、update、delete、query和restart。|
|resource|string|资源信息，字符串格式，具体请参见各个接口的具体定义。|
|content|json|具体的消息内容，由各个组件自行定义。|

> [!NOTE] 说明 
> 上行或下行消息中如果某个字段不存在，则表示该字段取其默认值。

消息分组定义如下：

|方向|名称|解释|云侧处理软件|边侧处理组件|
|--|--|--|--|--|
|边-云，云-边|resource|资源的生命周期信息和元数据信息|EdgeManager，DeviceManager|MetaManager，Edged|
|边-云，云-边|hardware|与硬件设备管理平台交互信息|EdgeManager|EventBus|
|边-云，云-边|function|边缘侧函数的状态信息|FunctionManager|MetaManager，Edged|
|边-云，云-边|user|用户自己的数据|用户的应用或其他云服务|EventBus|

### 下行消息接口格式<a name="ZH-CN_TOPIC_0000001577530692"></a>

中心网管下发到OM SDK的消息格式定义如下：

```json
{
    "header": {
        "msg_id": "584a2d50-11d1-4d60-83ce-f0cc48a45348",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "controller",
        "group": "hardware",
        "operation": "update",
        "resource": "profile_effect"
    },
    "content": {
    }
}
```

字段定义请参考[上行消息接口格式](#上行消息接口格式)。

## 上行消息接口<a name="ZH-CN_TOPIC_0000001628610457"></a>

### 注册认证信息<a name="ZH-CN_TOPIC_0000001628490493"></a>

OM向FusionDirector注册认证，携带账号、密码、产品名称、产品序列号、自定义资产标签，新增产品类型，用于区分产品是否支持BMC，是否为服务器形态。

消息头定义如下：

|一级资源名称|描述|类型|取值范围|
|--|--|--|--|
|Authorization|认证信息|string|Basic base64（账号：密码）|
|ProductName|产品名称|string|32字节|
|SerialNumber|产品序列号|string|64字节|
|AssetTag|自定义资产标签|string|256字节|
|DevMgmtType|产品类型|string|32字节，用于区分是否为BMC设备。取值为：<li>BMC：表示BMC管理设备。</li><li>AtlasEdge：表示AtlasEdge管理设备。默认不填AtlasEdge管理设备。</li>|

**响应实例**

```json
{
    "error": {
        "code": "Base.1.0.GeneralError",
        "message": "A general error has occurred. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [{
            "@odata.type": "",
            "MessageId": "FusionDirector.1.0.IAMLocked",
            "RelatedProperties": null,
            "Message": "There are insufficient privileges for the account or credentials associated with the current session to perform the requested operation.",
            "MessageArgs": null,
            "Severity": "Critical",
            "Resolution": "Either abandon the operation or change the associated access rights and resubmit the request if the operation failed."
        }]
    }
}
```

各元素定义如下：

<a name="zh-cn_topic_0000001397081566_table44156300"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001397081566_row40828621"><th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.1"><p id="zh-cn_topic_0000001397081566_p18784032"><a name="zh-cn_topic_0000001397081566_p18784032"></a><a name="zh-cn_topic_0000001397081566_p18784032"></a>一级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.2"><p id="zh-cn_topic_0000001397081566_p45111603"><a name="zh-cn_topic_0000001397081566_p45111603"></a><a name="zh-cn_topic_0000001397081566_p45111603"></a>二级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.3"><p id="zh-cn_topic_0000001397081566_p209911441195516"><a name="zh-cn_topic_0000001397081566_p209911441195516"></a><a name="zh-cn_topic_0000001397081566_p209911441195516"></a>三级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.5.1.4"><p id="zh-cn_topic_0000001397081566_p50692606"><a name="zh-cn_topic_0000001397081566_p50692606"></a><a name="zh-cn_topic_0000001397081566_p50692606"></a>说明</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001397081566_row12553595"><td class="cellrowborder" rowspan="11" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p10208299"><a name="zh-cn_topic_0000001397081566_p10208299"></a><a name="zh-cn_topic_0000001397081566_p10208299"></a>error</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p21565908"><a name="zh-cn_topic_0000001397081566_p21565908"></a><a name="zh-cn_topic_0000001397081566_p21565908"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p099124117555"><a name="zh-cn_topic_0000001397081566_p099124117555"></a><a name="zh-cn_topic_0000001397081566_p099124117555"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001397081566_p7560104251115"><a name="zh-cn_topic_0000001397081566_p7560104251115"></a><a name="zh-cn_topic_0000001397081566_p7560104251115"></a>FusionDirector返回错误</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row61630770"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p2072171411310"><a name="zh-cn_topic_0000001397081566_p2072171411310"></a><a name="zh-cn_topic_0000001397081566_p2072171411310"></a>code</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p4991941135519"><a name="zh-cn_topic_0000001397081566_p4991941135519"></a><a name="zh-cn_topic_0000001397081566_p4991941135519"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p17293132601519"><a name="zh-cn_topic_0000001397081566_p17293132601519"></a><a name="zh-cn_topic_0000001397081566_p17293132601519"></a>含义：错误码</p>
<p id="zh-cn_topic_0000001397081566_p19293426131513"><a name="zh-cn_topic_0000001397081566_p19293426131513"></a><a name="zh-cn_topic_0000001397081566_p19293426131513"></a>类型：string</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row999606"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p28582738"><a name="zh-cn_topic_0000001397081566_p28582738"></a><a name="zh-cn_topic_0000001397081566_p28582738"></a>message</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p0991174110553"><a name="zh-cn_topic_0000001397081566_p0991174110553"></a><a name="zh-cn_topic_0000001397081566_p0991174110553"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p651711275152"><a name="zh-cn_topic_0000001397081566_p651711275152"></a><a name="zh-cn_topic_0000001397081566_p651711275152"></a>含义：错误信息</p>
<p id="zh-cn_topic_0000001397081566_p125179271158"><a name="zh-cn_topic_0000001397081566_p125179271158"></a><a name="zh-cn_topic_0000001397081566_p125179271158"></a>类型：string</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row52686376"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p48856913"><a name="zh-cn_topic_0000001397081566_p48856913"></a><a name="zh-cn_topic_0000001397081566_p48856913"></a>@Message.ExtendedInfo</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p39911941195513"><a name="zh-cn_topic_0000001397081566_p39911941195513"></a><a name="zh-cn_topic_0000001397081566_p39911941195513"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p3637162821516"><a name="zh-cn_topic_0000001397081566_p3637162821516"></a><a name="zh-cn_topic_0000001397081566_p3637162821516"></a>含义：错误信息扩展信息</p>
<p id="zh-cn_topic_0000001397081566_p13637628101516"><a name="zh-cn_topic_0000001397081566_p13637628101516"></a><a name="zh-cn_topic_0000001397081566_p13637628101516"></a>类型：string</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row15771381"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p11948439205614"><a name="zh-cn_topic_0000001397081566_p11948439205614"></a><a name="zh-cn_topic_0000001397081566_p11948439205614"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p14559143120136"><a name="zh-cn_topic_0000001397081566_p14559143120136"></a><a name="zh-cn_topic_0000001397081566_p14559143120136"></a>@odata.type</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p270992914159"><a name="zh-cn_topic_0000001397081566_p270992914159"></a><a name="zh-cn_topic_0000001397081566_p270992914159"></a>此字段不需要解析</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row4999822"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p1194717394564"><a name="zh-cn_topic_0000001397081566_p1194717394564"></a><a name="zh-cn_topic_0000001397081566_p1194717394564"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p4991041165510"><a name="zh-cn_topic_0000001397081566_p4991041165510"></a><a name="zh-cn_topic_0000001397081566_p4991041165510"></a>MessageId</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p1841393111154"><a name="zh-cn_topic_0000001397081566_p1841393111154"></a><a name="zh-cn_topic_0000001397081566_p1841393111154"></a>含义：消息ID</p>
<p id="zh-cn_topic_0000001397081566_p841393121517"><a name="zh-cn_topic_0000001397081566_p841393121517"></a><a name="zh-cn_topic_0000001397081566_p841393121517"></a>类型：string</p>
<p id="zh-cn_topic_0000001397081566_p144131131161511"><a name="zh-cn_topic_0000001397081566_p144131131161511"></a><a name="zh-cn_topic_0000001397081566_p144131131161511"></a>取值：</p>
<a name="zh-cn_topic_0000001397081566_ul33227941815"></a><a name="zh-cn_topic_0000001397081566_ul33227941815"></a><ul id="zh-cn_topic_0000001397081566_ul33227941815"><li>FusionDirector.1.0.IAMLocked：FusionDirector被锁定</li><li>FusionDirector.1.0.SpareNodeIDInCorrect：备件的NodeID不正确</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row46535472"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p594610392567"><a name="zh-cn_topic_0000001397081566_p594610392567"></a><a name="zh-cn_topic_0000001397081566_p594610392567"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p2991941145520"><a name="zh-cn_topic_0000001397081566_p2991941145520"></a><a name="zh-cn_topic_0000001397081566_p2991941145520"></a>RelatedProperties</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p1327194493217"><a name="zh-cn_topic_0000001397081566_p1327194493217"></a><a name="zh-cn_topic_0000001397081566_p1327194493217"></a>此字段不需要解析</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row8378525"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p494514394560"><a name="zh-cn_topic_0000001397081566_p494514394560"></a><a name="zh-cn_topic_0000001397081566_p494514394560"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p1699114119557"><a name="zh-cn_topic_0000001397081566_p1699114119557"></a><a name="zh-cn_topic_0000001397081566_p1699114119557"></a>Message</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p0202163431516"><a name="zh-cn_topic_0000001397081566_p0202163431516"></a><a name="zh-cn_topic_0000001397081566_p0202163431516"></a>含义：消息内容</p>
<p id="zh-cn_topic_0000001397081566_p182021534131520"><a name="zh-cn_topic_0000001397081566_p182021534131520"></a><a name="zh-cn_topic_0000001397081566_p182021534131520"></a>类型：string</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row87422230296"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p194423915611"><a name="zh-cn_topic_0000001397081566_p194423915611"></a><a name="zh-cn_topic_0000001397081566_p194423915611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p179911541185510"><a name="zh-cn_topic_0000001397081566_p179911541185510"></a><a name="zh-cn_topic_0000001397081566_p179911541185510"></a>MessageArgs</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p9280935101516"><a name="zh-cn_topic_0000001397081566_p9280935101516"></a><a name="zh-cn_topic_0000001397081566_p9280935101516"></a>含义：消息参数</p>
<p id="zh-cn_topic_0000001397081566_p142801835161511"><a name="zh-cn_topic_0000001397081566_p142801835161511"></a><a name="zh-cn_topic_0000001397081566_p142801835161511"></a>类型：string</p>
<p id="zh-cn_topic_0000001397081566_p528053521513"><a name="zh-cn_topic_0000001397081566_p528053521513"></a><a name="zh-cn_topic_0000001397081566_p528053521513"></a>取值：如果MessageId取值为FusionDirector.1.0.SpareNodeIDInCorrect，则此字段为NodeID</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row536513211295"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p89431539155614"><a name="zh-cn_topic_0000001397081566_p89431539155614"></a><a name="zh-cn_topic_0000001397081566_p89431539155614"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p17991841165515"><a name="zh-cn_topic_0000001397081566_p17991841165515"></a><a name="zh-cn_topic_0000001397081566_p17991841165515"></a>Severity</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p687714863211"><a name="zh-cn_topic_0000001397081566_p687714863211"></a><a name="zh-cn_topic_0000001397081566_p687714863211"></a>此字段不需要解析</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081566_row108571623161312"><td class="cellrowborder" valign="top" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081566_p8857182312133"><a name="zh-cn_topic_0000001397081566_p8857182312133"></a><a name="zh-cn_topic_0000001397081566_p8857182312133"></a>-</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081566_p11991164195513"><a name="zh-cn_topic_0000001397081566_p11991164195513"></a><a name="zh-cn_topic_0000001397081566_p11991164195513"></a>Resolution</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081566_p118283504324"><a name="zh-cn_topic_0000001397081566_p118283504324"></a><a name="zh-cn_topic_0000001397081566_p118283504324"></a>此字段不需要解析</p>
</td>
</tr>
</tbody>
</table>

### 上报系统静态信息<a id="上报系统静态信息"></a>

系统静态信息在以下情况下会由边缘侧触发：

1. 升级固件等配置动作导致的系统信息变化，及时触发上报一次。
2. 定时每2分钟上报一次。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false,
        "resourceversion": ""
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/sys_info"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下：

```json
{
    "product_info": {
        "manufacturer": "Huawei",
        "product_name": "Atlas 200",
        "support_model": "Atlas 200 I A2",
        "serial_number": "asdf45dsf3sdg",
        "pcb_version": "Ver.C",
        "os_version": "EulerOS V100R001C01",
        "kernel_version": "2.6.3",
        "software_version": "1.0.0",
        "asset_tag": "123456",
        "cpuArchitecture": "ARM"
    },
    "product_capability_om": [
        "profile",
        "Assettag",
        "restart",
        "firmware_install",
        "info_collect",
        "rearm",
        "hostname_config",
        "ntp_server_config",
        "partition_config",
        "static_host_config",
        "name_server_config",
        "nfs_config",
        "net_manager_config",
        "password_config",
        "password_validity_config",
        "configuration_restore",
        "cert_mgmt",
        "lte_config",
        "access_control",
        "session_timeout_config",
        "cert_alarm_time_config",
        "security_load_config"
    ],
    "system": {
        "host_name": "Atlas200",
        "cpu_count": 1,
        "cpu_model": "Hi3559A",
        "memory_size": "4GB",
        "storage_size": "512GB",
        "inactive_firmware": "software",
        "inactive_configuration": "",
        "net_manager_domain": "fd.fusiondirector.huawei.com",
        "net_manager_address": "10.35.22.33",
        "net_manager_account": "edgeAccount"
    },
    "ntp_server": {
        "ntp_info_invalid": false,
        "service_enabled": true,
        "sync_net_manager": false,
        "preferred_server": "xx.xx.xx.xx",
        "alternate_server": "xx.xx.xx.xx"
    },
    "partitions": [{
        "name": "/dev/sda1",
        "capacity_bytes": 134217728,
        "storage_name": "eMMC",
        "storage_device": "/dev/sda",
        "storage_location": "HDD0",
        "file_system": "ext4",
        "mount_path": "/home/data",
        "system_partition_flag": true,
        "logic_name": "sda1"
    }],
    "ai_processors": [{
        "id": 1,
        "manufacturer": "Huawei",
        "model": "Hi1910",
        "calc_ability": "16T",
        "ddr_capacity": "4GB",
        "location": "HDD0"
    }],
    "extended_devices": [{
        "name": "SATADISK1",
        "device_type": "HDD",
        "device_name": "/dev/sda",
        "manufacturer": "Seagate",
        "model": "Hi3559AGMAC",
        "serial_number": "XXXX",
        "firmware_version": "XXXX",
        "location": "HDD0"
    }],
    "ethernet_interfaces": [{
        "id": "GMAC1",
        "name": "eth0",
        "description": "EthernetInterfaceoverWiredNetworkAdapter",
        "permanent_mac": "xx:xx:xx:xx:xx:xx",
        "mac": "xx:xx:xx:xx:xx:xx",
        "interface_enabled": true,
        "ipv4_addresses": [{
            "address": "xx.xx.xx.xx",
            "subnet_mask": "255.255.0.0",
            "gateway": "xx.xx.xx.xx",
            "address_origin": "Static",
            "tag": "Mgmt"
        }],
        "name_servers": ["xx.xx.xx.xx"],
        "location":"PORT2",
        "adapter_type": "GMAC",
        "lte_data_switch": "enable"
    }],
    "simple_storages": [{
        "name": "eMMC1",
        "type": "eMMC",
        "description": "SystemeMMCFlash",
        "devices": [{
            "name": "/dev/sda",
            "manufacturer": "Huawei",
            "model": "3000GT8",
            "capacity_bytes": 32000000000,
            "reserved_bytes": 3200,
            "partition_style": "GPT",
            "location": "HDD0"
        }]
    }],
    "firmware_list": [{
        "name": "MindXOM",
        "version": "1.0.2",
        "inactive_version": "1.0.3",
        "active_method": "inband",
        "board_id": "",
        "upgrade_agent": "OM"
    }],
    "static_host_list": [{
        "ip_address": "xx.xx.xx.xx",
        "name": "fd.huawei.com"
    }],
    "name_server": [{
        "ip_address": "xx.xx.xx.xx"
    }],
    "security_policy": {
        "password_validity": "180",
        "web_access": true,
        "ssh_access": true,
        "session_timeout": 50,
        "cert_alarm_time": 20,
        "security_load": [{
             "enable": "true",
             "start_time": "00:00",
             "end_time": "00:00",
             "ip_addr": "xx.xx.xx.xx",
             "mac_addr": "xx.xx.xx.xx.xx"
        }]
    },
    "accounts": [
        "admin",
        "user"
    ],
    "lte_info": {
    }
}
```

元素定义如下：

<a name="table22383951"></a>
<table><thead align="left"><tr id="row18511700"><th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.1"><p id="p23052717"><a name="p23052717"></a><a name="p23052717"></a>一级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.2"><p id="p55330787"><a name="p55330787"></a><a name="p55330787"></a>二级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.3"><p id="p52608733"><a name="p52608733"></a><a name="p52608733"></a>三级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.5.1.4"><p id="p33448963"><a name="p33448963"></a><a name="p33448963"></a>说明</p>
</th>
</tr>
</thead>
<tbody><tr id="row39375322214"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p393743102216"><a name="p393743102216"></a><a name="p393743102216"></a>product_info</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p19377332219"><a name="p19377332219"></a><a name="p19377332219"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1093773172212"><a name="p1093773172212"></a><a name="p1093773172212"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p9938133192211"><a name="p9938133192211"></a><a name="p9938133192211"></a>制造信息</p>
</td>
</tr>
<tr id="row47934394"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p57480694"><a name="p57480694"></a><a name="p57480694"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p25424598"><a name="p25424598"></a><a name="p25424598"></a>manufacturer</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p46126530"><a name="p46126530"></a><a name="p46126530"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p45261475"><a name="p45261475"></a><a name="p45261475"></a>含义：制造商</p>
<p id="p1755610311371"><a name="p1755610311371"></a><a name="p1755610311371"></a>类型：string</p>
<p id="p151021041153719"><a name="p151021041153719"></a><a name="p151021041153719"></a>取值：32字节</p>
</td>
</tr>
<tr id="row34385794"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p879912594212"><a name="p879912594212"></a><a name="p879912594212"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p52302850"><a name="p52302850"></a><a name="p52302850"></a>product_name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p8672447"><a name="p8672447"></a><a name="p8672447"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p31379588"><a name="p31379588"></a><a name="p31379588"></a>含义：产品名称</p>
<p id="p621213616381"><a name="p621213616381"></a><a name="p621213616381"></a>类型：string</p>
<p id="p9422141843814"><a name="p9422141843814"></a><a name="p9422141843814"></a>取值：64字节</p>
</td>
</tr>
<tr id="row18470102814251"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p11470112882519"><a name="p11470112882519"></a><a name="p11470112882519"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p747019288253"><a name="p747019288253"></a><a name="p747019288253"></a>support_model</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1947015284252"><a name="p1947015284252"></a><a name="p1947015284252"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p7869204372510"><a name="p7869204372510"></a><a name="p7869204372510"></a>含义：产品型号</p>
<p id="p68691343112519"><a name="p68691343112519"></a><a name="p68691343112519"></a>类型：string</p>
<p id="p1386954313251"><a name="p1386954313251"></a><a name="p1386954313251"></a>取值：Atlas 200 I A2</p>
</td>
</tr>
<tr id="row57596560"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p9799185902115"><a name="p9799185902115"></a><a name="p9799185902115"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1022729"><a name="p1022729"></a><a name="p1022729"></a>serial_number</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p15732240"><a name="p15732240"></a><a name="p15732240"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p66351956"><a name="p66351956"></a><a name="p66351956"></a>含义：序列号</p>
<p id="p8139253113810"><a name="p8139253113810"></a><a name="p8139253113810"></a>类型：string</p>
<p id="p181391753173814"><a name="p181391753173814"></a><a name="p181391753173814"></a>取值：64字节</p>
</td>
</tr>
<tr id="row66981435"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p3799205917213"><a name="p3799205917213"></a><a name="p3799205917213"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p36359009"><a name="p36359009"></a><a name="p36359009"></a>pcb_version</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p59398588"><a name="p59398588"></a><a name="p59398588"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p46556328"><a name="p46556328"></a><a name="p46556328"></a>含义：pcb版本</p>
<p id="p29253303917"><a name="p29253303917"></a><a name="p29253303917"></a>类型：string</p>
<p id="p69251313914"><a name="p69251313914"></a><a name="p69251313914"></a>取值：32字节</p>
</td>
</tr>
<tr id="row57134047"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1879925914218"><a name="p1879925914218"></a><a name="p1879925914218"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p53482885"><a name="p53482885"></a><a name="p53482885"></a>os_version</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p37146447"><a name="p37146447"></a><a name="p37146447"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p56072221"><a name="p56072221"></a><a name="p56072221"></a>含义：操作系统版本</p>
<p id="p996861717396"><a name="p996861717396"></a><a name="p996861717396"></a>类型：string</p>
<p id="p4968181710392"><a name="p4968181710392"></a><a name="p4968181710392"></a>取值：64字节</p>
</td>
</tr>
<tr id="row58586146"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1779913591217"><a name="p1779913591217"></a><a name="p1779913591217"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p51240508"><a name="p51240508"></a><a name="p51240508"></a>kernel_version</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p56840478"><a name="p56840478"></a><a name="p56840478"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p40675992"><a name="p40675992"></a><a name="p40675992"></a>含义：内核版本</p>
<p id="p12423153219397"><a name="p12423153219397"></a><a name="p12423153219397"></a>类型：string</p>
<p id="p542373217391"><a name="p542373217391"></a><a name="p542373217391"></a>取值：32字节</p>
</td>
</tr>
<tr id="row50452527"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1679920599217"><a name="p1679920599217"></a><a name="p1679920599217"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p38115282"><a name="p38115282"></a><a name="p38115282"></a>software_version</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p330118"><a name="p330118"></a><a name="p330118"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p26739591"><a name="p26739591"></a><a name="p26739591"></a>含义：软件版本</p>
<p id="p106911445396"><a name="p106911445396"></a><a name="p106911445396"></a>类型：string</p>
<p id="p14691144443913"><a name="p14691144443913"></a><a name="p14691144443913"></a>取值：32字节</p>
</td>
</tr>
<tr id="row8760572"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p379925972111"><a name="p379925972111"></a><a name="p379925972111"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p32931164"><a name="p32931164"></a><a name="p32931164"></a>asset_tag</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p50178604"><a name="p50178604"></a><a name="p50178604"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p37935133"><a name="p37935133"></a><a name="p37935133"></a>含义：自定义电子标签</p>
<p id="p532311545391"><a name="p532311545391"></a><a name="p532311545391"></a>类型：string</p>
<p id="p19324115453919"><a name="p19324115453919"></a><a name="p19324115453919"></a>取值：255字节</p>
</td>
</tr>
<tr id="row3432123911590"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1451565720409"><a name="p1451565720409"></a><a name="p1451565720409"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p151914359917"><a name="p151914359917"></a><a name="p151914359917"></a>cpuArchitecture</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1551519572403"><a name="p1551519572403"></a><a name="p1551519572403"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p123759195295"><a name="p123759195295"></a><a name="p123759195295"></a>含义：CPU架构类型</p>
<p id="p11375101972911"><a name="p11375101972911"></a><a name="p11375101972911"></a>类型：string</p>
<p id="p7347161283013"><a name="p7347161283013"></a><a name="p7347161283013"></a>取值：“x86”或“ARM”</p>
</td>
</tr>
<tr id="row060116013318"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p6585294447"><a name="p6585294447"></a><a name="p6585294447"></a>product_capability_om</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p561234124410"><a name="p561234124410"></a><a name="p561234124410"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1861294118448"><a name="p1861294118448"></a><a name="p1861294118448"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1661244184411"><a name="p1661244184411"></a><a name="p1661244184411"></a>设备能力</p>
</td>
</tr>
<tr id="row54961231631"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p175859964410"><a name="p175859964410"></a><a name="p175859964410"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p17612194110445"><a name="p17612194110445"></a><a name="p17612194110445"></a>profile</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p12612204112440"><a name="p12612204112440"></a><a name="p12612204112440"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1561274111449"><a name="p1561274111449"></a><a name="p1561274111449"></a>含义：配置导入</p>
<p id="p26121341164415"><a name="p26121341164415"></a><a name="p26121341164415"></a>类型：string</p>
</td>
</tr>
<tr id="row14155458316"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17586797446"><a name="p17586797446"></a><a name="p17586797446"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p0613941104418"><a name="p0613941104418"></a><a name="p0613941104418"></a>Assettag</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p116131341184414"><a name="p116131341184414"></a><a name="p116131341184414"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1361364114445"><a name="p1361364114445"></a><a name="p1361364114445"></a>含义：资产标签</p>
<p id="p361316417444"><a name="p361316417444"></a><a name="p361316417444"></a>类型：string</p>
</td>
</tr>
<tr id="row628816912316"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p2058649204412"><a name="p2058649204412"></a><a name="p2058649204412"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1861334144416"><a name="p1861334144416"></a><a name="p1861334144416"></a>restart</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p106135415443"><a name="p106135415443"></a><a name="p106135415443"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p9613641114411"><a name="p9613641114411"></a><a name="p9613641114411"></a>含义：支持系统重启</p>
<p id="p16613641124410"><a name="p16613641124410"></a><a name="p16613641124410"></a>类型：string</p>
</td>
</tr>
<tr id="row2024911111310"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p185864974414"><a name="p185864974414"></a><a name="p185864974414"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p3613641144418"><a name="p3613641144418"></a><a name="p3613641144418"></a>firmware_install</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p12613641174419"><a name="p12613641174419"></a><a name="p12613641174419"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p2061414417448"><a name="p2061414417448"></a><a name="p2061414417448"></a>含义：支持固件升级</p>
<p id="p18614164119445"><a name="p18614164119445"></a><a name="p18614164119445"></a>类型：string</p>
</td>
</tr>
<tr id="row1375201512317"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p2587993441"><a name="p2587993441"></a><a name="p2587993441"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p15614124115447"><a name="p15614124115447"></a><a name="p15614124115447"></a>info_collect</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1761484115446"><a name="p1761484115446"></a><a name="p1761484115446"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p19614164124419"><a name="p19614164124419"></a><a name="p19614164124419"></a>含义：信息收集</p>
<p id="p11614144114411"><a name="p11614144114411"></a><a name="p11614144114411"></a>类型：string</p>
</td>
</tr>
<tr id="row1463319171635"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p185870954413"><a name="p185870954413"></a><a name="p185870954413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1461474113447"><a name="p1461474113447"></a><a name="p1461474113447"></a>rearm</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p9614641164415"><a name="p9614641164415"></a><a name="p9614641164415"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p6614144194410"><a name="p6614144194410"></a><a name="p6614144194410"></a>含义：告警上报</p>
<p id="p11614441184417"><a name="p11614441184417"></a><a name="p11614441184417"></a>类型：string</p>
</td>
</tr>
<tr id="row868911193319"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p358718974411"><a name="p358718974411"></a><a name="p358718974411"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p26151441154419"><a name="p26151441154419"></a><a name="p26151441154419"></a>hostname_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p16615741194418"><a name="p16615741194418"></a><a name="p16615741194418"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p66159412447"><a name="p66159412447"></a><a name="p66159412447"></a>含义：支持配置主机名</p>
<p id="p1361514410448"><a name="p1361514410448"></a><a name="p1361514410448"></a>类型：string</p>
</td>
</tr>
<tr id="row19513221132"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p165883954412"><a name="p165883954412"></a><a name="p165883954412"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p12616174154418"><a name="p12616174154418"></a><a name="p12616174154418"></a>ntp_server_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1061617412443"><a name="p1061617412443"></a><a name="p1061617412443"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p13616164164413"><a name="p13616164164413"></a><a name="p13616164164413"></a>含义：支持NTP服务</p>
<p id="p1616144112446"><a name="p1616144112446"></a><a name="p1616144112446"></a>类型：string</p>
</td>
</tr>
<tr id="row173935231237"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1589159144418"><a name="p1589159144418"></a><a name="p1589159144418"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p18616204115447"><a name="p18616204115447"></a><a name="p18616204115447"></a>partition_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p16616154118448"><a name="p16616154118448"></a><a name="p16616154118448"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p2616164120445"><a name="p2616164120445"></a><a name="p2616164120445"></a>含义：支持磁盘管理</p>
<p id="p196161541174412"><a name="p196161541174412"></a><a name="p196161541174412"></a>类型：string</p>
</td>
</tr>
<tr id="row0737142918310"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p658910964410"><a name="p658910964410"></a><a name="p658910964410"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1061754119446"><a name="p1061754119446"></a><a name="p1061754119446"></a>static_host_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p0617641184417"><a name="p0617641184417"></a><a name="p0617641184417"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p106171441184411"><a name="p106171441184411"></a><a name="p106171441184411"></a>含义：支持本地静态配置</p>
<p id="p1561784110449"><a name="p1561784110449"></a><a name="p1561784110449"></a>类型：string</p>
</td>
</tr>
<tr id="row191218255317"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p145891916445"><a name="p145891916445"></a><a name="p145891916445"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1061714112442"><a name="p1061714112442"></a><a name="p1061714112442"></a>name_server_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p116171341194416"><a name="p116171341194416"></a><a name="p116171341194416"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p186173418442"><a name="p186173418442"></a><a name="p186173418442"></a>含义：支持域名解析</p>
<p id="p10617164164419"><a name="p10617164164419"></a><a name="p10617164164419"></a>类型：string</p>
</td>
</tr>
<tr id="row36574346312"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p158939124419"><a name="p158939124419"></a><a name="p158939124419"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p16170417443"><a name="p16170417443"></a><a name="p16170417443"></a>nfs_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p06171341134414"><a name="p06171341134414"></a><a name="p06171341134414"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p116175415444"><a name="p116175415444"></a><a name="p116175415444"></a>含义：支持NFS配置</p>
<p id="p116171141164420"><a name="p116171141164420"></a><a name="p116171141164420"></a>类型：string</p>
</td>
</tr>
<tr id="row156896361231"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p2590189174413"><a name="p2590189174413"></a><a name="p2590189174413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p15618141104411"><a name="p15618141104411"></a><a name="p15618141104411"></a>net_manager_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p66182041184412"><a name="p66182041184412"></a><a name="p66182041184412"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p26181341114418"><a name="p26181341114418"></a><a name="p26181341114418"></a>含义：支持网管配置</p>
<p id="p461815417445"><a name="p461815417445"></a><a name="p461815417445"></a>类型：string</p>
</td>
</tr>
<tr id="row036117385318"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p55905918448"><a name="p55905918448"></a><a name="p55905918448"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1361818414442"><a name="p1361818414442"></a><a name="p1361818414442"></a>password_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p13618164113443"><a name="p13618164113443"></a><a name="p13618164113443"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p46182413447"><a name="p46182413447"></a><a name="p46182413447"></a>含义：本地密码修改</p>
<p id="p861814413448"><a name="p861814413448"></a><a name="p861814413448"></a>类型：string</p>
</td>
</tr>
<tr id="row9473104011320"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p19590699443"><a name="p19590699443"></a><a name="p19590699443"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p461834111447"><a name="p461834111447"></a><a name="p461834111447"></a>password_validity_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p2618141114414"><a name="p2618141114414"></a><a name="p2618141114414"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p156184417445"><a name="p156184417445"></a><a name="p156184417445"></a>含义：本地密码有效期</p>
<p id="p661884184412"><a name="p661884184412"></a><a name="p661884184412"></a>类型：string</p>
</td>
</tr>
<tr id="row82741944230"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p5590097447"><a name="p5590097447"></a><a name="p5590097447"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1061912414440"><a name="p1061912414440"></a><a name="p1061912414440"></a>configuration_restore</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1161916412440"><a name="p1161916412440"></a><a name="p1161916412440"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p66191541124411"><a name="p66191541124411"></a><a name="p66191541124411"></a>含义：自动备份恢复能力</p>
<p id="p13619154110447"><a name="p13619154110447"></a><a name="p13619154110447"></a>类型：string</p>
</td>
</tr>
<tr id="row91292207504"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p813062011503"><a name="p813062011503"></a><a name="p813062011503"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p513022014507"><a name="p513022014507"></a><a name="p513022014507"></a>cert_mgmt</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p6130132015503"><a name="p6130132015503"></a><a name="p6130132015503"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p18130112011502"><a name="p18130112011502"></a><a name="p18130112011502"></a>含义：支持证书管理</p>
<p id="p927919493503"><a name="p927919493503"></a><a name="p927919493503"></a>类型：string</p>
</td>
</tr>
<tr id="row855931085818"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p124773971213"><a name="p124773971213"></a><a name="p124773971213"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p16478699121"><a name="p16478699121"></a><a name="p16478699121"></a>lte_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p747814912124"><a name="p747814912124"></a><a name="p747814912124"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p11470928201220"><a name="p11470928201220"></a><a name="p11470928201220"></a>含义：支持lte配置</p>
<p id="p247019283126"><a name="p247019283126"></a><a name="p247019283126"></a>类型：string</p>
</td>
</tr>
<tr id="row1257684812119"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p35766487116"><a name="p35766487116"></a><a name="p35766487116"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p16577648514"><a name="p16577648514"></a><a name="p16577648514"></a>access_control</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1757724812113"><a name="p1757724812113"></a><a name="p1757724812113"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1557720487119"><a name="p1557720487119"></a><a name="p1557720487119"></a>含义：支持打开或者关闭近端访问能力</p>
<p id="p14598191234"><a name="p14598191234"></a><a name="p14598191234"></a>类型：string</p>
</td>
</tr>
<tr id="row1220355212113"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p152036528116"><a name="p152036528116"></a><a name="p152036528116"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p142031521715"><a name="p142031521715"></a><a name="p142031521715"></a>session_timeout_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p320315527111"><a name="p320315527111"></a><a name="p320315527111"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p020418523110"><a name="p020418523110"></a><a name="p020418523110"></a>含义：会话超时时间</p>
<p id="p2146154312312"><a name="p2146154312312"></a><a name="p2146154312312"></a>类型：string</p>
</td>
</tr>
<tr id="row882375614118"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1682355612111"><a name="p1682355612111"></a><a name="p1682355612111"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p17823056815"><a name="p17823056815"></a><a name="p17823056815"></a>cert_alarm_time_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p19823456719"><a name="p19823456719"></a><a name="p19823456719"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1823135614117"><a name="p1823135614117"></a><a name="p1823135614117"></a>含义：证书提前告警时间</p>
<p id="p2486767419"><a name="p2486767419"></a><a name="p2486767419"></a>类型：string</p>
</td>
</tr>
<tr id="row127112116210"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p671114112211"><a name="p671114112211"></a><a name="p671114112211"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p6711211213"><a name="p6711211213"></a><a name="p6711211213"></a>security_load_config</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p571110117217"><a name="p571110117217"></a><a name="p571110117217"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p177115115210"><a name="p177115115210"></a><a name="p177115115210"></a>含义：安全登录规则</p>
<p id="p3367015342"><a name="p3367015342"></a><a name="p3367015342"></a>类型：string</p>
</td>
</tr>
<tr id="row182682506225"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p202689501224"><a name="p202689501224"></a><a name="p202689501224"></a>system</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p12681505223"><a name="p12681505223"></a><a name="p12681505223"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p18268165052219"><a name="p18268165052219"></a><a name="p18268165052219"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p16268175062212"><a name="p16268175062212"></a><a name="p16268175062212"></a>系统信息</p>
</td>
</tr>
<tr id="row1379631"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p44641324"><a name="p44641324"></a><a name="p44641324"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p59177481"><a name="p59177481"></a><a name="p59177481"></a>host_name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p28646638"><a name="p28646638"></a><a name="p28646638"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p38676356"><a name="p38676356"></a><a name="p38676356"></a>含义：主机名</p>
<p id="p682819564462"><a name="p682819564462"></a><a name="p682819564462"></a>类型：string</p>
<p id="p5828165616461"><a name="p5828165616461"></a><a name="p5828165616461"></a>取值：64字节</p>
</td>
</tr>
<tr id="row18428913"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1814118341683"><a name="p1814118341683"></a><a name="p1814118341683"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p49034346"><a name="p49034346"></a><a name="p49034346"></a>cpu_count</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p12359118"><a name="p12359118"></a><a name="p12359118"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p61564502"><a name="p61564502"></a><a name="p61564502"></a>含义：CPU个数</p>
<p id="p192721429476"><a name="p192721429476"></a><a name="p192721429476"></a>类型：int32</p>
</td>
</tr>
<tr id="row127884579437"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p278917577432"><a name="p278917577432"></a><a name="p278917577432"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p518665974315"><a name="p518665974315"></a><a name="p518665974315"></a>cpu_model</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p0789145713435"><a name="p0789145713435"></a><a name="p0789145713435"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p57891157134310"><a name="p57891157134310"></a><a name="p57891157134310"></a>含义：CPU型号</p>
<p id="p15971818134714"><a name="p15971818134714"></a><a name="p15971818134714"></a>类型：string</p>
<p id="p1497151824715"><a name="p1497151824715"></a><a name="p1497151824715"></a>取值：32字节</p>
</td>
</tr>
<tr id="row35154584"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p81411342816"><a name="p81411342816"></a><a name="p81411342816"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p63170470"><a name="p63170470"></a><a name="p63170470"></a>memory_size</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p16534450"><a name="p16534450"></a><a name="p16534450"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p64222070"><a name="p64222070"></a><a name="p64222070"></a>含义：系统内存大小</p>
<p id="p1674142284716"><a name="p1674142284716"></a><a name="p1674142284716"></a>类型：string</p>
<p id="p1567472210475"><a name="p1567472210475"></a><a name="p1567472210475"></a>取值：32字节</p>
</td>
</tr>
<tr id="row61375378"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1314114348811"><a name="p1314114348811"></a><a name="p1314114348811"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p30674761"><a name="p30674761"></a><a name="p30674761"></a>storage_size</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1627703"><a name="p1627703"></a><a name="p1627703"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p64735122"><a name="p64735122"></a><a name="p64735122"></a>含义：系统存储大小</p>
<p id="p18379526134715"><a name="p18379526134715"></a><a name="p18379526134715"></a>类型：string</p>
<p id="p3379132618474"><a name="p3379132618474"></a><a name="p3379132618474"></a>取值：32字节</p>
</td>
</tr>
<tr id="row23336725"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p414113341817"><a name="p414113341817"></a><a name="p414113341817"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p36934056"><a name="p36934056"></a><a name="p36934056"></a>inactive_firmware</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p38868546"><a name="p38868546"></a><a name="p38868546"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p61344541"><a name="p61344541"></a><a name="p61344541"></a>含义：未生效的固件名称</p>
<p id="p182022974717"><a name="p182022974717"></a><a name="p182022974717"></a>类型：string</p>
<p id="p1382019291477"><a name="p1382019291477"></a><a name="p1382019291477"></a>取值：32字节</p>
</td>
</tr>
<tr id="row65796188"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p914210342813"><a name="p914210342813"></a><a name="p914210342813"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p44580128"><a name="p44580128"></a><a name="p44580128"></a>inactive_configuration</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p54220609"><a name="p54220609"></a><a name="p54220609"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p29793174"><a name="p29793174"></a><a name="p29793174"></a>含义：未生效的配置文件名称</p>
<p id="p16286135104712"><a name="p16286135104712"></a><a name="p16286135104712"></a>类型：string</p>
<p id="p42864358479"><a name="p42864358479"></a><a name="p42864358479"></a>取值：32字节，如果有多个未生效的配置文件，只会显示其中一个，没有时返回空</p>
</td>
</tr>
<tr id="row05149209113"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p7515720181112"><a name="p7515720181112"></a><a name="p7515720181112"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p195151520141112"><a name="p195151520141112"></a><a name="p195151520141112"></a>net_manager_domain</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p19339239134417"><a name="p19339239134417"></a><a name="p19339239134417"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1651542061112"><a name="p1651542061112"></a><a name="p1651542061112"></a>含义：网管域名</p>
<p id="p1416655914715"><a name="p1416655914715"></a><a name="p1416655914715"></a>类型：string</p>
<p id="p81661359144711"><a name="p81661359144711"></a><a name="p81661359144711"></a>取值：64字节</p>
</td>
</tr>
<tr id="row815522691118"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p115542616118"><a name="p115542616118"></a><a name="p115542616118"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p158751831134418"><a name="p158751831134418"></a><a name="p158751831134418"></a>net_manager_address</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p63391639104413"><a name="p63391639104413"></a><a name="p63391639104413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p10155112613111"><a name="p10155112613111"></a><a name="p10155112613111"></a>含义：网管IP地址</p>
<p id="p143711010489"><a name="p143711010489"></a><a name="p143711010489"></a>类型：string</p>
<p id="p4437210204817"><a name="p4437210204817"></a><a name="p4437210204817"></a>取值：64字节</p>
</td>
</tr>
<tr id="row556433315116"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p9564173317112"><a name="p9564173317112"></a><a name="p9564173317112"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p556493318113"><a name="p556493318113"></a><a name="p556493318113"></a>net_manager_account</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p203401139114418"><a name="p203401139114418"></a><a name="p203401139114418"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p95641033191110"><a name="p95641033191110"></a><a name="p95641033191110"></a>含义：网管账号</p>
<p id="p10347823174816"><a name="p10347823174816"></a><a name="p10347823174816"></a>类型：string</p>
<p id="p163471123124817"><a name="p163471123124817"></a><a name="p163471123124817"></a>取值：32字节</p>
</td>
</tr>
<tr id="row65383662"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p111426341981"><a name="p111426341981"></a><a name="p111426341981"></a>ntp_server</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p22349654"><a name="p22349654"></a><a name="p22349654"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p65491559"><a name="p65491559"></a><a name="p65491559"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p3216064"><a name="p3216064"></a><a name="p3216064"></a>NTP服务器</p>
</td>
</tr>
<tr id="row798713396331"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p098743923316"><a name="p098743923316"></a><a name="p098743923316"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p179870399333"><a name="p179870399333"></a><a name="p179870399333"></a>ntp_info_invalid</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1298711394338"><a name="p1298711394338"></a><a name="p1298711394338"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p6545201213346"><a name="p6545201213346"></a><a name="p6545201213346"></a>含义：NTP服务器信息是否无效</p>
<p id="p1545161218345"><a name="p1545161218345"></a><a name="p1545161218345"></a>类型：bool</p>
<p id="p155451412103418"><a name="p155451412103418"></a><a name="p155451412103418"></a>取值：true或false</p>
<p id="p1595117460346"><a name="p1595117460346"></a><a name="p1595117460346"></a><span id="ph73265248546"><a name="ph73265248546"></a><a name="ph73265248546"></a>FusionDirector</span>默认值为false，表示节点不上报此字段，认为ntp_server整个数据有效。</p>
</td>
</tr>
<tr id="row54441431"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1914212345819"><a name="p1914212345819"></a><a name="p1914212345819"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p36855415"><a name="p36855415"></a><a name="p36855415"></a>service_enabled</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p6428154119912"><a name="p6428154119912"></a><a name="p6428154119912"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p15146986"><a name="p15146986"></a><a name="p15146986"></a>含义：是否使能NTP服务</p>
<p id="p13362122964813"><a name="p13362122964813"></a><a name="p13362122964813"></a>类型：bool</p>
<p id="p1836212296486"><a name="p1836212296486"></a><a name="p1836212296486"></a>取值：true或false</p>
<p id="p119194764818"><a name="p119194764818"></a><a name="p119194764818"></a>当取值为false时，不能配置主备服务器</p>
</td>
</tr>
<tr id="row5778202691014"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p137796266105"><a name="p137796266105"></a><a name="p137796266105"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p37792026111011"><a name="p37792026111011"></a><a name="p37792026111011"></a>sync_net_manager</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p19779132620106"><a name="p19779132620106"></a><a name="p19779132620106"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p577913262101"><a name="p577913262101"></a><a name="p577913262101"></a>含义：网管NTP使能标志</p>
<p id="p393911734915"><a name="p393911734915"></a><a name="p393911734915"></a>类型：bool</p>
<p id="p310120209493"><a name="p310120209493"></a><a name="p310120209493"></a>取值为：true或false</p>
</td>
</tr>
<tr id="row54590188"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p171421534580"><a name="p171421534580"></a><a name="p171421534580"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p6216825"><a name="p6216825"></a><a name="p6216825"></a>preferred_server</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p78401643799"><a name="p78401643799"></a><a name="p78401643799"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p53510952"><a name="p53510952"></a><a name="p53510952"></a>含义：NTP首选服务器</p>
<p id="p312614664911"><a name="p312614664911"></a><a name="p312614664911"></a>类型：string</p>
<p id="p131264464493"><a name="p131264464493"></a><a name="p131264464493"></a>取值：目前仅支持IPv4地址</p>
</td>
</tr>
<tr id="row14493706"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p614215341817"><a name="p614215341817"></a><a name="p614215341817"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p67054679"><a name="p67054679"></a><a name="p67054679"></a>alternate_server</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1051612458917"><a name="p1051612458917"></a><a name="p1051612458917"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p47151767"><a name="p47151767"></a><a name="p47151767"></a>含义：NTP备选服务器</p>
<p id="p2036931115013"><a name="p2036931115013"></a><a name="p2036931115013"></a>类型：string</p>
<p id="p19369101115015"><a name="p19369101115015"></a><a name="p19369101115015"></a>取值：目前仅支持IPv4地址</p>
</td>
</tr>
<tr id="row1724741122310"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p572484113239"><a name="p572484113239"></a><a name="p572484113239"></a>partitions</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1972404114234"><a name="p1972404114234"></a><a name="p1972404114234"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p77246411231"><a name="p77246411231"></a><a name="p77246411231"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p072494132312"><a name="p072494132312"></a><a name="p072494132312"></a>分区信息</p>
</td>
</tr>
<tr id="row17468285"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p5644996"><a name="p5644996"></a><a name="p5644996"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p54591560"><a name="p54591560"></a><a name="p54591560"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p59840240"><a name="p59840240"></a><a name="p59840240"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p15221285"><a name="p15221285"></a><a name="p15221285"></a>含义：物理设备名</p>
<p id="p626761719503"><a name="p626761719503"></a><a name="p626761719503"></a>类型：string</p>
<p id="p2267917115012"><a name="p2267917115012"></a><a name="p2267917115012"></a>取值：32字节，格式为<span class="filepath" id="filepath198421324135019"><a name="filepath198421324135019"></a><a name="filepath198421324135019"></a>“/dev/+设备分区名称”</span></p>
</td>
</tr>
<tr id="row12660981"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p193492547233"><a name="p193492547233"></a><a name="p193492547233"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p55032508"><a name="p55032508"></a><a name="p55032508"></a>capacity_bytes</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p28448160"><a name="p28448160"></a><a name="p28448160"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p22599653"><a name="p22599653"></a><a name="p22599653"></a>含义：分区总大小</p>
<p id="p1670183515010"><a name="p1670183515010"></a><a name="p1670183515010"></a>类型：int64</p>
<p id="p176701735175018"><a name="p176701735175018"></a><a name="p176701735175018"></a>取值：不超过磁盘最大剩余空间，必须是MB的整数</p>
</td>
</tr>
<tr id="row27194681"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p153498542232"><a name="p153498542232"></a><a name="p153498542232"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p48946257"><a name="p48946257"></a><a name="p48946257"></a>storage_name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p5223884"><a name="p5223884"></a><a name="p5223884"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p20481435"><a name="p20481435"></a><a name="p20481435"></a>含义：存储介质名称</p>
<p id="p1724715165118"><a name="p1724715165118"></a><a name="p1724715165118"></a>类型：string</p>
<p id="p6248156517"><a name="p6248156517"></a><a name="p6248156517"></a>取值：32字节。如果是简单存储，填对应存储介质类型；如果是卷，填volume</p>
</td>
</tr>
<tr id="row39437584"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1735017548237"><a name="p1735017548237"></a><a name="p1735017548237"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p45320684"><a name="p45320684"></a><a name="p45320684"></a>storage_device</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p47096807"><a name="p47096807"></a><a name="p47096807"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p56745035"><a name="p56745035"></a><a name="p56745035"></a>含义：存储设备</p>
<p id="p17173725119"><a name="p17173725119"></a><a name="p17173725119"></a>类型：string</p>
<p id="p2710377513"><a name="p2710377513"></a><a name="p2710377513"></a>取值：32字节，如果介质类型是volume，与storage_location保持一致</p>
</td>
</tr>
<tr id="row59103942"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p193507542238"><a name="p193507542238"></a><a name="p193507542238"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p25948923"><a name="p25948923"></a><a name="p25948923"></a>storage_location</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p21487984"><a name="p21487984"></a><a name="p21487984"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p62805121"><a name="p62805121"></a><a name="p62805121"></a>含义：存储设备位置</p>
<p id="p69631257145115"><a name="p69631257145115"></a><a name="p69631257145115"></a>类型：string</p>
<p id="p17963657145113"><a name="p17963657145113"></a><a name="p17963657145113"></a>取值：32字节</p>
</td>
</tr>
<tr id="row9599344"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p63504545235"><a name="p63504545235"></a><a name="p63504545235"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p33182897"><a name="p33182897"></a><a name="p33182897"></a>file_system</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p3460138"><a name="p3460138"></a><a name="p3460138"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p11835796"><a name="p11835796"></a><a name="p11835796"></a>含义：文件系统格式</p>
<p id="p1511251717521"><a name="p1511251717521"></a><a name="p1511251717521"></a>类型：string</p>
<p id="p5112717185210"><a name="p5112717185210"></a><a name="p5112717185210"></a>取值：当前只支持ext4</p>
</td>
</tr>
<tr id="row20257323"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p63509545232"><a name="p63509545232"></a><a name="p63509545232"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p32747341"><a name="p32747341"></a><a name="p32747341"></a>mount_path</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p35288932"><a name="p35288932"></a><a name="p35288932"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p39831264"><a name="p39831264"></a><a name="p39831264"></a>含义：挂载路径</p>
<p id="p99245299526"><a name="p99245299526"></a><a name="p99245299526"></a>类型：string</p>
<p id="p109241429105215"><a name="p109241429105215"></a><a name="p109241429105215"></a>取值：256字节</p>
</td>
</tr>
<tr id="row32004956"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p63501954142320"><a name="p63501954142320"></a><a name="p63501954142320"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p884050"><a name="p884050"></a><a name="p884050"></a>system_partition_flag</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p4499250"><a name="p4499250"></a><a name="p4499250"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p28894987"><a name="p28894987"></a><a name="p28894987"></a>含义：系统分区标志</p>
<p id="p56221142145217"><a name="p56221142145217"></a><a name="p56221142145217"></a>类型：bool</p>
<p id="p1262384235212"><a name="p1262384235212"></a><a name="p1262384235212"></a>取值：true或false</p>
</td>
</tr>
<tr id="row10457183215720"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p5113639417"><a name="p5113639417"></a><a name="p5113639417"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p15113113164115"><a name="p15113113164115"></a><a name="p15113113164115"></a>logic_name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p61136314415"><a name="p61136314415"></a><a name="p61136314415"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p13113936413"><a name="p13113936413"></a><a name="p13113936413"></a>含义：逻辑分区名</p>
<p id="p3225112244116"><a name="p3225112244116"></a><a name="p3225112244116"></a>类型：string</p>
</td>
</tr>
<tr id="row124951435102417"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p049617353241"><a name="p049617353241"></a><a name="p049617353241"></a>ai_processors</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p249623552414"><a name="p249623552414"></a><a name="p249623552414"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1496193515242"><a name="p1496193515242"></a><a name="p1496193515242"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p184961435122411"><a name="p184961435122411"></a><a name="p184961435122411"></a>AI处理器信息</p>
</td>
</tr>
<tr id="row57400477"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p2208450"><a name="p2208450"></a><a name="p2208450"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p56695448"><a name="p56695448"></a><a name="p56695448"></a>id</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p28928582"><a name="p28928582"></a><a name="p28928582"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p61513845"><a name="p61513845"></a><a name="p61513845"></a>含义：芯片编号</p>
<p id="p861420835319"><a name="p861420835319"></a><a name="p861420835319"></a>类型：int</p>
<p id="p1261411810535"><a name="p1261411810535"></a><a name="p1261411810535"></a>取值：当前只有一颗芯片，默认为0</p>
</td>
</tr>
<tr id="row63822110"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p7493349132411"><a name="p7493349132411"></a><a name="p7493349132411"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p44666762"><a name="p44666762"></a><a name="p44666762"></a>manufacturer</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p61237983"><a name="p61237983"></a><a name="p61237983"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p61329576"><a name="p61329576"></a><a name="p61329576"></a>含义：制造商</p>
<p id="p613713214538"><a name="p613713214538"></a><a name="p613713214538"></a>类型：string</p>
<p id="p1413773218530"><a name="p1413773218530"></a><a name="p1413773218530"></a>取值：32字节</p>
</td>
</tr>
<tr id="row54532986"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p6493114962415"><a name="p6493114962415"></a><a name="p6493114962415"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p33570762"><a name="p33570762"></a><a name="p33570762"></a>model</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p34877231"><a name="p34877231"></a><a name="p34877231"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p6483485"><a name="p6483485"></a><a name="p6483485"></a>含义：芯片型号</p>
<p id="p16526135017535"><a name="p16526135017535"></a><a name="p16526135017535"></a>类型：string</p>
<p id="p105261950135312"><a name="p105261950135312"></a><a name="p105261950135312"></a>取值：32字节</p>
</td>
</tr>
<tr id="row54355773"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p749394942419"><a name="p749394942419"></a><a name="p749394942419"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p11725830"><a name="p11725830"></a><a name="p11725830"></a>calc_ability</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p10268149"><a name="p10268149"></a><a name="p10268149"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p26413729"><a name="p26413729"></a><a name="p26413729"></a>含义：算力</p>
<p id="p1917856165311"><a name="p1917856165311"></a><a name="p1917856165311"></a>类型：string</p>
<p id="p139173566538"><a name="p139173566538"></a><a name="p139173566538"></a>取值：32字节，当前有16T和22T两种规格</p>
</td>
</tr>
<tr id="row27175612"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p74931049132419"><a name="p74931049132419"></a><a name="p74931049132419"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p58049713"><a name="p58049713"></a><a name="p58049713"></a>ddr_capacity</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p4406325"><a name="p4406325"></a><a name="p4406325"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p21368008"><a name="p21368008"></a><a name="p21368008"></a>含义：DDR内存大小</p>
<p id="p165155307546"><a name="p165155307546"></a><a name="p165155307546"></a>类型：string</p>
<p id="p15161830155415"><a name="p15161830155415"></a><a name="p15161830155415"></a>取值：32字节，当前有4G和8G两种规格</p>
</td>
</tr>
<tr id="row14164047121220"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1816434711213"><a name="p1816434711213"></a><a name="p1816434711213"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p13165204721216"><a name="p13165204721216"></a><a name="p13165204721216"></a>location</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1616519476125"><a name="p1616519476125"></a><a name="p1616519476125"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p10165247171219"><a name="p10165247171219"></a><a name="p10165247171219"></a>含义：AI处理器位置信息</p>
<p id="p207371627195514"><a name="p207371627195514"></a><a name="p207371627195514"></a>类型：string</p>
<p id="p87384275557"><a name="p87384275557"></a><a name="p87384275557"></a>取值：32字节</p>
</td>
</tr>
<tr id="row186261612511"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p962121612512"><a name="p962121612512"></a><a name="p962121612512"></a>extended_devices</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1621516152518"><a name="p1621516152518"></a><a name="p1621516152518"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p76241632510"><a name="p76241632510"></a><a name="p76241632510"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p146291652510"><a name="p146291652510"></a><a name="p146291652510"></a>外部设备</p>
</td>
</tr>
<tr id="row27777450"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p35380987"><a name="p35380987"></a><a name="p35380987"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p47287700"><a name="p47287700"></a><a name="p47287700"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p5098526"><a name="p5098526"></a><a name="p5098526"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p10327489"><a name="p10327489"></a><a name="p10327489"></a>含义：设备命名</p>
<p id="p1377134011556"><a name="p1377134011556"></a><a name="p1377134011556"></a>类型：string</p>
<p id="p137764065510"><a name="p137764065510"></a><a name="p137764065510"></a>取值：32字节</p>
</td>
</tr>
<tr id="row9664747"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p41021830122512"><a name="p41021830122512"></a><a name="p41021830122512"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p59643614"><a name="p59643614"></a><a name="p59643614"></a>device_type</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p66403450"><a name="p66403450"></a><a name="p66403450"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p9970403"><a name="p9970403"></a><a name="p9970403"></a>含义：设备类型</p>
<p id="p49661546125518"><a name="p49661546125518"></a><a name="p49661546125518"></a>类型：string</p>
</td>
</tr>
<tr id="row63405069"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p171021630162514"><a name="p171021630162514"></a><a name="p171021630162514"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p59921212"><a name="p59921212"></a><a name="p59921212"></a>device_name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p21780021"><a name="p21780021"></a><a name="p21780021"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p19351246"><a name="p19351246"></a><a name="p19351246"></a>含义：系统设备名</p>
<p id="p184111835564"><a name="p184111835564"></a><a name="p184111835564"></a>类型：string</p>
<p id="p14113315563"><a name="p14113315563"></a><a name="p14113315563"></a>取值：32字节，格式为<span class="filepath" id="filepath122763915567"><a name="filepath122763915567"></a><a name="filepath122763915567"></a>“/dev/+设备名称”</span>，如<span class="filepath" id="filepath102762099566"><a name="filepath102762099566"></a><a name="filepath102762099566"></a>“/dev/sd1”</span></p>
</td>
</tr>
<tr id="row9098763"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p12102530182515"><a name="p12102530182515"></a><a name="p12102530182515"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p37210315"><a name="p37210315"></a><a name="p37210315"></a>manufacturer</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p61245562"><a name="p61245562"></a><a name="p61245562"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p61943485"><a name="p61943485"></a><a name="p61943485"></a>含义：生产厂商</p>
<p id="p1474522216563"><a name="p1474522216563"></a><a name="p1474522216563"></a>类型：string</p>
<p id="p1745102219566"><a name="p1745102219566"></a><a name="p1745102219566"></a>取值：32字节</p>
</td>
</tr>
<tr id="row66466348"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p171021230112511"><a name="p171021230112511"></a><a name="p171021230112511"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p12315376"><a name="p12315376"></a><a name="p12315376"></a>model</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p58021439"><a name="p58021439"></a><a name="p58021439"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p2116151"><a name="p2116151"></a><a name="p2116151"></a>含义：设备型号</p>
<p id="p11885129175619"><a name="p11885129175619"></a><a name="p11885129175619"></a>类型：string</p>
<p id="p288511292562"><a name="p288511292562"></a><a name="p288511292562"></a>取值：32字节</p>
</td>
</tr>
<tr id="row67022140"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p910263062520"><a name="p910263062520"></a><a name="p910263062520"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p34986905"><a name="p34986905"></a><a name="p34986905"></a>serial_number</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p15367042"><a name="p15367042"></a><a name="p15367042"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p36770888"><a name="p36770888"></a><a name="p36770888"></a>含义：序列号</p>
<p id="p95428332569"><a name="p95428332569"></a><a name="p95428332569"></a>类型：string</p>
<p id="p85422033155615"><a name="p85422033155615"></a><a name="p85422033155615"></a>取值：64字节</p>
</td>
</tr>
<tr id="row43987152"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p510214301253"><a name="p510214301253"></a><a name="p510214301253"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p31592057"><a name="p31592057"></a><a name="p31592057"></a>firmware_version</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p8819847"><a name="p8819847"></a><a name="p8819847"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p43318981"><a name="p43318981"></a><a name="p43318981"></a>含义：固件版本号</p>
<p id="p201143482562"><a name="p201143482562"></a><a name="p201143482562"></a>类型：string</p>
<p id="p71144482567"><a name="p71144482567"></a><a name="p71144482567"></a>取值：64字节</p>
</td>
</tr>
<tr id="row21053573"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p11031030112512"><a name="p11031030112512"></a><a name="p11031030112512"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p22453839"><a name="p22453839"></a><a name="p22453839"></a>location</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p6821659"><a name="p6821659"></a><a name="p6821659"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p15683467"><a name="p15683467"></a><a name="p15683467"></a>含义：物理位置</p>
<p id="p121701352115615"><a name="p121701352115615"></a><a name="p121701352115615"></a>类型：string</p>
<p id="p151701352195615"><a name="p151701352195615"></a><a name="p151701352195615"></a>取值：32字节</p>
</td>
</tr>
<tr id="row023535712250"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1623525716251"><a name="p1623525716251"></a><a name="p1623525716251"></a>ethernet_interfaces</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1235105752519"><a name="p1235105752519"></a><a name="p1235105752519"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1223505715251"><a name="p1223505715251"></a><a name="p1223505715251"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1323510575259"><a name="p1323510575259"></a><a name="p1323510575259"></a>以太接口信息</p>
</td>
</tr>
<tr id="row45417071"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1254134102310"><a name="p1254134102310"></a><a name="p1254134102310"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p18047093"><a name="p18047093"></a><a name="p18047093"></a>id</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p52528460"><a name="p52528460"></a><a name="p52528460"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p26946830"><a name="p26946830"></a><a name="p26946830"></a>含义：网口ID</p>
<p id="p18539111414573"><a name="p18539111414573"></a><a name="p18539111414573"></a>类型：string</p>
<p id="p85391114145713"><a name="p85391114145713"></a><a name="p85391114145713"></a>取值：</p>
<p id="p1434082019573"><a name="p1434082019573"></a><a name="p1434082019573"></a>32字节，编号规则为<span class="uicontrol" id="uicontrol7340182010574"><a name="uicontrol7340182010574"></a><a name="uicontrol7340182010574"></a>“AdapterType+数字”</span>（从1开始），AdapterType有以下三种：</p>
<a name="ul18340112055717"></a><a name="ul18340112055717"></a><ul id="ul18340112055717"><li>GMAC：以太网口</li><li>Wifi</li><li>LTE</li></ul>
</td>
</tr>
<tr id="row32239941"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p18935111144613"><a name="p18935111144613"></a><a name="p18935111144613"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p66225832"><a name="p66225832"></a><a name="p66225832"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p62692203"><a name="p62692203"></a><a name="p62692203"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p44903662"><a name="p44903662"></a><a name="p44903662"></a>含义：网口名称</p>
<p id="p6609143117577"><a name="p6609143117577"></a><a name="p6609143117577"></a>类型：string</p>
<p id="p76091231125719"><a name="p76091231125719"></a><a name="p76091231125719"></a>取值：最大32字节。</p>
<a name="ul9912133714577"></a><a name="ul9912133714577"></a><ul id="ul9912133714577"><li>普通网口命名为：<span class="uicontrol" id="uicontrol1191223735715"><a name="uicontrol1191223735715"></a><a name="uicontrol1191223735715"></a>“eth+数字”</span></li><li>LTE命名为：<span class="uicontrol" id="uicontrol18912183711578"><a name="uicontrol18912183711578"></a><a name="uicontrol18912183711578"></a>“wwan+数字”</span></li><li>Wifi命名为：<span class="uicontrol" id="uicontrol891316371572"><a name="uicontrol891316371572"></a><a name="uicontrol891316371572"></a>“wlan+数字”</span></li></ul>
</td>
</tr>
<tr id="row45129217"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p15935191164613"><a name="p15935191164613"></a><a name="p15935191164613"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p8485396"><a name="p8485396"></a><a name="p8485396"></a>description</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p16228509"><a name="p16228509"></a><a name="p16228509"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p39440858"><a name="p39440858"></a><a name="p39440858"></a>含义：网口描述信息</p>
<p id="p10646649175710"><a name="p10646649175710"></a><a name="p10646649175710"></a>类型：string</p>
<p id="p116463498576"><a name="p116463498576"></a><a name="p116463498576"></a>取值：最大长度256字节</p>
</td>
</tr>
<tr id="row64359206"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p179353110462"><a name="p179353110462"></a><a name="p179353110462"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p11783519"><a name="p11783519"></a><a name="p11783519"></a>permanent_mac</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p14940952"><a name="p14940952"></a><a name="p14940952"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p2257561"><a name="p2257561"></a><a name="p2257561"></a>含义：生产持久化烧写的永久MAC地址</p>
<p id="p14986182587"><a name="p14986182587"></a><a name="p14986182587"></a>类型：string</p>
<p id="p39871584586"><a name="p39871584586"></a><a name="p39871584586"></a>取值：18字节</p>
</td>
</tr>
<tr id="row28540611"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1093531113466"><a name="p1093531113466"></a><a name="p1093531113466"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p21222324"><a name="p21222324"></a><a name="p21222324"></a>mac</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p41286679"><a name="p41286679"></a><a name="p41286679"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p55886711"><a name="p55886711"></a><a name="p55886711"></a>含义：用户实际使用的MAC地址</p>
<p id="p1319152215587"><a name="p1319152215587"></a><a name="p1319152215587"></a>类型：string</p>
<p id="p10191112275810"><a name="p10191112275810"></a><a name="p10191112275810"></a>取值：18字节</p>
</td>
</tr>
<tr id="row43134380"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1993511118468"><a name="p1993511118468"></a><a name="p1993511118468"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p6591497"><a name="p6591497"></a><a name="p6591497"></a>interface_enabled</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p64149281"><a name="p64149281"></a><a name="p64149281"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p28709257"><a name="p28709257"></a><a name="p28709257"></a>含义：网口使能标志</p>
<p id="p53971628155813"><a name="p53971628155813"></a><a name="p53971628155813"></a>类型：bool</p>
<p id="p1539722855810"><a name="p1539722855810"></a><a name="p1539722855810"></a>取值：true或false</p>
</td>
</tr>
<tr id="row15915198"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p793551111466"><a name="p793551111466"></a><a name="p793551111466"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p65334876"><a name="p65334876"></a><a name="p65334876"></a>ipv4_addresses</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p57633602"><a name="p57633602"></a><a name="p57633602"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p37810156"><a name="p37810156"></a><a name="p37810156"></a>含义：IPv4地址</p>
<p id="p11291185375816"><a name="p11291185375816"></a><a name="p11291185375816"></a>类型：list</p>
<p id="p1429211536585"><a name="p1429211536585"></a><a name="p1429211536585"></a>取值：最大支持16个</p>
</td>
</tr>
<tr id="row7121950"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p15936211164619"><a name="p15936211164619"></a><a name="p15936211164619"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p19350257"><a name="p19350257"></a><a name="p19350257"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p23866959"><a name="p23866959"></a><a name="p23866959"></a>address</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p54175493"><a name="p54175493"></a><a name="p54175493"></a>含义：IP地址</p>
<p id="p9253181810591"><a name="p9253181810591"></a><a name="p9253181810591"></a>类型：string</p>
<p id="p152531718135917"><a name="p152531718135917"></a><a name="p152531718135917"></a>取值：IPv4地址</p>
</td>
</tr>
<tr id="row63385722"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1936311204611"><a name="p1936311204611"></a><a name="p1936311204611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p93077"><a name="p93077"></a><a name="p93077"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p7539237"><a name="p7539237"></a><a name="p7539237"></a>subnet_mask</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p6698433"><a name="p6698433"></a><a name="p6698433"></a>含义：子网掩码</p>
<p id="p26909340598"><a name="p26909340598"></a><a name="p26909340598"></a>类型：string</p>
<p id="p96905343593"><a name="p96905343593"></a><a name="p96905343593"></a>取值：IPv4地址</p>
</td>
</tr>
<tr id="row63282757"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p5936191174618"><a name="p5936191174618"></a><a name="p5936191174618"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p62740188"><a name="p62740188"></a><a name="p62740188"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p48790505"><a name="p48790505"></a><a name="p48790505"></a>gateway</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p59716822"><a name="p59716822"></a><a name="p59716822"></a>含义：网关</p>
<p id="p517143935917"><a name="p517143935917"></a><a name="p517143935917"></a>类型：string</p>
<p id="p4171113985912"><a name="p4171113985912"></a><a name="p4171113985912"></a>取值：IPv4地址</p>
</td>
</tr>
<tr id="row50488062"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p19936141154614"><a name="p19936141154614"></a><a name="p19936141154614"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p2823526"><a name="p2823526"></a><a name="p2823526"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p27379016"><a name="p27379016"></a><a name="p27379016"></a>address_origin</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p3107839"><a name="p3107839"></a><a name="p3107839"></a>含义：地址来源</p>
<p id="p574624411597"><a name="p574624411597"></a><a name="p574624411597"></a>类型：string</p>
<p id="p2074694455919"><a name="p2074694455919"></a><a name="p2074694455919"></a>取值：Static或DHCP</p>
</td>
</tr>
<tr id="row39200872"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1893617116461"><a name="p1893617116461"></a><a name="p1893617116461"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p35760405"><a name="p35760405"></a><a name="p35760405"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p10911672"><a name="p10911672"></a><a name="p10911672"></a>tag</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p11430211"><a name="p11430211"></a><a name="p11430211"></a>含义：IP地址标签</p>
<p id="p1870365819594"><a name="p1870365819594"></a><a name="p1870365819594"></a>类型：string</p>
<p id="p117033586599"><a name="p117033586599"></a><a name="p117033586599"></a>取值：最大32字节</p>
</td>
</tr>
<tr id="row28716498"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p129361911134611"><a name="p129361911134611"></a><a name="p129361911134611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p34367063"><a name="p34367063"></a><a name="p34367063"></a>name_servers</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p32268724"><a name="p32268724"></a><a name="p32268724"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p63629870"><a name="p63629870"></a><a name="p63629870"></a>含义：域名服务器地址</p>
<p id="p15677111514018"><a name="p15677111514018"></a><a name="p15677111514018"></a>类型：string</p>
<p id="p146789151308"><a name="p146789151308"></a><a name="p146789151308"></a>取值：IPv4地址，最多配置两个</p>
</td>
</tr>
<tr id="row56247120"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p9936191117465"><a name="p9936191117465"></a><a name="p9936191117465"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p5715810"><a name="p5715810"></a><a name="p5715810"></a>adapter_type</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p60327427"><a name="p60327427"></a><a name="p60327427"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p54683413"><a name="p54683413"></a><a name="p54683413"></a>含义：网络接入方式</p>
<p id="p7142632909"><a name="p7142632909"></a><a name="p7142632909"></a>类型：string</p>
<p id="p7142143215012"><a name="p7142143215012"></a><a name="p7142143215012"></a>取值：GMAC、Wifi和LTE</p>
</td>
</tr>
<tr id="row57882367"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p493661164616"><a name="p493661164616"></a><a name="p493661164616"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p64262513"><a name="p64262513"></a><a name="p64262513"></a>lte_data_switch</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p37881073"><a name="p37881073"></a><a name="p37881073"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p48468053"><a name="p48468053"></a><a name="p48468053"></a>含义：LTE数据开关</p>
<p id="p752810120118"><a name="p752810120118"></a><a name="p752810120118"></a>类型：string</p>
<p id="p1152814117119"><a name="p1152814117119"></a><a name="p1152814117119"></a>取值：enable或disable。非LTE网口时为空</p>
</td>
</tr>
<tr id="row159610271881"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p13653172216192"><a name="p13653172216192"></a><a name="p13653172216192"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p2653722131912"><a name="p2653722131912"></a><a name="p2653722131912"></a>location</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1765332241910"><a name="p1765332241910"></a><a name="p1765332241910"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p365362212199"><a name="p365362212199"></a><a name="p365362212199"></a>含义：物理位置</p>
<p id="p8183104852017"><a name="p8183104852017"></a><a name="p8183104852017"></a>类型：string</p>
<p id="p1242082810214"><a name="p1242082810214"></a><a name="p1242082810214"></a>取值：最大32字节</p>
</td>
</tr>
<tr id="row6753195717465"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p11753135710464"><a name="p11753135710464"></a><a name="p11753135710464"></a>simple_storages</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p13753125744617"><a name="p13753125744617"></a><a name="p13753125744617"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p67531574466"><a name="p67531574466"></a><a name="p67531574466"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p6753357194613"><a name="p6753357194613"></a><a name="p6753357194613"></a>简单存储信息</p>
</td>
</tr>
<tr id="row8951053"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p53946678"><a name="p53946678"></a><a name="p53946678"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p7604796"><a name="p7604796"></a><a name="p7604796"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p12008710"><a name="p12008710"></a><a name="p12008710"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p33181436"><a name="p33181436"></a><a name="p33181436"></a>含义：名称</p>
<p id="p1522142918412"><a name="p1522142918412"></a><a name="p1522142918412"></a>类型：string</p>
<p id="p1952219294418"><a name="p1952219294418"></a><a name="p1952219294418"></a>取值：最大32字节</p>
</td>
</tr>
<tr id="row20276493"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1571615911475"><a name="p1571615911475"></a><a name="p1571615911475"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p24307161"><a name="p24307161"></a><a name="p24307161"></a>type</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p22722991"><a name="p22722991"></a><a name="p22722991"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p28622982"><a name="p28622982"></a><a name="p28622982"></a>含义：存储介质类型</p>
<p id="p15988431746"><a name="p15988431746"></a><a name="p15988431746"></a>类型：string</p>
<p id="p89813435413"><a name="p89813435413"></a><a name="p89813435413"></a>取值：最大32字节</p>
</td>
</tr>
<tr id="row21747459"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p207161097472"><a name="p207161097472"></a><a name="p207161097472"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p11635903"><a name="p11635903"></a><a name="p11635903"></a>description</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p2984059"><a name="p2984059"></a><a name="p2984059"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p40382252"><a name="p40382252"></a><a name="p40382252"></a>含义：描述信息</p>
<p id="p2268258742"><a name="p2268258742"></a><a name="p2268258742"></a>类型：string</p>
<p id="p126818582418"><a name="p126818582418"></a><a name="p126818582418"></a>取值：最大256字节</p>
</td>
</tr>
<tr id="row19500019"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1771720910475"><a name="p1771720910475"></a><a name="p1771720910475"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p30130195"><a name="p30130195"></a><a name="p30130195"></a>devices</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p24626720"><a name="p24626720"></a><a name="p24626720"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p48607285"><a name="p48607285"></a><a name="p48607285"></a>含义：存储设备</p>
<p id="p1831833732215"><a name="p1831833732215"></a><a name="p1831833732215"></a>类型：list</p>
</td>
</tr>
<tr id="row32607337"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p67170924714"><a name="p67170924714"></a><a name="p67170924714"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p60794242"><a name="p60794242"></a><a name="p60794242"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p25386585"><a name="p25386585"></a><a name="p25386585"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p43047533"><a name="p43047533"></a><a name="p43047533"></a>含义：系统设备名，以<span class="filepath" id="filepath15456165520482"><a name="filepath15456165520482"></a><a name="filepath15456165520482"></a>“/dev/”</span>开始，表示系统设备</p>
<p id="p29084171454"><a name="p29084171454"></a><a name="p29084171454"></a>类型：string</p>
<p id="p19908417458"><a name="p19908417458"></a><a name="p19908417458"></a>取值：32字节</p>
</td>
</tr>
<tr id="row31364028"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1171710974713"><a name="p1171710974713"></a><a name="p1171710974713"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p23616130"><a name="p23616130"></a><a name="p23616130"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p33858349"><a name="p33858349"></a><a name="p33858349"></a>manufacturer</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p58171753"><a name="p58171753"></a><a name="p58171753"></a>含义：生产厂商</p>
<p id="p36817391264"><a name="p36817391264"></a><a name="p36817391264"></a>类型：string</p>
<p id="p26812391262"><a name="p26812391262"></a><a name="p26812391262"></a>取值：32字节</p>
</td>
</tr>
<tr id="row16665928"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1371710944716"><a name="p1371710944716"></a><a name="p1371710944716"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p24820110"><a name="p24820110"></a><a name="p24820110"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p64271912"><a name="p64271912"></a><a name="p64271912"></a>model</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p38642374"><a name="p38642374"></a><a name="p38642374"></a>含义：设备型号</p>
<p id="p10961355065"><a name="p10961355065"></a><a name="p10961355065"></a>类型：string</p>
<p id="p1896115551664"><a name="p1896115551664"></a><a name="p1896115551664"></a>取值：32字节</p>
</td>
</tr>
<tr id="row25088947"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p071759174718"><a name="p071759174718"></a><a name="p071759174718"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p57647469"><a name="p57647469"></a><a name="p57647469"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p38933407"><a name="p38933407"></a><a name="p38933407"></a>capacity_bytes</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p66598249"><a name="p66598249"></a><a name="p66598249"></a>含义：存储空间总大小</p>
<p id="p1613981371"><a name="p1613981371"></a><a name="p1613981371"></a>类型：int64</p>
</td>
</tr>
<tr id="row47727144"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p871710915479"><a name="p871710915479"></a><a name="p871710915479"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p7835243"><a name="p7835243"></a><a name="p7835243"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p30674927"><a name="p30674927"></a><a name="p30674927"></a>reserved_bytes</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1641152"><a name="p1641152"></a><a name="p1641152"></a>含义：系统保留空间大小</p>
<p id="p37286168711"><a name="p37286168711"></a><a name="p37286168711"></a>类型：int64</p>
<p id="p9728111618715"><a name="p9728111618715"></a><a name="p9728111618715"></a>取值：小于capacity_bytes的值</p>
</td>
</tr>
<tr id="row3188637"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17717209154712"><a name="p17717209154712"></a><a name="p17717209154712"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p49796509"><a name="p49796509"></a><a name="p49796509"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p6985405"><a name="p6985405"></a><a name="p6985405"></a>partition_style</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p28946942"><a name="p28946942"></a><a name="p28946942"></a>含义：分区方式</p>
<p id="p288317386714"><a name="p288317386714"></a><a name="p288317386714"></a>类型：string</p>
<p id="p7883193811712"><a name="p7883193811712"></a><a name="p7883193811712"></a>取值：32字节</p>
</td>
</tr>
<tr id="row25242640"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p471710914478"><a name="p471710914478"></a><a name="p471710914478"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p59396640"><a name="p59396640"></a><a name="p59396640"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p46398518"><a name="p46398518"></a><a name="p46398518"></a>location</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p183650"><a name="p183650"></a><a name="p183650"></a>含义：设备物理位置</p>
<p id="p893144217714"><a name="p893144217714"></a><a name="p893144217714"></a>类型：string</p>
<p id="p13931242873"><a name="p13931242873"></a><a name="p13931242873"></a>取值：256字节</p>
</td>
</tr>
<tr id="row352834764718"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p452811474476"><a name="p452811474476"></a><a name="p452811474476"></a>firmware_list</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p135281647144716"><a name="p135281647144716"></a><a name="p135281647144716"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p9528184715471"><a name="p9528184715471"></a><a name="p9528184715471"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p4528164754713"><a name="p4528164754713"></a><a name="p4528164754713"></a>可升级固件列表</p>
</td>
</tr>
<tr id="row36356336"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p59182141"><a name="p59182141"></a><a name="p59182141"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p29024106"><a name="p29024106"></a><a name="p29024106"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p2142367"><a name="p2142367"></a><a name="p2142367"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p39314053"><a name="p39314053"></a><a name="p39314053"></a>含义：固件名称</p>
<p id="p128036471874"><a name="p128036471874"></a><a name="p128036471874"></a>类型：string</p>
<p id="p4803144719710"><a name="p4803144719710"></a><a name="p4803144719710"></a>取值：32字节</p>
</td>
</tr>
<tr id="row25726362"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1967287154814"><a name="p1967287154814"></a><a name="p1967287154814"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p11868339"><a name="p11868339"></a><a name="p11868339"></a>version</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p21811431"><a name="p21811431"></a><a name="p21811431"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p21895487"><a name="p21895487"></a><a name="p21895487"></a>含义：当前运行版本</p>
<p id="p9702651570"><a name="p9702651570"></a><a name="p9702651570"></a>类型：string</p>
<p id="p570375115714"><a name="p570375115714"></a><a name="p570375115714"></a>取值：32字节</p>
</td>
</tr>
<tr id="row54385769"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p13672678482"><a name="p13672678482"></a><a name="p13672678482"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p7202071"><a name="p7202071"></a><a name="p7202071"></a>inactive_version</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p46496883"><a name="p46496883"></a><a name="p46496883"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p8151175"><a name="p8151175"></a><a name="p8151175"></a>含义：未生效的固件版本</p>
<p id="p16771555872"><a name="p16771555872"></a><a name="p16771555872"></a>类型：string</p>
<p id="p187735519710"><a name="p187735519710"></a><a name="p187735519710"></a>取值：256字节</p>
</td>
</tr>
<tr id="row14010827"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p367210724818"><a name="p367210724818"></a><a name="p367210724818"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p53004834"><a name="p53004834"></a><a name="p53004834"></a>active_method</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p65533152"><a name="p65533152"></a><a name="p65533152"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p6585099"><a name="p6585099"></a><a name="p6585099"></a>含义：生效方法</p>
<p id="p289391089"><a name="p289391089"></a><a name="p289391089"></a>类型：string</p>
<p id="p989191820"><a name="p989191820"></a><a name="p989191820"></a>取值：inband、outband或者auto</p>
</td>
</tr>
<tr id="row12527142493615"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17528172419369"><a name="p17528172419369"></a><a name="p17528172419369"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p165287245367"><a name="p165287245367"></a><a name="p165287245367"></a>board_id</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p165285242364"><a name="p165285242364"></a><a name="p165285242364"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p15210433367"><a name="p15210433367"></a><a name="p15210433367"></a>含义：板号</p>
<p id="p82115435362"><a name="p82115435362"></a><a name="p82115435362"></a>类型：string</p>
<p id="p5229431367"><a name="p5229431367"></a><a name="p5229431367"></a>取值：256字节</p>
</td>
</tr>
<tr id="row760035423619"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p76001654123619"><a name="p76001654123619"></a><a name="p76001654123619"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p3600135416369"><a name="p3600135416369"></a><a name="p3600135416369"></a>upgrade_agent</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p56001954173613"><a name="p56001954173613"></a><a name="p56001954173613"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p141001853711"><a name="p141001853711"></a><a name="p141001853711"></a>含义：升级代理</p>
<p id="p310048123711"><a name="p310048123711"></a><a name="p310048123711"></a>类型：string</p>
<p id="p121004818375"><a name="p121004818375"></a><a name="p121004818375"></a>取值：OM</p>
</td>
</tr>
<tr id="row17189174611183"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p21901946191819"><a name="p21901946191819"></a><a name="p21901946191819"></a>static_host_list</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1419015461183"><a name="p1419015461183"></a><a name="p1419015461183"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p219014468184"><a name="p219014468184"></a><a name="p219014468184"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p519054616186"><a name="p519054616186"></a><a name="p519054616186"></a>含义：主机名查询静态表</p>
<p id="p12888105018161"><a name="p12888105018161"></a><a name="p12888105018161"></a>取值：最大支持配置128条，由<span id="ph994511350131"><a name="ph994511350131"></a><a name="ph994511350131"></a>FusionDirector</span>保证，底层不做限制；仅包含用户配置的结果。不上报的信息包括：</p>
<a name="ul16779114161711"></a><a name="ul16779114161711"></a><ul id="ul16779114161711"><li>系统默认自带的<strong id="b13779154161716"><a name="b13779154161716"></a><a name="b13779154161716"></a>localhost</strong>，以及预置的<strong id="b9779144171715"><a name="b9779144171715"></a><a name="b9779144171715"></a>fd.fusiondirector.huawei.com</strong>不上报</li><li>name大于256字节，或包含0-9/a-z/A-Z/./-之外字符的不合法信息不上报</li></ul>
</td>
</tr>
<tr id="row14354116161920"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p935415614194"><a name="p935415614194"></a><a name="p935415614194"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p135456201920"><a name="p135456201920"></a><a name="p135456201920"></a>ip_address</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p23541862194"><a name="p23541862194"></a><a name="p23541862194"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p035466191916"><a name="p035466191916"></a><a name="p035466191916"></a>含义：静态域名解析IP地址</p>
<p id="p41011169170"><a name="p41011169170"></a><a name="p41011169170"></a>类型：string</p>
<p id="p8101121611718"><a name="p8101121611718"></a><a name="p8101121611718"></a>取值：只支持IPv4</p>
</td>
</tr>
<tr id="row3181717111919"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17181111711193"><a name="p17181111711193"></a><a name="p17181111711193"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1018119172199"><a name="p1018119172199"></a><a name="p1018119172199"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1618117173192"><a name="p1618117173192"></a><a name="p1618117173192"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p121811517161916"><a name="p121811517161916"></a><a name="p121811517161916"></a>含义：静态域名信息</p>
<p id="p684532111712"><a name="p684532111712"></a><a name="p684532111712"></a>类型：string</p>
<p id="p168416326170"><a name="p168416326170"></a><a name="p168416326170"></a>取值：64字节</p>
</td>
</tr>
<tr id="row122394243198"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p3239224121913"><a name="p3239224121913"></a><a name="p3239224121913"></a>name_server</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p5239132416193"><a name="p5239132416193"></a><a name="p5239132416193"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p523920247193"><a name="p523920247193"></a><a name="p523920247193"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p16239424131918"><a name="p16239424131918"></a><a name="p16239424131918"></a>含义：域名服务器</p>
<p id="p8324174814171"><a name="p8324174814171"></a><a name="p8324174814171"></a>取值：最多支持3个</p>
</td>
</tr>
<tr id="row1429125991919"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p16297597199"><a name="p16297597199"></a><a name="p16297597199"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p229115971911"><a name="p229115971911"></a><a name="p229115971911"></a>ip_address</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p152995951912"><a name="p152995951912"></a><a name="p152995951912"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p629559191912"><a name="p629559191912"></a><a name="p629559191912"></a>含义：域名服务器地址</p>
<p id="p73291318151814"><a name="p73291318151814"></a><a name="p73291318151814"></a>类型：string</p>
<p id="p19329161810187"><a name="p19329161810187"></a><a name="p19329161810187"></a>取值：仅支持IPv4地址</p>
</td>
</tr>
<tr id="row7173174615204"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p617415467203"><a name="p617415467203"></a><a name="p617415467203"></a>security_policy</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p101741846162017"><a name="p101741846162017"></a><a name="p101741846162017"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p418325572111"><a name="p418325572111"></a><a name="p418325572111"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p21742469201"><a name="p21742469201"></a><a name="p21742469201"></a>安全策略</p>
</td>
</tr>
<tr id="row148145052013"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p848205062019"><a name="p848205062019"></a><a name="p848205062019"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p04875017202"><a name="p04875017202"></a><a name="p04875017202"></a>password_validity</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p19228135518218"><a name="p19228135518218"></a><a name="p19228135518218"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1548165013201"><a name="p1548165013201"></a><a name="p1548165013201"></a>含义：密码有效期</p>
<p id="p112571718161913"><a name="p112571718161913"></a><a name="p112571718161913"></a>类型：string</p>
<p id="p1510321419196"><a name="p1510321419196"></a><a name="p1510321419196"></a>取值：0~365，0表示永远不过期</p>
</td>
</tr>
<tr id="row13209141512717"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17209181512715"><a name="p17209181512715"></a><a name="p17209181512715"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1820961519710"><a name="p1820961519710"></a><a name="p1820961519710"></a>web_access</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p52095154718"><a name="p52095154718"></a><a name="p52095154718"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p14210815772"><a name="p14210815772"></a><a name="p14210815772"></a>含义：是否打开web近端接入开关</p>
<p id="p171011518111012"><a name="p171011518111012"></a><a name="p171011518111012"></a>类型：bool</p>
<p id="p1051518271108"><a name="p1051518271108"></a><a name="p1051518271108"></a>取值：true或者false</p>
</td>
</tr>
<tr id="row0735191910710"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p8735171919719"><a name="p8735171919719"></a><a name="p8735171919719"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p19736191918719"><a name="p19736191918719"></a><a name="p19736191918719"></a>ssh_access</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1573615191717"><a name="p1573615191717"></a><a name="p1573615191717"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p207365191679"><a name="p207365191679"></a><a name="p207365191679"></a>含义：是否打开ssh近端接入开关</p>
<p id="p281443961010"><a name="p281443961010"></a><a name="p281443961010"></a>类型：bool</p>
<p id="p195981149121012"><a name="p195981149121012"></a><a name="p195981149121012"></a>取值：true或者false</p>
</td>
</tr>
<tr id="row18578122416716"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1157910247718"><a name="p1157910247718"></a><a name="p1157910247718"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p105790244713"><a name="p105790244713"></a><a name="p105790244713"></a>session_timeout</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p165797246711"><a name="p165797246711"></a><a name="p165797246711"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1557914241170"><a name="p1557914241170"></a><a name="p1557914241170"></a>含义：会话超时时间，单位为分钟</p>
<p id="p5933147171110"><a name="p5933147171110"></a><a name="p5933147171110"></a>类型：int</p>
<p id="p12310196112"><a name="p12310196112"></a><a name="p12310196112"></a>取值：5~120</p>
</td>
</tr>
<tr id="row55041829676"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p19504192918719"><a name="p19504192918719"></a><a name="p19504192918719"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p19504729274"><a name="p19504729274"></a><a name="p19504729274"></a>cert_alarm_time</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p05041629277"><a name="p05041629277"></a><a name="p05041629277"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p2050420292078"><a name="p2050420292078"></a><a name="p2050420292078"></a>含义：证书过期提前告警时间，单位为天</p>
<p id="p192988387116"><a name="p192988387116"></a><a name="p192988387116"></a>类型：int</p>
<p id="p1138784818111"><a name="p1138784818111"></a><a name="p1138784818111"></a>取值：7~180</p>
</td>
</tr>
<tr id="row1218118334710"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1018117331877"><a name="p1018117331877"></a><a name="p1018117331877"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p5181143315712"><a name="p5181143315712"></a><a name="p5181143315712"></a>security_load</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1218113315716"><a name="p1218113315716"></a><a name="p1218113315716"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p01816335714"><a name="p01816335714"></a><a name="p01816335714"></a>含义：登录规则</p>
<p id="p12517151571211"><a name="p12517151571211"></a><a name="p12517151571211"></a>类型：list</p>
<p id="p13788833151210"><a name="p13788833151210"></a><a name="p13788833151210"></a>取值：最大支持30个</p>
</td>
</tr>
<tr id="row151245111492"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p1812441113918"><a name="p1812441113918"></a><a name="p1812441113918"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p91245114919"><a name="p91245114919"></a><a name="p91245114919"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p4124211794"><a name="p4124211794"></a><a name="p4124211794"></a>enable</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1312471118918"><a name="p1312471118918"></a><a name="p1312471118918"></a>含义：是否使能本条登录规则黑名单</p>
<p id="p18126944101215"><a name="p18126944101215"></a><a name="p18126944101215"></a>类型：bool</p>
<p id="p197051558151216"><a name="p197051558151216"></a><a name="p197051558151216"></a>取值：true or false</p>
</td>
</tr>
<tr id="row97330151597"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p373320151995"><a name="p373320151995"></a><a name="p373320151995"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p7733915391"><a name="p7733915391"></a><a name="p7733915391"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p173319156911"><a name="p173319156911"></a><a name="p173319156911"></a>start_time</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p97339151293"><a name="p97339151293"></a><a name="p97339151293"></a>含义：登录规则黑名单生效开始时间</p>
<p id="p728177101312"><a name="p728177101312"></a><a name="p728177101312"></a>类型：string</p>
<p id="p18185514111310"><a name="p18185514111310"></a><a name="p18185514111310"></a>取值：合法时间格式</p>
</td>
</tr>
<tr id="row274715206912"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p27479208918"><a name="p27479208918"></a><a name="p27479208918"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1774716206916"><a name="p1774716206916"></a><a name="p1774716206916"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1774715208910"><a name="p1774715208910"></a><a name="p1774715208910"></a>end_time</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1074772010920"><a name="p1074772010920"></a><a name="p1074772010920"></a>含义：登录规则黑名单生效结束时间</p>
<p id="p1924963214131"><a name="p1924963214131"></a><a name="p1924963214131"></a>类型：string</p>
<p id="p3569143941311"><a name="p3569143941311"></a><a name="p3569143941311"></a>取值：合法时间格式</p>
</td>
</tr>
<tr id="row127310241794"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p20273524395"><a name="p20273524395"></a><a name="p20273524395"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p2027318241094"><a name="p2027318241094"></a><a name="p2027318241094"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1227322413912"><a name="p1227322413912"></a><a name="p1227322413912"></a>ip_addr</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1527310241896"><a name="p1527310241896"></a><a name="p1527310241896"></a>含义：禁止登录的IP地址</p>
<p id="p6932195241319"><a name="p6932195241319"></a><a name="p6932195241319"></a>类型：string</p>
<p id="p202451539144"><a name="p202451539144"></a><a name="p202451539144"></a>取值：合法的IPv4地址，可以有xxx.xxx.xxx.xxx或者xxx.xxx.xxx.xxx/mask两种形式，xxx.xxx.xxx.xxx为单个IP地址，xxx.xxx.xxx.xxx/mask为IP地址段</p>
</td>
</tr>
<tr id="row56217581698"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p66219581095"><a name="p66219581095"></a><a name="p66219581095"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1262175817914"><a name="p1262175817914"></a><a name="p1262175817914"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1562115818918"><a name="p1562115818918"></a><a name="p1562115818918"></a>mac_addr</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1621358391"><a name="p1621358391"></a><a name="p1621358391"></a>含义：禁止登录的mac地址</p>
<p id="p16168119111411"><a name="p16168119111411"></a><a name="p16168119111411"></a>类型：string</p>
<p id="p9107102751416"><a name="p9107102751416"></a><a name="p9107102751416"></a>取值：合法的mac地址</p>
</td>
</tr>
<tr id="row161588712119"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p21587752112"><a name="p21587752112"></a><a name="p21587752112"></a>accounts</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p191589752116"><a name="p191589752116"></a><a name="p191589752116"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p132721755112111"><a name="p132721755112111"></a><a name="p132721755112111"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p515817715214"><a name="p515817715214"></a><a name="p515817715214"></a>含义：用户名列表</p>
<p id="p10131122418199"><a name="p10131122418199"></a><a name="p10131122418199"></a>类型：list</p>
<p id="p19131202416199"><a name="p19131202416199"></a><a name="p19131202416199"></a>取值：64字节</p>
</td>
</tr>
</tbody>
</table>

**QoS保障属性<a name="section17448725131619"></a>**

为保障OM的正常使用，避免出现卡顿或者上传、下载任务过慢的问题，网络带宽必须满足基本要求，推荐值如下：

- OM所在边缘设备与FusionDirector之间的带宽 ≥ 50Mbit/s
- 其他网络要求：时延 < 30ms，丢包率 < 3%

### 上报系统状态信息<a name="ZH-CN_TOPIC_0000001628610497"></a>

系统运行时的动态信息，由边缘侧固定1分钟发布一次。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/sys_status"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下：

```json
{
    "system": {
        "temperature": 55,
        "power": "75W",
        "voltage": "38V",
        "cpu_heating": "Stop",
        "disk_heating": "Cooling",
        "usb_hub_heating": "Heating",
        "up_time": "06:56:58 up 6 min",
        "date_time": "{date_time}",
        "time_zone": "+8",
        "cpu_usage": "45%",
        "memory_usage": "30%",
        "health_status":"OK",
        "ha_role": "active | standby", 
        "peer_ha_role": "active | standby", 
        "local_node_health": "normal | abnormal",
        "peer_node_health": "normal | abnormal"
    },
    "eth_statistics": [{
        "id": "WiFi0",
        "link_status": "LinkUp",
        "work_mode": "1000Mbps",
        "statistics": {
            "send_packages": 123456,
            "recv_packages": 123456,
            "error_packages": 123456,
            "drop_packages": 123456
        }
    }],
    "partitions": [{
        "name": "/dev/sda1",
        "free_bytes": 1048576,
        "logic_name": "sda1"
        "health": true
    }],
    "extended_devices": [{
        "name": "disk1",
        "status": {
            "state": "Enabled",
            "health": true
        }
    }],
    "simple_storages": [{
        "name": "eMMC",
        "devices": [{
            "name": "/dev/sda",
            "left_bytes": 32000000000,
            "health": true
        }]
    }],
    "ai_processors": [{
        "id": 1,
        "temperature": 50,
        "health": true,
        "occupancy_rate": {
            "ai_core": "25%",
            "ai_cpu": "30%",
            "ctrl_cpu": "25%",
            "ddr_cap": "50%",
            "ddr_bw": "60%"
        }
    }],
    "lte_info": [{
        "default_getaway": "",
        "lte_enable": true,
        "sim_exist": true,
        "state_data": true,
        "state_lte": true,
        "network_signal_level": 4,
        "network_type": "4G",
        "ip_addr": "xx.xx.xx.xx",
        "apn_info": [{
                "apn_name": "",
                "apn_user": "",
                "auth_type": "",
                "mode_type": "",
        }]
    }]
}
```

元素定义如下：

<a name="zh-cn_topic_0000001447121493_table32238654"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001447121493_row2347941"><th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.1"><p id="zh-cn_topic_0000001447121493_p55965548"><a name="zh-cn_topic_0000001447121493_p55965548"></a><a name="zh-cn_topic_0000001447121493_p55965548"></a>一级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.2"><p id="zh-cn_topic_0000001447121493_p36915568"><a name="zh-cn_topic_0000001447121493_p36915568"></a><a name="zh-cn_topic_0000001447121493_p36915568"></a>二级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.3"><p id="zh-cn_topic_0000001447121493_p37371032"><a name="zh-cn_topic_0000001447121493_p37371032"></a><a name="zh-cn_topic_0000001447121493_p37371032"></a>三级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.5.1.4"><p id="zh-cn_topic_0000001447121493_p7154765"><a name="zh-cn_topic_0000001447121493_p7154765"></a><a name="zh-cn_topic_0000001447121493_p7154765"></a>说明</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001447121493_row952455915144"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p452405981410"><a name="zh-cn_topic_0000001447121493_p452405981410"></a><a name="zh-cn_topic_0000001447121493_p452405981410"></a>system</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p1552435901413"><a name="zh-cn_topic_0000001447121493_p1552435901413"></a><a name="zh-cn_topic_0000001447121493_p1552435901413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p552485961420"><a name="zh-cn_topic_0000001447121493_p552485961420"></a><a name="zh-cn_topic_0000001447121493_p552485961420"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p1524125991419"><a name="zh-cn_topic_0000001447121493_p1524125991419"></a><a name="zh-cn_topic_0000001447121493_p1524125991419"></a>系统动态信息</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row48566241"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p41551467"><a name="zh-cn_topic_0000001447121493_p41551467"></a><a name="zh-cn_topic_0000001447121493_p41551467"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p10225657"><a name="zh-cn_topic_0000001447121493_p10225657"></a><a name="zh-cn_topic_0000001447121493_p10225657"></a>temperature</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p22971892"><a name="zh-cn_topic_0000001447121493_p22971892"></a><a name="zh-cn_topic_0000001447121493_p22971892"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p48783939"><a name="zh-cn_topic_0000001447121493_p48783939"></a><a name="zh-cn_topic_0000001447121493_p48783939"></a>含义：系统温度</p>
<p id="zh-cn_topic_0000001447121493_p1096313116497"><a name="zh-cn_topic_0000001447121493_p1096313116497"></a><a name="zh-cn_topic_0000001447121493_p1096313116497"></a>类型：int</p>
<p id="zh-cn_topic_0000001447121493_p12855398497"><a name="zh-cn_topic_0000001447121493_p12855398497"></a><a name="zh-cn_topic_0000001447121493_p12855398497"></a>取值：单位默认为℃</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row61956088"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p59491429141519"><a name="zh-cn_topic_0000001447121493_p59491429141519"></a><a name="zh-cn_topic_0000001447121493_p59491429141519"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p15506900"><a name="zh-cn_topic_0000001447121493_p15506900"></a><a name="zh-cn_topic_0000001447121493_p15506900"></a>power</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p48099407"><a name="zh-cn_topic_0000001447121493_p48099407"></a><a name="zh-cn_topic_0000001447121493_p48099407"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p3737907"><a name="zh-cn_topic_0000001447121493_p3737907"></a><a name="zh-cn_topic_0000001447121493_p3737907"></a>含义：功率</p>
<p id="zh-cn_topic_0000001447121493_p125390372517"><a name="zh-cn_topic_0000001447121493_p125390372517"></a><a name="zh-cn_topic_0000001447121493_p125390372517"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p05396370518"><a name="zh-cn_topic_0000001447121493_p05396370518"></a><a name="zh-cn_topic_0000001447121493_p05396370518"></a>取值：16字节，如20W</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row65728494"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p8949122912159"><a name="zh-cn_topic_0000001447121493_p8949122912159"></a><a name="zh-cn_topic_0000001447121493_p8949122912159"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p3090723"><a name="zh-cn_topic_0000001447121493_p3090723"></a><a name="zh-cn_topic_0000001447121493_p3090723"></a>voltage</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p49021976"><a name="zh-cn_topic_0000001447121493_p49021976"></a><a name="zh-cn_topic_0000001447121493_p49021976"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p11357097"><a name="zh-cn_topic_0000001447121493_p11357097"></a><a name="zh-cn_topic_0000001447121493_p11357097"></a>含义：电压</p>
<p id="zh-cn_topic_0000001447121493_p1049645318519"><a name="zh-cn_topic_0000001447121493_p1049645318519"></a><a name="zh-cn_topic_0000001447121493_p1049645318519"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p7496165315116"><a name="zh-cn_topic_0000001447121493_p7496165315116"></a><a name="zh-cn_topic_0000001447121493_p7496165315116"></a>取值：16字节，如16V</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row6394584"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p1894913294154"><a name="zh-cn_topic_0000001447121493_p1894913294154"></a><a name="zh-cn_topic_0000001447121493_p1894913294154"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p11826969"><a name="zh-cn_topic_0000001447121493_p11826969"></a><a name="zh-cn_topic_0000001447121493_p11826969"></a>cpu_heating</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p18460434"><a name="zh-cn_topic_0000001447121493_p18460434"></a><a name="zh-cn_topic_0000001447121493_p18460434"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p18900220"><a name="zh-cn_topic_0000001447121493_p18900220"></a><a name="zh-cn_topic_0000001447121493_p18900220"></a>含义：CPU加热状态</p>
<p id="zh-cn_topic_0000001447121493_p322106105217"><a name="zh-cn_topic_0000001447121493_p322106105217"></a><a name="zh-cn_topic_0000001447121493_p322106105217"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p112211617524"><a name="zh-cn_topic_0000001447121493_p112211617524"></a><a name="zh-cn_topic_0000001447121493_p112211617524"></a>取值：Heating、Cooling或Stop</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row18731475"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p1094912941516"><a name="zh-cn_topic_0000001447121493_p1094912941516"></a><a name="zh-cn_topic_0000001447121493_p1094912941516"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p20879349"><a name="zh-cn_topic_0000001447121493_p20879349"></a><a name="zh-cn_topic_0000001447121493_p20879349"></a>disk_heating</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p13505676"><a name="zh-cn_topic_0000001447121493_p13505676"></a><a name="zh-cn_topic_0000001447121493_p13505676"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p20217992"><a name="zh-cn_topic_0000001447121493_p20217992"></a><a name="zh-cn_topic_0000001447121493_p20217992"></a>含义：硬盘加热状态</p>
<p id="zh-cn_topic_0000001447121493_p101241183521"><a name="zh-cn_topic_0000001447121493_p101241183521"></a><a name="zh-cn_topic_0000001447121493_p101241183521"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p3124121816527"><a name="zh-cn_topic_0000001447121493_p3124121816527"></a><a name="zh-cn_topic_0000001447121493_p3124121816527"></a>取值：Heating、Cooling或Stop</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row1853813480189"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p1453804817187"><a name="zh-cn_topic_0000001447121493_p1453804817187"></a><a name="zh-cn_topic_0000001447121493_p1453804817187"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p1253824881816"><a name="zh-cn_topic_0000001447121493_p1253824881816"></a><a name="zh-cn_topic_0000001447121493_p1253824881816"></a>usb_hub_heating</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p45381648151819"><a name="zh-cn_topic_0000001447121493_p45381648151819"></a><a name="zh-cn_topic_0000001447121493_p45381648151819"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p0340102151911"><a name="zh-cn_topic_0000001447121493_p0340102151911"></a><a name="zh-cn_topic_0000001447121493_p0340102151911"></a>含义：USB Hub加热状态</p>
<p id="zh-cn_topic_0000001447121493_p73411021171918"><a name="zh-cn_topic_0000001447121493_p73411021171918"></a><a name="zh-cn_topic_0000001447121493_p73411021171918"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p16341172181916"><a name="zh-cn_topic_0000001447121493_p16341172181916"></a><a name="zh-cn_topic_0000001447121493_p16341172181916"></a>取值：Heating、Cooling或Stop</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row52681540"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p189491929101516"><a name="zh-cn_topic_0000001447121493_p189491929101516"></a><a name="zh-cn_topic_0000001447121493_p189491929101516"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p32937530"><a name="zh-cn_topic_0000001447121493_p32937530"></a><a name="zh-cn_topic_0000001447121493_p32937530"></a>up_time</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p50694298"><a name="zh-cn_topic_0000001447121493_p50694298"></a><a name="zh-cn_topic_0000001447121493_p50694298"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p12597491"><a name="zh-cn_topic_0000001447121493_p12597491"></a><a name="zh-cn_topic_0000001447121493_p12597491"></a>含义：系统运行时间</p>
<p id="zh-cn_topic_0000001447121493_p2348428185217"><a name="zh-cn_topic_0000001447121493_p2348428185217"></a><a name="zh-cn_topic_0000001447121493_p2348428185217"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1534817289524"><a name="zh-cn_topic_0000001447121493_p1534817289524"></a><a name="zh-cn_topic_0000001447121493_p1534817289524"></a>取值：32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row34604056"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p2949112912153"><a name="zh-cn_topic_0000001447121493_p2949112912153"></a><a name="zh-cn_topic_0000001447121493_p2949112912153"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p7929847"><a name="zh-cn_topic_0000001447121493_p7929847"></a><a name="zh-cn_topic_0000001447121493_p7929847"></a>date_time</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p38337881"><a name="zh-cn_topic_0000001447121493_p38337881"></a><a name="zh-cn_topic_0000001447121493_p38337881"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p18360680"><a name="zh-cn_topic_0000001447121493_p18360680"></a><a name="zh-cn_topic_0000001447121493_p18360680"></a>含义：系统当前时间</p>
<p id="zh-cn_topic_0000001447121493_p7900134217521"><a name="zh-cn_topic_0000001447121493_p7900134217521"></a><a name="zh-cn_topic_0000001447121493_p7900134217521"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1590084265218"><a name="zh-cn_topic_0000001447121493_p1590084265218"></a><a name="zh-cn_topic_0000001447121493_p1590084265218"></a>取值：32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row36121349"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p094972971515"><a name="zh-cn_topic_0000001447121493_p094972971515"></a><a name="zh-cn_topic_0000001447121493_p094972971515"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p30773760"><a name="zh-cn_topic_0000001447121493_p30773760"></a><a name="zh-cn_topic_0000001447121493_p30773760"></a>time_zone</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p9646638"><a name="zh-cn_topic_0000001447121493_p9646638"></a><a name="zh-cn_topic_0000001447121493_p9646638"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p43180253"><a name="zh-cn_topic_0000001447121493_p43180253"></a><a name="zh-cn_topic_0000001447121493_p43180253"></a>含义：时区</p>
<p id="zh-cn_topic_0000001447121493_p463619485521"><a name="zh-cn_topic_0000001447121493_p463619485521"></a><a name="zh-cn_topic_0000001447121493_p463619485521"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p063674817522"><a name="zh-cn_topic_0000001447121493_p063674817522"></a><a name="zh-cn_topic_0000001447121493_p063674817522"></a>取值："-12" - "+11"</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row16633286"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p1594962971513"><a name="zh-cn_topic_0000001447121493_p1594962971513"></a><a name="zh-cn_topic_0000001447121493_p1594962971513"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p11982130"><a name="zh-cn_topic_0000001447121493_p11982130"></a><a name="zh-cn_topic_0000001447121493_p11982130"></a>cpu_usage</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p31028487"><a name="zh-cn_topic_0000001447121493_p31028487"></a><a name="zh-cn_topic_0000001447121493_p31028487"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p30279504"><a name="zh-cn_topic_0000001447121493_p30279504"></a><a name="zh-cn_topic_0000001447121493_p30279504"></a>含义：CPU利用率</p>
<p id="zh-cn_topic_0000001447121493_p13935912135310"><a name="zh-cn_topic_0000001447121493_p13935912135310"></a><a name="zh-cn_topic_0000001447121493_p13935912135310"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p159351812115318"><a name="zh-cn_topic_0000001447121493_p159351812115318"></a><a name="zh-cn_topic_0000001447121493_p159351812115318"></a>取值：0% ~ 100%</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row60104802"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p794919299153"><a name="zh-cn_topic_0000001447121493_p794919299153"></a><a name="zh-cn_topic_0000001447121493_p794919299153"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p15924658"><a name="zh-cn_topic_0000001447121493_p15924658"></a><a name="zh-cn_topic_0000001447121493_p15924658"></a>memory_usage</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p14828951"><a name="zh-cn_topic_0000001447121493_p14828951"></a><a name="zh-cn_topic_0000001447121493_p14828951"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p60294405"><a name="zh-cn_topic_0000001447121493_p60294405"></a><a name="zh-cn_topic_0000001447121493_p60294405"></a>含义：内存利用率</p>
<p id="zh-cn_topic_0000001447121493_p18042025145317"><a name="zh-cn_topic_0000001447121493_p18042025145317"></a><a name="zh-cn_topic_0000001447121493_p18042025145317"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p198041325185315"><a name="zh-cn_topic_0000001447121493_p198041325185315"></a><a name="zh-cn_topic_0000001447121493_p198041325185315"></a>取值：0% ~ 100%</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row64878181"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p1949229151518"><a name="zh-cn_topic_0000001447121493_p1949229151518"></a><a name="zh-cn_topic_0000001447121493_p1949229151518"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p61332526"><a name="zh-cn_topic_0000001447121493_p61332526"></a><a name="zh-cn_topic_0000001447121493_p61332526"></a>health_status</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p1878685"><a name="zh-cn_topic_0000001447121493_p1878685"></a><a name="zh-cn_topic_0000001447121493_p1878685"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p17955785"><a name="zh-cn_topic_0000001447121493_p17955785"></a><a name="zh-cn_topic_0000001447121493_p17955785"></a>含义：健康状态</p>
<p id="zh-cn_topic_0000001447121493_p1434183217533"><a name="zh-cn_topic_0000001447121493_p1434183217533"></a><a name="zh-cn_topic_0000001447121493_p1434183217533"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p0434932115311"><a name="zh-cn_topic_0000001447121493_p0434932115311"></a><a name="zh-cn_topic_0000001447121493_p0434932115311"></a>取值：</p>
<a name="zh-cn_topic_0000001447121493_ul7748140195316"></a><a name="zh-cn_topic_0000001447121493_ul7748140195316"></a><ul id="zh-cn_topic_0000001447121493_ul7748140195316"><li>OK：表示当前系统无告警</li><li>Warning/Critical：表示系统当前存在最高级别为Warning/Critical的告警</li><li>Unknown：表示健康状态未知</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row18206244"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p3949202918153"><a name="zh-cn_topic_0000001447121493_p3949202918153"></a><a name="zh-cn_topic_0000001447121493_p3949202918153"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p64499993"><a name="zh-cn_topic_0000001447121493_p64499993"></a><a name="zh-cn_topic_0000001447121493_p64499993"></a>ha_role</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p57116957"><a name="zh-cn_topic_0000001447121493_p57116957"></a><a name="zh-cn_topic_0000001447121493_p57116957"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p63070768"><a name="zh-cn_topic_0000001447121493_p63070768"></a><a name="zh-cn_topic_0000001447121493_p63070768"></a>含义：本端HA角色</p>
<p id="zh-cn_topic_0000001447121493_p247175012536"><a name="zh-cn_topic_0000001447121493_p247175012536"></a><a name="zh-cn_topic_0000001447121493_p247175012536"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p124711950165315"><a name="zh-cn_topic_0000001447121493_p124711950165315"></a><a name="zh-cn_topic_0000001447121493_p124711950165315"></a>取值：只能选择active或standby，当没对接上时，返回为空</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row59428348"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p129491296156"><a name="zh-cn_topic_0000001447121493_p129491296156"></a><a name="zh-cn_topic_0000001447121493_p129491296156"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p6894066"><a name="zh-cn_topic_0000001447121493_p6894066"></a><a name="zh-cn_topic_0000001447121493_p6894066"></a>peer_ha_role</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p21548451"><a name="zh-cn_topic_0000001447121493_p21548451"></a><a name="zh-cn_topic_0000001447121493_p21548451"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p594138"><a name="zh-cn_topic_0000001447121493_p594138"></a><a name="zh-cn_topic_0000001447121493_p594138"></a>含义：对端HA角色</p>
<p id="zh-cn_topic_0000001447121493_p95073415541"><a name="zh-cn_topic_0000001447121493_p95073415541"></a><a name="zh-cn_topic_0000001447121493_p95073415541"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1250717445420"><a name="zh-cn_topic_0000001447121493_p1250717445420"></a><a name="zh-cn_topic_0000001447121493_p1250717445420"></a>取值：只能选择active或standby，当没对接上时，返回为空</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row52429714"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p13949132911159"><a name="zh-cn_topic_0000001447121493_p13949132911159"></a><a name="zh-cn_topic_0000001447121493_p13949132911159"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p58429564"><a name="zh-cn_topic_0000001447121493_p58429564"></a><a name="zh-cn_topic_0000001447121493_p58429564"></a>local_node_health</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p35174278"><a name="zh-cn_topic_0000001447121493_p35174278"></a><a name="zh-cn_topic_0000001447121493_p35174278"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p30544297"><a name="zh-cn_topic_0000001447121493_p30544297"></a><a name="zh-cn_topic_0000001447121493_p30544297"></a>含义：本端节点健康状态</p>
<p id="zh-cn_topic_0000001447121493_p47851221165412"><a name="zh-cn_topic_0000001447121493_p47851221165412"></a><a name="zh-cn_topic_0000001447121493_p47851221165412"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p107851721175418"><a name="zh-cn_topic_0000001447121493_p107851721175418"></a><a name="zh-cn_topic_0000001447121493_p107851721175418"></a>取值：32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row59487507"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p494917290154"><a name="zh-cn_topic_0000001447121493_p494917290154"></a><a name="zh-cn_topic_0000001447121493_p494917290154"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p59495430"><a name="zh-cn_topic_0000001447121493_p59495430"></a><a name="zh-cn_topic_0000001447121493_p59495430"></a>peer_node_health</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p54400546"><a name="zh-cn_topic_0000001447121493_p54400546"></a><a name="zh-cn_topic_0000001447121493_p54400546"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p44368139"><a name="zh-cn_topic_0000001447121493_p44368139"></a><a name="zh-cn_topic_0000001447121493_p44368139"></a>含义：对端节点健康状态</p>
<p id="zh-cn_topic_0000001447121493_p4714103312548"><a name="zh-cn_topic_0000001447121493_p4714103312548"></a><a name="zh-cn_topic_0000001447121493_p4714103312548"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p87141133145417"><a name="zh-cn_topic_0000001447121493_p87141133145417"></a><a name="zh-cn_topic_0000001447121493_p87141133145417"></a>取值：32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row12341721102219"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p734220217223"><a name="zh-cn_topic_0000001447121493_p734220217223"></a><a name="zh-cn_topic_0000001447121493_p734220217223"></a>eth_statistics</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p14342821152211"><a name="zh-cn_topic_0000001447121493_p14342821152211"></a><a name="zh-cn_topic_0000001447121493_p14342821152211"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p11342142132218"><a name="zh-cn_topic_0000001447121493_p11342142132218"></a><a name="zh-cn_topic_0000001447121493_p11342142132218"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p133426216221"><a name="zh-cn_topic_0000001447121493_p133426216221"></a><a name="zh-cn_topic_0000001447121493_p133426216221"></a>以太网口统计信息</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row45704206"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p21755034"><a name="zh-cn_topic_0000001447121493_p21755034"></a><a name="zh-cn_topic_0000001447121493_p21755034"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p22896455"><a name="zh-cn_topic_0000001447121493_p22896455"></a><a name="zh-cn_topic_0000001447121493_p22896455"></a>id</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p42673536"><a name="zh-cn_topic_0000001447121493_p42673536"></a><a name="zh-cn_topic_0000001447121493_p42673536"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p34004353"><a name="zh-cn_topic_0000001447121493_p34004353"></a><a name="zh-cn_topic_0000001447121493_p34004353"></a>含义：以太网口ID</p>
<p id="zh-cn_topic_0000001447121493_p199151928018"><a name="zh-cn_topic_0000001447121493_p199151928018"></a><a name="zh-cn_topic_0000001447121493_p199151928018"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1891513212010"><a name="zh-cn_topic_0000001447121493_p1891513212010"></a><a name="zh-cn_topic_0000001447121493_p1891513212010"></a>取值：GMAC+数字，32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row35972372"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p123334320227"><a name="zh-cn_topic_0000001447121493_p123334320227"></a><a name="zh-cn_topic_0000001447121493_p123334320227"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p59970673"><a name="zh-cn_topic_0000001447121493_p59970673"></a><a name="zh-cn_topic_0000001447121493_p59970673"></a>link_status</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p25786377"><a name="zh-cn_topic_0000001447121493_p25786377"></a><a name="zh-cn_topic_0000001447121493_p25786377"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p8321777"><a name="zh-cn_topic_0000001447121493_p8321777"></a><a name="zh-cn_topic_0000001447121493_p8321777"></a>含义：link状态</p>
<p id="zh-cn_topic_0000001447121493_p18603123315016"><a name="zh-cn_topic_0000001447121493_p18603123315016"></a><a name="zh-cn_topic_0000001447121493_p18603123315016"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p060323311012"><a name="zh-cn_topic_0000001447121493_p060323311012"></a><a name="zh-cn_topic_0000001447121493_p060323311012"></a>取值：LinkUP或LinkDown</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row21519974"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p03331032112219"><a name="zh-cn_topic_0000001447121493_p03331032112219"></a><a name="zh-cn_topic_0000001447121493_p03331032112219"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p62612740"><a name="zh-cn_topic_0000001447121493_p62612740"></a><a name="zh-cn_topic_0000001447121493_p62612740"></a>work_mode</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p38467178"><a name="zh-cn_topic_0000001447121493_p38467178"></a><a name="zh-cn_topic_0000001447121493_p38467178"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p28833684"><a name="zh-cn_topic_0000001447121493_p28833684"></a><a name="zh-cn_topic_0000001447121493_p28833684"></a>含义：工作模式</p>
<p id="zh-cn_topic_0000001447121493_p15950545601"><a name="zh-cn_topic_0000001447121493_p15950545601"></a><a name="zh-cn_topic_0000001447121493_p15950545601"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p895004516019"><a name="zh-cn_topic_0000001447121493_p895004516019"></a><a name="zh-cn_topic_0000001447121493_p895004516019"></a>取值：100Mbps或1000Mbps</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row48343558"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p13333113282219"><a name="zh-cn_topic_0000001447121493_p13333113282219"></a><a name="zh-cn_topic_0000001447121493_p13333113282219"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p25598425"><a name="zh-cn_topic_0000001447121493_p25598425"></a><a name="zh-cn_topic_0000001447121493_p25598425"></a>statistics</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p60206519"><a name="zh-cn_topic_0000001447121493_p60206519"></a><a name="zh-cn_topic_0000001447121493_p60206519"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p44889837"><a name="zh-cn_topic_0000001447121493_p44889837"></a><a name="zh-cn_topic_0000001447121493_p44889837"></a>含义：网口收发包统计信息</p>
<p id="zh-cn_topic_0000001447121493_p96021601216"><a name="zh-cn_topic_0000001447121493_p96021601216"></a><a name="zh-cn_topic_0000001447121493_p96021601216"></a>类型：list</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row34103458"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p4333232162212"><a name="zh-cn_topic_0000001447121493_p4333232162212"></a><a name="zh-cn_topic_0000001447121493_p4333232162212"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p11836605"><a name="zh-cn_topic_0000001447121493_p11836605"></a><a name="zh-cn_topic_0000001447121493_p11836605"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p19240909"><a name="zh-cn_topic_0000001447121493_p19240909"></a><a name="zh-cn_topic_0000001447121493_p19240909"></a>send_packages</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p15009769"><a name="zh-cn_topic_0000001447121493_p15009769"></a><a name="zh-cn_topic_0000001447121493_p15009769"></a>含义：发送报文</p>
<p id="zh-cn_topic_0000001447121493_p199701091611"><a name="zh-cn_topic_0000001447121493_p199701091611"></a><a name="zh-cn_topic_0000001447121493_p199701091611"></a>类型：Int64</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row5130925"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p15333203232220"><a name="zh-cn_topic_0000001447121493_p15333203232220"></a><a name="zh-cn_topic_0000001447121493_p15333203232220"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p42462276"><a name="zh-cn_topic_0000001447121493_p42462276"></a><a name="zh-cn_topic_0000001447121493_p42462276"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p16892369"><a name="zh-cn_topic_0000001447121493_p16892369"></a><a name="zh-cn_topic_0000001447121493_p16892369"></a>recv_packages</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p26104638"><a name="zh-cn_topic_0000001447121493_p26104638"></a><a name="zh-cn_topic_0000001447121493_p26104638"></a>含义：接收报文</p>
<p id="zh-cn_topic_0000001447121493_p13268123116116"><a name="zh-cn_topic_0000001447121493_p13268123116116"></a><a name="zh-cn_topic_0000001447121493_p13268123116116"></a>类型：Int64</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row29287846"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p5333143252216"><a name="zh-cn_topic_0000001447121493_p5333143252216"></a><a name="zh-cn_topic_0000001447121493_p5333143252216"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p24881422"><a name="zh-cn_topic_0000001447121493_p24881422"></a><a name="zh-cn_topic_0000001447121493_p24881422"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p2129264"><a name="zh-cn_topic_0000001447121493_p2129264"></a><a name="zh-cn_topic_0000001447121493_p2129264"></a>error_packages</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p38252711"><a name="zh-cn_topic_0000001447121493_p38252711"></a><a name="zh-cn_topic_0000001447121493_p38252711"></a>含义：错误报文</p>
<p id="zh-cn_topic_0000001447121493_p4682153311118"><a name="zh-cn_topic_0000001447121493_p4682153311118"></a><a name="zh-cn_topic_0000001447121493_p4682153311118"></a>类型：Int64</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row34237264"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p533353213223"><a name="zh-cn_topic_0000001447121493_p533353213223"></a><a name="zh-cn_topic_0000001447121493_p533353213223"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p17327357"><a name="zh-cn_topic_0000001447121493_p17327357"></a><a name="zh-cn_topic_0000001447121493_p17327357"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p61338656"><a name="zh-cn_topic_0000001447121493_p61338656"></a><a name="zh-cn_topic_0000001447121493_p61338656"></a>drop_packages</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p2375211"><a name="zh-cn_topic_0000001447121493_p2375211"></a><a name="zh-cn_topic_0000001447121493_p2375211"></a>含义：丢弃报文</p>
<p id="zh-cn_topic_0000001447121493_p52527361916"><a name="zh-cn_topic_0000001447121493_p52527361916"></a><a name="zh-cn_topic_0000001447121493_p52527361916"></a>类型：Int64</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row45390343241"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p13539203482411"><a name="zh-cn_topic_0000001447121493_p13539203482411"></a><a name="zh-cn_topic_0000001447121493_p13539203482411"></a>partitions</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p1753973412418"><a name="zh-cn_topic_0000001447121493_p1753973412418"></a><a name="zh-cn_topic_0000001447121493_p1753973412418"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p16539123412411"><a name="zh-cn_topic_0000001447121493_p16539123412411"></a><a name="zh-cn_topic_0000001447121493_p16539123412411"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p20539103418241"><a name="zh-cn_topic_0000001447121493_p20539103418241"></a><a name="zh-cn_topic_0000001447121493_p20539103418241"></a>分区信息</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row10519736"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p46792305"><a name="zh-cn_topic_0000001447121493_p46792305"></a><a name="zh-cn_topic_0000001447121493_p46792305"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p32080330"><a name="zh-cn_topic_0000001447121493_p32080330"></a><a name="zh-cn_topic_0000001447121493_p32080330"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p48369899"><a name="zh-cn_topic_0000001447121493_p48369899"></a><a name="zh-cn_topic_0000001447121493_p48369899"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p25647741"><a name="zh-cn_topic_0000001447121493_p25647741"></a><a name="zh-cn_topic_0000001447121493_p25647741"></a>含义：分区设备名称</p>
<p id="zh-cn_topic_0000001447121493_p20402048015"><a name="zh-cn_topic_0000001447121493_p20402048015"></a><a name="zh-cn_topic_0000001447121493_p20402048015"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1341348510"><a name="zh-cn_topic_0000001447121493_p1341348510"></a><a name="zh-cn_topic_0000001447121493_p1341348510"></a>取值：32字节，格式为<span class="filepath" id="zh-cn_topic_0000001447121493_filepath96254811"><a name="zh-cn_topic_0000001447121493_filepath96254811"></a><a name="zh-cn_topic_0000001447121493_filepath96254811"></a>“/dev/+设备名称”</span></p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row27743911"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p292695116244"><a name="zh-cn_topic_0000001447121493_p292695116244"></a><a name="zh-cn_topic_0000001447121493_p292695116244"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p28566041"><a name="zh-cn_topic_0000001447121493_p28566041"></a><a name="zh-cn_topic_0000001447121493_p28566041"></a>free_bytes</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p32147993"><a name="zh-cn_topic_0000001447121493_p32147993"></a><a name="zh-cn_topic_0000001447121493_p32147993"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p53850621"><a name="zh-cn_topic_0000001447121493_p53850621"></a><a name="zh-cn_topic_0000001447121493_p53850621"></a>含义：可用空间大小</p>
<p id="zh-cn_topic_0000001447121493_p843418590113"><a name="zh-cn_topic_0000001447121493_p843418590113"></a><a name="zh-cn_topic_0000001447121493_p843418590113"></a>类型：Int64</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row6040620"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p69261851172413"><a name="zh-cn_topic_0000001447121493_p69261851172413"></a><a name="zh-cn_topic_0000001447121493_p69261851172413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p38284531"><a name="zh-cn_topic_0000001447121493_p38284531"></a><a name="zh-cn_topic_0000001447121493_p38284531"></a>health</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p14039291"><a name="zh-cn_topic_0000001447121493_p14039291"></a><a name="zh-cn_topic_0000001447121493_p14039291"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p63440763"><a name="zh-cn_topic_0000001447121493_p63440763"></a><a name="zh-cn_topic_0000001447121493_p63440763"></a>含义：健康状态</p>
<p id="zh-cn_topic_0000001447121493_p884117151129"><a name="zh-cn_topic_0000001447121493_p884117151129"></a><a name="zh-cn_topic_0000001447121493_p884117151129"></a>类型：bool</p>
<p id="zh-cn_topic_0000001447121493_p198413151623"><a name="zh-cn_topic_0000001447121493_p198413151623"></a><a name="zh-cn_topic_0000001447121493_p198413151623"></a>取值：true或false</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row73451135162817"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p163469054314"><a name="zh-cn_topic_0000001447121493_p163469054314"></a><a name="zh-cn_topic_0000001447121493_p163469054314"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p634618019434"><a name="zh-cn_topic_0000001447121493_p634618019434"></a><a name="zh-cn_topic_0000001447121493_p634618019434"></a>logic_name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p434616016432"><a name="zh-cn_topic_0000001447121493_p434616016432"></a><a name="zh-cn_topic_0000001447121493_p434616016432"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p15346905432"><a name="zh-cn_topic_0000001447121493_p15346905432"></a><a name="zh-cn_topic_0000001447121493_p15346905432"></a>含义：逻辑分区名称</p>
<p id="zh-cn_topic_0000001447121493_p1855806114317"><a name="zh-cn_topic_0000001447121493_p1855806114317"></a><a name="zh-cn_topic_0000001447121493_p1855806114317"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p2554593431"><a name="zh-cn_topic_0000001447121493_p2554593431"></a><a name="zh-cn_topic_0000001447121493_p2554593431"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row1431161118255"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p13119117253"><a name="zh-cn_topic_0000001447121493_p13119117253"></a><a name="zh-cn_topic_0000001447121493_p13119117253"></a>extended_devices</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p17318118258"><a name="zh-cn_topic_0000001447121493_p17318118258"></a><a name="zh-cn_topic_0000001447121493_p17318118258"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p13329113256"><a name="zh-cn_topic_0000001447121493_p13329113256"></a><a name="zh-cn_topic_0000001447121493_p13329113256"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p183291115257"><a name="zh-cn_topic_0000001447121493_p183291115257"></a><a name="zh-cn_topic_0000001447121493_p183291115257"></a>扩展设备状态信息</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row34921333"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p10055747"><a name="zh-cn_topic_0000001447121493_p10055747"></a><a name="zh-cn_topic_0000001447121493_p10055747"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p9209153"><a name="zh-cn_topic_0000001447121493_p9209153"></a><a name="zh-cn_topic_0000001447121493_p9209153"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p7743895"><a name="zh-cn_topic_0000001447121493_p7743895"></a><a name="zh-cn_topic_0000001447121493_p7743895"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p23275758"><a name="zh-cn_topic_0000001447121493_p23275758"></a><a name="zh-cn_topic_0000001447121493_p23275758"></a>含义：设备命名</p>
<p id="zh-cn_topic_0000001447121493_p946115341129"><a name="zh-cn_topic_0000001447121493_p946115341129"></a><a name="zh-cn_topic_0000001447121493_p946115341129"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1646118340210"><a name="zh-cn_topic_0000001447121493_p1646118340210"></a><a name="zh-cn_topic_0000001447121493_p1646118340210"></a>取值：32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row20722603"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p143577236252"><a name="zh-cn_topic_0000001447121493_p143577236252"></a><a name="zh-cn_topic_0000001447121493_p143577236252"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p65550951"><a name="zh-cn_topic_0000001447121493_p65550951"></a><a name="zh-cn_topic_0000001447121493_p65550951"></a>status</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p8026787"><a name="zh-cn_topic_0000001447121493_p8026787"></a><a name="zh-cn_topic_0000001447121493_p8026787"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p46190036"><a name="zh-cn_topic_0000001447121493_p46190036"></a><a name="zh-cn_topic_0000001447121493_p46190036"></a>含义：分区状态</p>
<p id="zh-cn_topic_0000001447121493_p5570052723"><a name="zh-cn_topic_0000001447121493_p5570052723"></a><a name="zh-cn_topic_0000001447121493_p5570052723"></a>类型：list</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row37042395"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p3357112332514"><a name="zh-cn_topic_0000001447121493_p3357112332514"></a><a name="zh-cn_topic_0000001447121493_p3357112332514"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p33957371"><a name="zh-cn_topic_0000001447121493_p33957371"></a><a name="zh-cn_topic_0000001447121493_p33957371"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p66192520"><a name="zh-cn_topic_0000001447121493_p66192520"></a><a name="zh-cn_topic_0000001447121493_p66192520"></a>state</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p59993885"><a name="zh-cn_topic_0000001447121493_p59993885"></a><a name="zh-cn_topic_0000001447121493_p59993885"></a>含义：使能状态</p>
<p id="zh-cn_topic_0000001447121493_p661935831"><a name="zh-cn_topic_0000001447121493_p661935831"></a><a name="zh-cn_topic_0000001447121493_p661935831"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p66191358311"><a name="zh-cn_topic_0000001447121493_p66191358311"></a><a name="zh-cn_topic_0000001447121493_p66191358311"></a>取值：Enabled或Disabled</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row36242321"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p0357923102510"><a name="zh-cn_topic_0000001447121493_p0357923102510"></a><a name="zh-cn_topic_0000001447121493_p0357923102510"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p19168682"><a name="zh-cn_topic_0000001447121493_p19168682"></a><a name="zh-cn_topic_0000001447121493_p19168682"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p9159425"><a name="zh-cn_topic_0000001447121493_p9159425"></a><a name="zh-cn_topic_0000001447121493_p9159425"></a>health</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p3715989"><a name="zh-cn_topic_0000001447121493_p3715989"></a><a name="zh-cn_topic_0000001447121493_p3715989"></a>含义：健康状态</p>
<p id="zh-cn_topic_0000001447121493_p1884265518312"><a name="zh-cn_topic_0000001447121493_p1884265518312"></a><a name="zh-cn_topic_0000001447121493_p1884265518312"></a>类型：bool</p>
<p id="zh-cn_topic_0000001447121493_p138424551733"><a name="zh-cn_topic_0000001447121493_p138424551733"></a><a name="zh-cn_topic_0000001447121493_p138424551733"></a>取值：true或false</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row14685348102517"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p568544812520"><a name="zh-cn_topic_0000001447121493_p568544812520"></a><a name="zh-cn_topic_0000001447121493_p568544812520"></a>simple_storages</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p668524812259"><a name="zh-cn_topic_0000001447121493_p668524812259"></a><a name="zh-cn_topic_0000001447121493_p668524812259"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p11685194832513"><a name="zh-cn_topic_0000001447121493_p11685194832513"></a><a name="zh-cn_topic_0000001447121493_p11685194832513"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p668518480255"><a name="zh-cn_topic_0000001447121493_p668518480255"></a><a name="zh-cn_topic_0000001447121493_p668518480255"></a>简单存储状态信息</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row10716912"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p62763577"><a name="zh-cn_topic_0000001447121493_p62763577"></a><a name="zh-cn_topic_0000001447121493_p62763577"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p50684982"><a name="zh-cn_topic_0000001447121493_p50684982"></a><a name="zh-cn_topic_0000001447121493_p50684982"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p11842896"><a name="zh-cn_topic_0000001447121493_p11842896"></a><a name="zh-cn_topic_0000001447121493_p11842896"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p19750510"><a name="zh-cn_topic_0000001447121493_p19750510"></a><a name="zh-cn_topic_0000001447121493_p19750510"></a>含义：名称</p>
<p id="zh-cn_topic_0000001447121493_p1434220101712"><a name="zh-cn_topic_0000001447121493_p1434220101712"></a><a name="zh-cn_topic_0000001447121493_p1434220101712"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p534215101979"><a name="zh-cn_topic_0000001447121493_p534215101979"></a><a name="zh-cn_topic_0000001447121493_p534215101979"></a>取值：最大32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row30057883"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p220064582815"><a name="zh-cn_topic_0000001447121493_p220064582815"></a><a name="zh-cn_topic_0000001447121493_p220064582815"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p43928661"><a name="zh-cn_topic_0000001447121493_p43928661"></a><a name="zh-cn_topic_0000001447121493_p43928661"></a>devices</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p1451783"><a name="zh-cn_topic_0000001447121493_p1451783"></a><a name="zh-cn_topic_0000001447121493_p1451783"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p50485585"><a name="zh-cn_topic_0000001447121493_p50485585"></a><a name="zh-cn_topic_0000001447121493_p50485585"></a>-</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row13395536"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p2020054552814"><a name="zh-cn_topic_0000001447121493_p2020054552814"></a><a name="zh-cn_topic_0000001447121493_p2020054552814"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p42611603"><a name="zh-cn_topic_0000001447121493_p42611603"></a><a name="zh-cn_topic_0000001447121493_p42611603"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p28987799"><a name="zh-cn_topic_0000001447121493_p28987799"></a><a name="zh-cn_topic_0000001447121493_p28987799"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p66310376"><a name="zh-cn_topic_0000001447121493_p66310376"></a><a name="zh-cn_topic_0000001447121493_p66310376"></a>含义：系统设备名</p>
<p id="zh-cn_topic_0000001447121493_p666317333717"><a name="zh-cn_topic_0000001447121493_p666317333717"></a><a name="zh-cn_topic_0000001447121493_p666317333717"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p156631533874"><a name="zh-cn_topic_0000001447121493_p156631533874"></a><a name="zh-cn_topic_0000001447121493_p156631533874"></a>取值：32字节，以<span class="filepath" id="zh-cn_topic_0000001447121493_filepath113881039878"><a name="zh-cn_topic_0000001447121493_filepath113881039878"></a><a name="zh-cn_topic_0000001447121493_filepath113881039878"></a>“/dev/”</span>开始，表示系统设备</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row27617285"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p14200345152813"><a name="zh-cn_topic_0000001447121493_p14200345152813"></a><a name="zh-cn_topic_0000001447121493_p14200345152813"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p3079941"><a name="zh-cn_topic_0000001447121493_p3079941"></a><a name="zh-cn_topic_0000001447121493_p3079941"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p48148645"><a name="zh-cn_topic_0000001447121493_p48148645"></a><a name="zh-cn_topic_0000001447121493_p48148645"></a>left_bytes</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p7726182"><a name="zh-cn_topic_0000001447121493_p7726182"></a><a name="zh-cn_topic_0000001447121493_p7726182"></a>含义：剩余可用空间</p>
<p id="zh-cn_topic_0000001447121493_p88941846776"><a name="zh-cn_topic_0000001447121493_p88941846776"></a><a name="zh-cn_topic_0000001447121493_p88941846776"></a>类型：Int64</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row17294008"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p10200194572810"><a name="zh-cn_topic_0000001447121493_p10200194572810"></a><a name="zh-cn_topic_0000001447121493_p10200194572810"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p52009416"><a name="zh-cn_topic_0000001447121493_p52009416"></a><a name="zh-cn_topic_0000001447121493_p52009416"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p52013150"><a name="zh-cn_topic_0000001447121493_p52013150"></a><a name="zh-cn_topic_0000001447121493_p52013150"></a>health</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p52315658"><a name="zh-cn_topic_0000001447121493_p52315658"></a><a name="zh-cn_topic_0000001447121493_p52315658"></a>含义：健康状态</p>
<p id="zh-cn_topic_0000001447121493_p0140443810"><a name="zh-cn_topic_0000001447121493_p0140443810"></a><a name="zh-cn_topic_0000001447121493_p0140443810"></a>类型：bool</p>
<p id="zh-cn_topic_0000001447121493_p814013415810"><a name="zh-cn_topic_0000001447121493_p814013415810"></a><a name="zh-cn_topic_0000001447121493_p814013415810"></a>取值：true或false</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row17756161742918"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p167572017122914"><a name="zh-cn_topic_0000001447121493_p167572017122914"></a><a name="zh-cn_topic_0000001447121493_p167572017122914"></a>ai_processors</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p13757161742918"><a name="zh-cn_topic_0000001447121493_p13757161742918"></a><a name="zh-cn_topic_0000001447121493_p13757161742918"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p1775781715296"><a name="zh-cn_topic_0000001447121493_p1775781715296"></a><a name="zh-cn_topic_0000001447121493_p1775781715296"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p167571717162916"><a name="zh-cn_topic_0000001447121493_p167571717162916"></a><a name="zh-cn_topic_0000001447121493_p167571717162916"></a>NPU动态信息</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row65559985"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p8758529"><a name="zh-cn_topic_0000001447121493_p8758529"></a><a name="zh-cn_topic_0000001447121493_p8758529"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p38352221"><a name="zh-cn_topic_0000001447121493_p38352221"></a><a name="zh-cn_topic_0000001447121493_p38352221"></a>id</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p19522222"><a name="zh-cn_topic_0000001447121493_p19522222"></a><a name="zh-cn_topic_0000001447121493_p19522222"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p37796188"><a name="zh-cn_topic_0000001447121493_p37796188"></a><a name="zh-cn_topic_0000001447121493_p37796188"></a>含义：AI芯片编号</p>
<p id="zh-cn_topic_0000001447121493_p1881344412814"><a name="zh-cn_topic_0000001447121493_p1881344412814"></a><a name="zh-cn_topic_0000001447121493_p1881344412814"></a>类型：int</p>
<p id="zh-cn_topic_0000001447121493_p188131744683"><a name="zh-cn_topic_0000001447121493_p188131744683"></a><a name="zh-cn_topic_0000001447121493_p188131744683"></a>取值：默认一个芯片，编号为0</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row54761048"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p968132912919"><a name="zh-cn_topic_0000001447121493_p968132912919"></a><a name="zh-cn_topic_0000001447121493_p968132912919"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p53493202"><a name="zh-cn_topic_0000001447121493_p53493202"></a><a name="zh-cn_topic_0000001447121493_p53493202"></a>temperature</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p37982087"><a name="zh-cn_topic_0000001447121493_p37982087"></a><a name="zh-cn_topic_0000001447121493_p37982087"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p56650221"><a name="zh-cn_topic_0000001447121493_p56650221"></a><a name="zh-cn_topic_0000001447121493_p56650221"></a>含义：AI芯片温度</p>
<p id="zh-cn_topic_0000001447121493_p207031858985"><a name="zh-cn_topic_0000001447121493_p207031858985"></a><a name="zh-cn_topic_0000001447121493_p207031858985"></a>类型：int</p>
<p id="zh-cn_topic_0000001447121493_p6703145816817"><a name="zh-cn_topic_0000001447121493_p6703145816817"></a><a name="zh-cn_topic_0000001447121493_p6703145816817"></a>取值：单位默认为℃</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row30499138"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p16681829192911"><a name="zh-cn_topic_0000001447121493_p16681829192911"></a><a name="zh-cn_topic_0000001447121493_p16681829192911"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p53321051"><a name="zh-cn_topic_0000001447121493_p53321051"></a><a name="zh-cn_topic_0000001447121493_p53321051"></a>health</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p24037903"><a name="zh-cn_topic_0000001447121493_p24037903"></a><a name="zh-cn_topic_0000001447121493_p24037903"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p913127"><a name="zh-cn_topic_0000001447121493_p913127"></a><a name="zh-cn_topic_0000001447121493_p913127"></a>含义：健康状态</p>
<p id="zh-cn_topic_0000001447121493_p2429191216914"><a name="zh-cn_topic_0000001447121493_p2429191216914"></a><a name="zh-cn_topic_0000001447121493_p2429191216914"></a>类型：bool</p>
<p id="zh-cn_topic_0000001447121493_p19430201216917"><a name="zh-cn_topic_0000001447121493_p19430201216917"></a><a name="zh-cn_topic_0000001447121493_p19430201216917"></a>取值：true或false</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row30857297"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p868112911299"><a name="zh-cn_topic_0000001447121493_p868112911299"></a><a name="zh-cn_topic_0000001447121493_p868112911299"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p54393343"><a name="zh-cn_topic_0000001447121493_p54393343"></a><a name="zh-cn_topic_0000001447121493_p54393343"></a>occupancy_rate</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p43784667"><a name="zh-cn_topic_0000001447121493_p43784667"></a><a name="zh-cn_topic_0000001447121493_p43784667"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p56897135"><a name="zh-cn_topic_0000001447121493_p56897135"></a><a name="zh-cn_topic_0000001447121493_p56897135"></a>含义：资源占用率信息</p>
<p id="zh-cn_topic_0000001447121493_p19486172814915"><a name="zh-cn_topic_0000001447121493_p19486172814915"></a><a name="zh-cn_topic_0000001447121493_p19486172814915"></a>类型：list</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row47917043"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p1468132910293"><a name="zh-cn_topic_0000001447121493_p1468132910293"></a><a name="zh-cn_topic_0000001447121493_p1468132910293"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p45803850"><a name="zh-cn_topic_0000001447121493_p45803850"></a><a name="zh-cn_topic_0000001447121493_p45803850"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p19124360"><a name="zh-cn_topic_0000001447121493_p19124360"></a><a name="zh-cn_topic_0000001447121493_p19124360"></a>ai_core</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p5569331"><a name="zh-cn_topic_0000001447121493_p5569331"></a><a name="zh-cn_topic_0000001447121493_p5569331"></a>含义：AI Core占用率</p>
<p id="zh-cn_topic_0000001447121493_p1024114441793"><a name="zh-cn_topic_0000001447121493_p1024114441793"></a><a name="zh-cn_topic_0000001447121493_p1024114441793"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p15242184415916"><a name="zh-cn_topic_0000001447121493_p15242184415916"></a><a name="zh-cn_topic_0000001447121493_p15242184415916"></a>取值：0%~100%</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row30003806"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p76815293295"><a name="zh-cn_topic_0000001447121493_p76815293295"></a><a name="zh-cn_topic_0000001447121493_p76815293295"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p24673169"><a name="zh-cn_topic_0000001447121493_p24673169"></a><a name="zh-cn_topic_0000001447121493_p24673169"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p52369659"><a name="zh-cn_topic_0000001447121493_p52369659"></a><a name="zh-cn_topic_0000001447121493_p52369659"></a>ai_cpu</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p14084006"><a name="zh-cn_topic_0000001447121493_p14084006"></a><a name="zh-cn_topic_0000001447121493_p14084006"></a>含义：AI CPU占用率</p>
<p id="zh-cn_topic_0000001447121493_p1788311116109"><a name="zh-cn_topic_0000001447121493_p1788311116109"></a><a name="zh-cn_topic_0000001447121493_p1788311116109"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p148831181010"><a name="zh-cn_topic_0000001447121493_p148831181010"></a><a name="zh-cn_topic_0000001447121493_p148831181010"></a>取值：0%~100%</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row33465549"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p106872972911"><a name="zh-cn_topic_0000001447121493_p106872972911"></a><a name="zh-cn_topic_0000001447121493_p106872972911"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p54376753"><a name="zh-cn_topic_0000001447121493_p54376753"></a><a name="zh-cn_topic_0000001447121493_p54376753"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p42440866"><a name="zh-cn_topic_0000001447121493_p42440866"></a><a name="zh-cn_topic_0000001447121493_p42440866"></a>ctrl_cpu</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p15158087"><a name="zh-cn_topic_0000001447121493_p15158087"></a><a name="zh-cn_topic_0000001447121493_p15158087"></a>含义：控制CPU占用率</p>
<p id="zh-cn_topic_0000001447121493_p680510714107"><a name="zh-cn_topic_0000001447121493_p680510714107"></a><a name="zh-cn_topic_0000001447121493_p680510714107"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p0805157181017"><a name="zh-cn_topic_0000001447121493_p0805157181017"></a><a name="zh-cn_topic_0000001447121493_p0805157181017"></a>取值：0%~100%</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row38973127"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p146814292299"><a name="zh-cn_topic_0000001447121493_p146814292299"></a><a name="zh-cn_topic_0000001447121493_p146814292299"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p17916367"><a name="zh-cn_topic_0000001447121493_p17916367"></a><a name="zh-cn_topic_0000001447121493_p17916367"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p41939632"><a name="zh-cn_topic_0000001447121493_p41939632"></a><a name="zh-cn_topic_0000001447121493_p41939632"></a>ddr_cap</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p41667033"><a name="zh-cn_topic_0000001447121493_p41667033"></a><a name="zh-cn_topic_0000001447121493_p41667033"></a>含义：DDR内存占用率</p>
<p id="zh-cn_topic_0000001447121493_p168681011191010"><a name="zh-cn_topic_0000001447121493_p168681011191010"></a><a name="zh-cn_topic_0000001447121493_p168681011191010"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1386971171013"><a name="zh-cn_topic_0000001447121493_p1386971171013"></a><a name="zh-cn_topic_0000001447121493_p1386971171013"></a>取值：0%~100%</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row51514200"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p1468429112910"><a name="zh-cn_topic_0000001447121493_p1468429112910"></a><a name="zh-cn_topic_0000001447121493_p1468429112910"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p18524768"><a name="zh-cn_topic_0000001447121493_p18524768"></a><a name="zh-cn_topic_0000001447121493_p18524768"></a><strong id="zh-cn_topic_0000001447121493_b32505188"><a name="zh-cn_topic_0000001447121493_b32505188"></a><a name="zh-cn_topic_0000001447121493_b32505188"></a></strong>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p15674539"><a name="zh-cn_topic_0000001447121493_p15674539"></a><a name="zh-cn_topic_0000001447121493_p15674539"></a>ddr_bw</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p61678167"><a name="zh-cn_topic_0000001447121493_p61678167"></a><a name="zh-cn_topic_0000001447121493_p61678167"></a>含义：DDR带宽占用率</p>
<p id="zh-cn_topic_0000001447121493_p19899101601016"><a name="zh-cn_topic_0000001447121493_p19899101601016"></a><a name="zh-cn_topic_0000001447121493_p19899101601016"></a>类型：string</p>
<p id="zh-cn_topic_0000001447121493_p1689941612106"><a name="zh-cn_topic_0000001447121493_p1689941612106"></a><a name="zh-cn_topic_0000001447121493_p1689941612106"></a>取值：0%~100%</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447121493_row1164435819205"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001447121493_p16644145862018"><a name="zh-cn_topic_0000001447121493_p16644145862018"></a><a name="zh-cn_topic_0000001447121493_p16644145862018"></a>lte_info</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001447121493_p1064419584202"><a name="zh-cn_topic_0000001447121493_p1064419584202"></a><a name="zh-cn_topic_0000001447121493_p1064419584202"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001447121493_p564435811208"><a name="zh-cn_topic_0000001447121493_p564435811208"></a><a name="zh-cn_topic_0000001447121493_p564435811208"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001447121493_p20730101910219"><a name="zh-cn_topic_0000001447121493_p20730101910219"></a><a name="zh-cn_topic_0000001447121493_p20730101910219"></a>含义：无线网络信息</p>
<p id="zh-cn_topic_0000001447121493_p973012196218"><a name="zh-cn_topic_0000001447121493_p973012196218"></a><a name="zh-cn_topic_0000001447121493_p973012196218"></a>类型：list</p>
<p id="zh-cn_topic_0000001447121493_p1173081912110"><a name="zh-cn_topic_0000001447121493_p1173081912110"></a><a name="zh-cn_topic_0000001447121493_p1173081912110"></a>取值：无线网络相关信息</p>
</td>
</tr>
<tr id="row14810444173212"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p176743012415"><a name="p176743012415"></a><a name="p176743012415"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p520842181510"><a name="p520842181510"></a><a name="p520842181510"></a>default_getaway</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p120862111520"><a name="p120862111520"></a><a name="p120862111520"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1220872112159"><a name="p1220872112159"></a><a name="p1220872112159"></a>默认网关，默认为false。</p>
<a name="ul126683291176"></a><a name="ul126683291176"></a><ul id="ul126683291176"><li>true：配置默认网关。</li><li>false：未配置默认网关。</li></ul>
</td>
</tr>
<tr id="row1745848193211"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17677303413"><a name="p17677303413"></a><a name="p17677303413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p7209921181516"><a name="p7209921181516"></a><a name="p7209921181516"></a>lte_enable</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p202091621201513"><a name="p202091621201513"></a><a name="p202091621201513"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p1520914213155"><a name="p1520914213155"></a><a name="p1520914213155"></a>无线网络是否使能，默认为false。</p>
<a name="ul1469820282214"></a><a name="ul1469820282214"></a><ul id="ul1469820282214"><li>true：使能。</li><li>false：不可用。</li></ul>
</td>
</tr>
<tr id="row112441148123211"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p967103013416"><a name="p967103013416"></a><a name="p967103013416"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p18617143041515"><a name="p18617143041515"></a><a name="p18617143041515"></a>sim_exist</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p8617143012159"><a name="p8617143012159"></a><a name="p8617143012159"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p161713307157"><a name="p161713307157"></a><a name="p161713307157"></a>SIM卡是否在位。</p>
<a name="ul1575212812221"></a><a name="ul1575212812221"></a><ul id="ul1575212812221"><li>true：在位。</li><li>false：不在位。</li></ul>
</td>
</tr>
<tr id="row134212048163219"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17671301748"><a name="p17671301748"></a><a name="p17671301748"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1961833081512"><a name="p1961833081512"></a><a name="p1961833081512"></a>state_data</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p156184309155"><a name="p156184309155"></a><a name="p156184309155"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p36181304157"><a name="p36181304157"></a><a name="p36181304157"></a>移动数据的开关状态。</p>
<a name="ul149114486225"></a><a name="ul149114486225"></a><ul id="ul149114486225"><li>true：打开状态。</li><li>false：关闭状态。</li></ul>
</td>
</tr>
<tr id="row11589948203213"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p7684306412"><a name="p7684306412"></a><a name="p7684306412"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p20618193041510"><a name="p20618193041510"></a><a name="p20618193041510"></a>state_lte</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p46181430191513"><a name="p46181430191513"></a><a name="p46181430191513"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p12618143011510"><a name="p12618143011510"></a><a name="p12618143011510"></a>无线网络的开关状态。</p>
<a name="ul647119137234"></a><a name="ul647119137234"></a><ul id="ul647119137234"><li>true：打开状态。</li><li>false：关闭状态。</li></ul>
</td>
</tr>
<tr id="row1874134812323"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p116863010419"><a name="p116863010419"></a><a name="p116863010419"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1297735121519"><a name="p1297735121519"></a><a name="p1297735121519"></a>network_signal_level</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p129723551510"><a name="p129723551510"></a><a name="p129723551510"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p22981735121511"><a name="p22981735121511"></a><a name="p22981735121511"></a>数字，信号强度。</p>
<p id="zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_p11402371432"><a name="zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_p11402371432"></a><a name="zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_p11402371432"></a>取值范围：0~5级，取值0时，代表网络断开，这时network_type的取值为null。</p>
</td>
</tr>
<tr id="row1590294813322"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p16681309415"><a name="p16681309415"></a><a name="p16681309415"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p192985358151"><a name="p192985358151"></a><a name="p192985358151"></a>network_type</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p122985355154"><a name="p122985355154"></a><a name="p122985355154"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p8298163516156"><a name="p8298163516156"></a><a name="p8298163516156"></a>字符串，网络状态。</p>
<p id="p1370181182411"><a name="p1370181182411"></a><a name="p1370181182411"></a>取值范围为2G、3G、4G、5G。</p>
<p id="p178521582367"><a name="p178521582367"></a><a name="p178521582367"></a>当网络断开，取值为null。</p>
</td>
</tr>
<tr id="row15531349193211"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p46843015410"><a name="p46843015410"></a><a name="p46843015410"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p142981535171515"><a name="p142981535171515"></a><a name="p142981535171515"></a>ip_addr</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p429811355158"><a name="p429811355158"></a><a name="p429811355158"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p2298635131515"><a name="p2298635131515"></a><a name="p2298635131515"></a>字符串，ipv4地址。</p>
<p id="p327333252420"><a name="p327333252420"></a><a name="p327333252420"></a>无线网络拨号成功后，会显示ip地址。</p>
</td>
</tr>
<tr id="row112051491326"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p16682030145"><a name="p16682030145"></a><a name="p16682030145"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p12298435111510"><a name="p12298435111510"></a><a name="p12298435111510"></a>apn_info</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p18298133519159"><a name="p18298133519159"></a><a name="p18298133519159"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p72981635171513"><a name="p72981635171513"></a><a name="p72981635171513"></a>数组，apn相关信息。</p>
</td>
</tr>
<tr id="row1837318492322"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p13687301747"><a name="p13687301747"></a><a name="p13687301747"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p729883561513"><a name="p729883561513"></a><a name="p729883561513"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p4298335141513"><a name="p4298335141513"></a><a name="p4298335141513"></a>apn_name</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p22981335111514"><a name="p22981335111514"></a><a name="p22981335111514"></a>拨号时使用的APN名称，允许为空。</p>
</td>
</tr>
<tr id="row16517174913321"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p82981735111517"><a name="p82981735111517"></a><a name="p82981735111517"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p1929803511153"><a name="p1929803511153"></a><a name="p1929803511153"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1929883513152"><a name="p1929883513152"></a><a name="p1929883513152"></a>apn_user</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p18299135151514"><a name="p18299135151514"></a><a name="p18299135151514"></a>拨号时使用的APN用户名，允许为空。</p>
</td>
</tr>
<tr id="row20694849133212"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p104401329269"><a name="p104401329269"></a><a name="p104401329269"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p164401723260"><a name="p164401723260"></a><a name="p164401723260"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p1344010211260"><a name="p1344010211260"></a><a name="p1344010211260"></a>auth_type</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p11440192152615"><a name="p11440192152615"></a><a name="p11440192152615"></a>身份验证类型。</p>
<p id="zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_p1494215421738"><a name="zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_p1494215421738"></a><a name="zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_p1494215421738"></a>取值为0、1、2、3，其含义分别为：</p>
<a name="zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_ul1425241416412"></a><a name="zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_ul1425241416412"></a><ul id="zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_ul1425241416412"><li>0：NONE</li><li>1：PAP</li><li>2：CHAP</li><li>3：PAP or CHAP</li></ul>
</td>
</tr>
<tr id="row285411497326"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="p17441023265"><a name="p17441023265"></a><a name="p17441023265"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="p12441122132610"><a name="p12441122132610"></a><a name="p12441122132610"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="p6441202172610"><a name="p6441202172610"></a><a name="p6441202172610"></a>mode_type</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="p15441228262"><a name="p15441228262"></a><a name="p15441228262"></a>无线网络模块类型。</p>
<p id="p1375164820581"><a name="p1375164820581"></a><a name="p1375164820581"></a>取值为0、1、2、3，其含义分别为：</p>
<a name="ul1637574885815"></a><a name="ul1637574885815"></a><ul id="ul1637574885815"><li>0：华为 ME909S（暂不支持）</li><li>1：移远 EC25（暂不支持）</li><li>2：移远 EC200T</li><li>3：移远 RM500U</li></ul>
</td>
</tr>
</tbody>
</table>

### 上报固件安装/升级进度<a name="ZH-CN_TOPIC_0000001577530736"></a>

执行固件升级时，升级进度未达到100%前，由边缘侧定时5秒发布一次，仅上报当前正在升级的固件升级进度信息。当收到rearm消息时，会把所有未生效的固件信息一起返回。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/upgrade_progress"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "members": [{
        "operator": "install",
        "name": "A500-3000-3010-firmware",
        "version": "1.0.0",
        "percentage": "20%",
        "result": "failed",
        "reason": "ERR.165, MiniD upgrade failed"
    }]
}
```

元素定义如下：

<a name="zh-cn_topic_0000001396921754_table48735027"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001396921754_row2298077"><th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.1"><p id="zh-cn_topic_0000001396921754_p51926520"><a name="zh-cn_topic_0000001396921754_p51926520"></a><a name="zh-cn_topic_0000001396921754_p51926520"></a>一级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.2"><p id="zh-cn_topic_0000001396921754_p45298596"><a name="zh-cn_topic_0000001396921754_p45298596"></a><a name="zh-cn_topic_0000001396921754_p45298596"></a>二级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.3"><p id="zh-cn_topic_0000001396921754_p45307656"><a name="zh-cn_topic_0000001396921754_p45307656"></a><a name="zh-cn_topic_0000001396921754_p45307656"></a>描述</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.4"><p id="zh-cn_topic_0000001396921754_p46041549"><a name="zh-cn_topic_0000001396921754_p46041549"></a><a name="zh-cn_topic_0000001396921754_p46041549"></a>类型</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.6.1.5"><p id="zh-cn_topic_0000001396921754_p38378011"><a name="zh-cn_topic_0000001396921754_p38378011"></a><a name="zh-cn_topic_0000001396921754_p38378011"></a>取值范围</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001396921754_row10868702"><td class="cellrowborder" rowspan="6" valign="top" width="15%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396921754_p7949639"><a name="zh-cn_topic_0000001396921754_p7949639"></a><a name="zh-cn_topic_0000001396921754_p7949639"></a>members</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396921754_p39940984"><a name="zh-cn_topic_0000001396921754_p39940984"></a><a name="zh-cn_topic_0000001396921754_p39940984"></a>operator</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396921754_p13994266"><a name="zh-cn_topic_0000001396921754_p13994266"></a><a name="zh-cn_topic_0000001396921754_p13994266"></a>操作类型</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001396921754_p59793748"><a name="zh-cn_topic_0000001396921754_p59793748"></a><a name="zh-cn_topic_0000001396921754_p59793748"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396921754_p11455439"><a name="zh-cn_topic_0000001396921754_p11455439"></a><a name="zh-cn_topic_0000001396921754_p11455439"></a>取值为install或uninstall。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921754_row33381819"><td class="cellrowborder" valign="top" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396921754_p41895032"><a name="zh-cn_topic_0000001396921754_p41895032"></a><a name="zh-cn_topic_0000001396921754_p41895032"></a>name</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396921754_p38054464"><a name="zh-cn_topic_0000001396921754_p38054464"></a><a name="zh-cn_topic_0000001396921754_p38054464"></a>名称</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396921754_p62512728"><a name="zh-cn_topic_0000001396921754_p62512728"></a><a name="zh-cn_topic_0000001396921754_p62512728"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001396921754_p30366184"><a name="zh-cn_topic_0000001396921754_p30366184"></a><a name="zh-cn_topic_0000001396921754_p30366184"></a>如果是主机软件，表示该软件名称。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921754_row4860205"><td class="cellrowborder" valign="top" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396921754_p11097898"><a name="zh-cn_topic_0000001396921754_p11097898"></a><a name="zh-cn_topic_0000001396921754_p11097898"></a>version</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396921754_p26514531"><a name="zh-cn_topic_0000001396921754_p26514531"></a><a name="zh-cn_topic_0000001396921754_p26514531"></a>固件版本</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396921754_p193401"><a name="zh-cn_topic_0000001396921754_p193401"></a><a name="zh-cn_topic_0000001396921754_p193401"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001396921754_p15665491"><a name="zh-cn_topic_0000001396921754_p15665491"></a><a name="zh-cn_topic_0000001396921754_p15665491"></a>32字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921754_row6771693"><td class="cellrowborder" valign="top" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396921754_p3013405"><a name="zh-cn_topic_0000001396921754_p3013405"></a><a name="zh-cn_topic_0000001396921754_p3013405"></a>percentage</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396921754_p42759282"><a name="zh-cn_topic_0000001396921754_p42759282"></a><a name="zh-cn_topic_0000001396921754_p42759282"></a>升级百分比</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396921754_p40949840"><a name="zh-cn_topic_0000001396921754_p40949840"></a><a name="zh-cn_topic_0000001396921754_p40949840"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001396921754_p28602718"><a name="zh-cn_topic_0000001396921754_p28602718"></a><a name="zh-cn_topic_0000001396921754_p28602718"></a>取值范围为0%~100%。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921754_row56097870"><td class="cellrowborder" valign="top" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396921754_p33119622"><a name="zh-cn_topic_0000001396921754_p33119622"></a><a name="zh-cn_topic_0000001396921754_p33119622"></a>result</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396921754_p65443729"><a name="zh-cn_topic_0000001396921754_p65443729"></a><a name="zh-cn_topic_0000001396921754_p65443729"></a>升级结果</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396921754_p66450672"><a name="zh-cn_topic_0000001396921754_p66450672"></a><a name="zh-cn_topic_0000001396921754_p66450672"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001396921754_p13795340"><a name="zh-cn_topic_0000001396921754_p13795340"></a><a name="zh-cn_topic_0000001396921754_p13795340"></a>取值为success、failed或processing。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921754_row57049196"><td class="cellrowborder" valign="top" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396921754_p33640543"><a name="zh-cn_topic_0000001396921754_p33640543"></a><a name="zh-cn_topic_0000001396921754_p33640543"></a>reason</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396921754_p40529449"><a name="zh-cn_topic_0000001396921754_p40529449"></a><a name="zh-cn_topic_0000001396921754_p40529449"></a>升级失败原因</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396921754_p61659956"><a name="zh-cn_topic_0000001396921754_p61659956"></a><a name="zh-cn_topic_0000001396921754_p61659956"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001396921754_p28400552"><a name="zh-cn_topic_0000001396921754_p28400552"></a><a name="zh-cn_topic_0000001396921754_p28400552"></a>返回格式统一为：ERR.errcode，detailinfo，具体错误码信息，参见<a href="#错误码说明">错误码说明</a>。</p>
</td>
</tr>
</tbody>
</table>

### 上报信息收集进度<a name="ZH-CN_TOPIC_0000001628849841"></a>

执行信息收集时，收集进度未达到100%前，由边缘侧定时5秒发布一次。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/info_collect_process"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下：

```json
{
    "type": "all",
    "module": "all",
    "percentage": "30%",
    "result": "processing",
    "reason":""
}
```

各元素定义如下：

|一级资源名称|描述|类型|取值范围|
|--|--|--|--|
|type|收集信息类型|string|当前只支持all|
|module|日志模块|string|当前只支持APP和all|
|percentage|收集进度|string|取值范围为0%~100%|
|result|收集结果|string|取值为success、failed和processing|
|reason|收集日志失败原因|string|返回格式统一为：ERR.errcode，detailinfo，具体错误码信息，请参见[错误码说明](#错误码说明)。|

### 上报配置生效进度<a id="上报配置生效进度"></a>

执行配置生效接口时，在生效进度未达到100%时，由边缘侧定时5秒钟向中心侧上报一次进度。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/profile_effect"
    },
    "content": {
        "profile_name": "profile001",
        "percentage": "30%",
        "result": " processing ",
        "reason": "System status abnormal"
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "profile_name": "profile001",
    "percentage": "30%",
    "result": "in progress",
    "reason": "System status abnormal"
}
```

各元素定义如下：

|一级资源名称|描述|类型|取值范围|
|--|--|--|--|
|profile_name|配置文件名|string|32字节|
|percentage|收集进度|string|取值范围为0%~100%|
|result|收集结果|string|取值为success、failed、partial failed和in progress|
|reason|失败原因|string|返回格式统一为：ERR.errcode，detailinfo，具体错误码信息，请参见[错误码说明](#错误码说明)。|

### 上报硬件告警/事件<a name="ZH-CN_TOPIC_0000001577530728"></a>

当边缘侧发生告警时，主动上报当前产生/恢复的告警信息；当收到rearm消息时，会上报当前所有未恢复的告警。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/alarm"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下：

```json
{
    "alarm": [{
        "type": "alarm",
        "alarmId": "0x00010002",
        "alarmName": "emmc temperature up anomaly",
        "resource": "EMMC0",
        "perceivedSeverity": "MINOR",
        "timestamp": "2018-11-08T21:57:21+08:00",
        "sn": "1",
        "notificationType": "alarm",
        "detailedInformation": "EMMC temperature abnormal",
        "suggestion": "alarm handle suggestion",
        "reason": "alarm reason",
        "impact": "alarm impact"
    }]
}
```

告警ID定义待定。

各元素定义如下：

**表 1** 

<a name="zh-cn_topic_0000001447161485_table44156300"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001447161485_row40828621"><th class="cellrowborder" valign="top" width="15%" id="mcps1.2.6.1.1"><p id="zh-cn_topic_0000001447161485_p18784032"><a name="zh-cn_topic_0000001447161485_p18784032"></a><a name="zh-cn_topic_0000001447161485_p18784032"></a>一级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.2.6.1.2"><p id="zh-cn_topic_0000001447161485_p45111603"><a name="zh-cn_topic_0000001447161485_p45111603"></a><a name="zh-cn_topic_0000001447161485_p45111603"></a>二级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.2.6.1.3"><p id="zh-cn_topic_0000001447161485_p30161198"><a name="zh-cn_topic_0000001447161485_p30161198"></a><a name="zh-cn_topic_0000001447161485_p30161198"></a>描述</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.2.6.1.4"><p id="zh-cn_topic_0000001447161485_p27137978"><a name="zh-cn_topic_0000001447161485_p27137978"></a><a name="zh-cn_topic_0000001447161485_p27137978"></a>类型</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.2.6.1.5"><p id="zh-cn_topic_0000001447161485_p50692606"><a name="zh-cn_topic_0000001447161485_p50692606"></a><a name="zh-cn_topic_0000001447161485_p50692606"></a>取值范围</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001447161485_row12553595"><td class="cellrowborder" rowspan="12" valign="top" width="15%" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p10208299"><a name="zh-cn_topic_0000001447161485_p10208299"></a><a name="zh-cn_topic_0000001447161485_p10208299"></a>alarm</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p21565908"><a name="zh-cn_topic_0000001447161485_p21565908"></a><a name="zh-cn_topic_0000001447161485_p21565908"></a>type</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p2008093"><a name="zh-cn_topic_0000001447161485_p2008093"></a><a name="zh-cn_topic_0000001447161485_p2008093"></a>告警类型</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p28437806"><a name="zh-cn_topic_0000001447161485_p28437806"></a><a name="zh-cn_topic_0000001447161485_p28437806"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.2.6.1.5 "><p id="zh-cn_topic_0000001447161485_p21760944"><a name="zh-cn_topic_0000001447161485_p21760944"></a><a name="zh-cn_topic_0000001447161485_p21760944"></a>取值为alarm和event</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row61630770"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p28582738"><a name="zh-cn_topic_0000001447161485_p28582738"></a><a name="zh-cn_topic_0000001447161485_p28582738"></a>alarmId</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p33500465"><a name="zh-cn_topic_0000001447161485_p33500465"></a><a name="zh-cn_topic_0000001447161485_p33500465"></a>告警ID</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p29183140"><a name="zh-cn_topic_0000001447161485_p29183140"></a><a name="zh-cn_topic_0000001447161485_p29183140"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p15024148"><a name="zh-cn_topic_0000001447161485_p15024148"></a><a name="zh-cn_topic_0000001447161485_p15024148"></a>固定格式为0x00010002，其中最高位默认为0，也就是取值范围为：0x00000000~0x0fffffff</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row999606"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p48856913"><a name="zh-cn_topic_0000001447161485_p48856913"></a><a name="zh-cn_topic_0000001447161485_p48856913"></a>alarmName</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p65095887"><a name="zh-cn_topic_0000001447161485_p65095887"></a><a name="zh-cn_topic_0000001447161485_p65095887"></a>告警名称</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p38275534"><a name="zh-cn_topic_0000001447161485_p38275534"></a><a name="zh-cn_topic_0000001447161485_p38275534"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p264811211232"><a name="zh-cn_topic_0000001447161485_p264811211232"></a><a name="zh-cn_topic_0000001447161485_p264811211232"></a>[a-z0-9A-Z- _]{1,64}</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row52686376"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p64667859"><a name="zh-cn_topic_0000001447161485_p64667859"></a><a name="zh-cn_topic_0000001447161485_p64667859"></a>resource</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p3605227"><a name="zh-cn_topic_0000001447161485_p3605227"></a><a name="zh-cn_topic_0000001447161485_p3605227"></a>告警资源名称</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p23587984"><a name="zh-cn_topic_0000001447161485_p23587984"></a><a name="zh-cn_topic_0000001447161485_p23587984"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p31578537"><a name="zh-cn_topic_0000001447161485_p31578537"></a><a name="zh-cn_topic_0000001447161485_p31578537"></a>[a-z0-9A-Z- _]{1,256}</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row15771381"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p61272867"><a name="zh-cn_topic_0000001447161485_p61272867"></a><a name="zh-cn_topic_0000001447161485_p61272867"></a>perceivedSeverity</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p64155199"><a name="zh-cn_topic_0000001447161485_p64155199"></a><a name="zh-cn_topic_0000001447161485_p64155199"></a>告警级别</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p29188627"><a name="zh-cn_topic_0000001447161485_p29188627"></a><a name="zh-cn_topic_0000001447161485_p29188627"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p15468616"><a name="zh-cn_topic_0000001447161485_p15468616"></a><a name="zh-cn_topic_0000001447161485_p15468616"></a>取值为MINOR/MAJOR/CRITICAL</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row4999822"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p54712468"><a name="zh-cn_topic_0000001447161485_p54712468"></a><a name="zh-cn_topic_0000001447161485_p54712468"></a>timestamp</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p2524942"><a name="zh-cn_topic_0000001447161485_p2524942"></a><a name="zh-cn_topic_0000001447161485_p2524942"></a>告警产生时间</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p3193740"><a name="zh-cn_topic_0000001447161485_p3193740"></a><a name="zh-cn_topic_0000001447161485_p3193740"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p57366391"><a name="zh-cn_topic_0000001447161485_p57366391"></a><a name="zh-cn_topic_0000001447161485_p57366391"></a>64字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row209211451134510"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p492105124510"><a name="zh-cn_topic_0000001447161485_p492105124510"></a><a name="zh-cn_topic_0000001447161485_p492105124510"></a>sn</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p159221551144519"><a name="zh-cn_topic_0000001447161485_p159221551144519"></a><a name="zh-cn_topic_0000001447161485_p159221551144519"></a>告警序列号</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p18922851164511"><a name="zh-cn_topic_0000001447161485_p18922851164511"></a><a name="zh-cn_topic_0000001447161485_p18922851164511"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p1392212516457"><a name="zh-cn_topic_0000001447161485_p1392212516457"></a><a name="zh-cn_topic_0000001447161485_p1392212516457"></a>-</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row46535472"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p41014082"><a name="zh-cn_topic_0000001447161485_p41014082"></a><a name="zh-cn_topic_0000001447161485_p41014082"></a>notificationType</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p33806307"><a name="zh-cn_topic_0000001447161485_p33806307"></a><a name="zh-cn_topic_0000001447161485_p33806307"></a>告警标志</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="p191912231257"><a name="p191912231257"></a><a name="p191912231257"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p8387487"><a name="zh-cn_topic_0000001447161485_p8387487"></a><a name="zh-cn_topic_0000001447161485_p8387487"></a>表示告警产生还是恢复，取值为alarm和clear，当type为event时，固定为空</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row8378525"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p1064113145299"><a name="zh-cn_topic_0000001447161485_p1064113145299"></a><a name="zh-cn_topic_0000001447161485_p1064113145299"></a>detailedInformation</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p18911313"><a name="zh-cn_topic_0000001447161485_p18911313"></a><a name="zh-cn_topic_0000001447161485_p18911313"></a>告警描述</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p55421393"><a name="zh-cn_topic_0000001447161485_p55421393"></a><a name="zh-cn_topic_0000001447161485_p55421393"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p59947829"><a name="zh-cn_topic_0000001447161485_p59947829"></a><a name="zh-cn_topic_0000001447161485_p59947829"></a>256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row87422230296"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p77439238299"><a name="zh-cn_topic_0000001447161485_p77439238299"></a><a name="zh-cn_topic_0000001447161485_p77439238299"></a>suggestion</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p117431223152914"><a name="zh-cn_topic_0000001447161485_p117431223152914"></a><a name="zh-cn_topic_0000001447161485_p117431223152914"></a>告警处理建议</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p8743142372919"><a name="zh-cn_topic_0000001447161485_p8743142372919"></a><a name="zh-cn_topic_0000001447161485_p8743142372919"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p3478145210306"><a name="zh-cn_topic_0000001447161485_p3478145210306"></a><a name="zh-cn_topic_0000001447161485_p3478145210306"></a>4096字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row536513211295"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p83651732182912"><a name="zh-cn_topic_0000001447161485_p83651732182912"></a><a name="zh-cn_topic_0000001447161485_p83651732182912"></a>reason</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p336583272912"><a name="zh-cn_topic_0000001447161485_p336583272912"></a><a name="zh-cn_topic_0000001447161485_p336583272912"></a>告警原因</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p13655329293"><a name="zh-cn_topic_0000001447161485_p13655329293"></a><a name="zh-cn_topic_0000001447161485_p13655329293"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p1742165313305"><a name="zh-cn_topic_0000001447161485_p1742165313305"></a><a name="zh-cn_topic_0000001447161485_p1742165313305"></a>256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161485_row8214344202918"><td class="cellrowborder" valign="top" headers="mcps1.2.6.1.1 "><p id="zh-cn_topic_0000001447161485_p72149448290"><a name="zh-cn_topic_0000001447161485_p72149448290"></a><a name="zh-cn_topic_0000001447161485_p72149448290"></a>impact</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.2 "><p id="zh-cn_topic_0000001447161485_p19214164416296"><a name="zh-cn_topic_0000001447161485_p19214164416296"></a><a name="zh-cn_topic_0000001447161485_p19214164416296"></a>告警影响</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.3 "><p id="zh-cn_topic_0000001447161485_p12214164492912"><a name="zh-cn_topic_0000001447161485_p12214164492912"></a><a name="zh-cn_topic_0000001447161485_p12214164492912"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.6.1.4 "><p id="zh-cn_topic_0000001447161485_p1294617581304"><a name="zh-cn_topic_0000001447161485_p1294617581304"></a><a name="zh-cn_topic_0000001447161485_p1294617581304"></a>256字节</p>
</td>
</tr>
</tbody>
</table>

### 上报系统可复位状态<a name="ZH-CN_TOPIC_0000001628610473"></a>

当边缘侧收到复位命令时，首先进行系统检查，系统正常情况下返回允许复位消息；当系统正在执行升级、配置导入等动作时，返回不可复位消息以及不可复位原因说明。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/restart_result"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "restartable": true | false,
    "reason": "System status abnormal"
}
```

各元素定义如下：

|一级资源名称|描述|类型|取值范围|
|--|--|--|--|
|restartable|复位状态|string|取值只能为true和false，正常情况下返回true。当系统正在执行升级、配置导入等动作时，返回false，并返回具体的原因说明。|
|reason|失败原因|string|返回格式统一为：ERR.errcode，detailinfo，具体错误码信息，请参见[错误码说明](#错误码说明)。|

### 返回配置结果<a id="返回配置结果"></a>

该接口为通用接口，用于边缘侧向中心侧返回配置结果。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware | resource",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/config_result"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "topic": "profile",
    "percentage": "100%",
    "result": "success",
    "reason": "ERR.300, Parameter error:Parameter role error"
}
```

各元素定义如下：

|一级资源名称|描述|类型|取值范围|
|--|--|--|--|
|topic|订阅消息|string|32字节|
|percentage|收集进度|string|取值范围为0%~100%|
|result|收集结果|string|取值为success、failed、partial failed和processing|
|reason|失败原因|string|返回格式统一为：ERR.errcode，detailinfo，具体错误码信息，请参见[错误码说明](#错误码说明)。|

### 上报keepalive信息<a name="ZH-CN_TOPIC_0000001578489828"></a>

OM会按照heartbeat参数设置的时间间隔（默认时间间隔为15秒），定时上报一次keepalive信息，确保设备在线。

**消息实例**

```json
{
    "header" : {
        "msg_id" : "e33f7bff-a2fa-4f26-821b-37bdfa848b9d",
        "parent_msg_id": "",
        "timestamp" : 1642645840936,
        "sync": false
    },
    "route" : {
        "source" : "websocket",
        "group" : "resource",
        "operation" : "keepalive",
        "resource" : "node"
    },
    "content" : "ping"
}
```

### 上报heartbeat信息<a name="ZH-CN_TOPIC_0000001628729909"></a>

OM除了以上的keepalive信息外，使用的websocket协议还存在默认的ping、pong心跳帧机制，客户端会定期发送帧首部Opcode值为9的控制帧，服务端在收到此消息后会回复帧首部Opcode值为A的控制帧，用于判断连接是否正常。消息的payload数据是4字节随机数据，用于判断ping、pong消息是否配对。

**消息示例**

```json
{
    b"\xa2\x85\x91\x07"
}
```

**返回结果**

```json
{
    b"\xa2\x85\x91\x07"
}
```

### 上报对接FusionDirector根证书信息<a name="ZH-CN_TOPIC_0000001628849861"></a>

对接FusionDirector根证书信息需由FusionDirector侧主动发起查询，OM响应查询消息，在响应报文中携带FusionDirector根证书信息。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/cert_info"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "root_certificate":{
        "cert_is_full": true/false,
        "cert_lists":[{
            "cert_type":"FDRootCert",
            "cert_name":"",
            "issuer":"CN=xxx.ca.com, OU=IT, O=xxx, L=ShenZhen, S=GuangDong, C=CN",
            "subject":"CN=xxx.ca.com, OU=IT, O=xxx, L=ShenZhen, S=GuangDong, C=CN",
            "valid_not_before":"Jan 07 2017 GMT",
            "valid_not_after":"Jan 05 2027 GMT",
            "serial_number":"ff ff ff ff ff ff ff ff",
            "is_import_crl":false,
            "signature_algorithm":"sha256WithRSAEncryption",
            "fingerprint":"ffffffffffffffffffffffffff",
            "key_usage":"Signing, CRL Sign",
            "public_key_length_bits":2048
        }]
    }
}
```

元素定义如下：

<a name="zh-cn_topic_0000001396921694_table22383951"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001396921694_row18511700"><th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.1"><p id="zh-cn_topic_0000001396921694_p23052717"><a name="zh-cn_topic_0000001396921694_p23052717"></a><a name="zh-cn_topic_0000001396921694_p23052717"></a>一级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.2"><p id="zh-cn_topic_0000001396921694_p55330787"><a name="zh-cn_topic_0000001396921694_p55330787"></a><a name="zh-cn_topic_0000001396921694_p55330787"></a>二级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.3"><p id="zh-cn_topic_0000001396921694_p52608733"><a name="zh-cn_topic_0000001396921694_p52608733"></a><a name="zh-cn_topic_0000001396921694_p52608733"></a>三级资源名称</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.5.1.4"><p id="zh-cn_topic_0000001396921694_p33448963"><a name="zh-cn_topic_0000001396921694_p33448963"></a><a name="zh-cn_topic_0000001396921694_p33448963"></a>说明</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001396921694_row1890145561918"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p29011055121918"><a name="zh-cn_topic_0000001396921694_p29011055121918"></a><a name="zh-cn_topic_0000001396921694_p29011055121918"></a>root_certificate</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p119016558194"><a name="zh-cn_topic_0000001396921694_p119016558194"></a><a name="zh-cn_topic_0000001396921694_p119016558194"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p7901195517192"><a name="zh-cn_topic_0000001396921694_p7901195517192"></a><a name="zh-cn_topic_0000001396921694_p7901195517192"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p2317106122417"><a name="zh-cn_topic_0000001396921694_p2317106122417"></a><a name="zh-cn_topic_0000001396921694_p2317106122417"></a>含义：根证书信息</p>
<p id="zh-cn_topic_0000001396921694_p113171366247"><a name="zh-cn_topic_0000001396921694_p113171366247"></a><a name="zh-cn_topic_0000001396921694_p113171366247"></a>类型：array</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row59051218173317"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p14905161873313"><a name="zh-cn_topic_0000001396921694_p14905161873313"></a><a name="zh-cn_topic_0000001396921694_p14905161873313"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p6905191815331"><a name="zh-cn_topic_0000001396921694_p6905191815331"></a><a name="zh-cn_topic_0000001396921694_p6905191815331"></a>cert_is_full</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p69051218143317"><a name="zh-cn_topic_0000001396921694_p69051218143317"></a><a name="zh-cn_topic_0000001396921694_p69051218143317"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p1942118352337"><a name="zh-cn_topic_0000001396921694_p1942118352337"></a><a name="zh-cn_topic_0000001396921694_p1942118352337"></a>含义：证书是否已经达到最大导入数量</p>
<p id="zh-cn_topic_0000001396921694_p1842153593313"><a name="zh-cn_topic_0000001396921694_p1842153593313"></a><a name="zh-cn_topic_0000001396921694_p1842153593313"></a>类型：bool</p>
<p id="zh-cn_topic_0000001396921694_p194217353339"><a name="zh-cn_topic_0000001396921694_p194217353339"></a><a name="zh-cn_topic_0000001396921694_p194217353339"></a>取值：True/False</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row148971949191919"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p19897184961912"><a name="zh-cn_topic_0000001396921694_p19897184961912"></a><a name="zh-cn_topic_0000001396921694_p19897184961912"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p989716495195"><a name="zh-cn_topic_0000001396921694_p989716495195"></a><a name="zh-cn_topic_0000001396921694_p989716495195"></a>cert_lists</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p1289784913199"><a name="zh-cn_topic_0000001396921694_p1289784913199"></a><a name="zh-cn_topic_0000001396921694_p1289784913199"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p189669513257"><a name="zh-cn_topic_0000001396921694_p189669513257"></a><a name="zh-cn_topic_0000001396921694_p189669513257"></a>含义：证书列表信息</p>
<p id="zh-cn_topic_0000001396921694_p119661651102511"><a name="zh-cn_topic_0000001396921694_p119661651102511"></a><a name="zh-cn_topic_0000001396921694_p119661651102511"></a>类型：list</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row054101882619"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p16541718142613"><a name="zh-cn_topic_0000001396921694_p16541718142613"></a><a name="zh-cn_topic_0000001396921694_p16541718142613"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p1454181819264"><a name="zh-cn_topic_0000001396921694_p1454181819264"></a><a name="zh-cn_topic_0000001396921694_p1454181819264"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p155411218192617"><a name="zh-cn_topic_0000001396921694_p155411218192617"></a><a name="zh-cn_topic_0000001396921694_p155411218192617"></a>cert_type</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p2914123562713"><a name="zh-cn_topic_0000001396921694_p2914123562713"></a><a name="zh-cn_topic_0000001396921694_p2914123562713"></a>含义：证书类型</p>
<p id="zh-cn_topic_0000001396921694_p3914193572713"><a name="zh-cn_topic_0000001396921694_p3914193572713"></a><a name="zh-cn_topic_0000001396921694_p3914193572713"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p89141135192712"><a name="zh-cn_topic_0000001396921694_p89141135192712"></a><a name="zh-cn_topic_0000001396921694_p89141135192712"></a>取值：FDRootCert</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row5265114982611"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p926504915265"><a name="zh-cn_topic_0000001396921694_p926504915265"></a><a name="zh-cn_topic_0000001396921694_p926504915265"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p1265749152620"><a name="zh-cn_topic_0000001396921694_p1265749152620"></a><a name="zh-cn_topic_0000001396921694_p1265749152620"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p15265184962620"><a name="zh-cn_topic_0000001396921694_p15265184962620"></a><a name="zh-cn_topic_0000001396921694_p15265184962620"></a>cert_name</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p1213893083214"><a name="zh-cn_topic_0000001396921694_p1213893083214"></a><a name="zh-cn_topic_0000001396921694_p1213893083214"></a>含义：证书名称</p>
<p id="zh-cn_topic_0000001396921694_p41387304324"><a name="zh-cn_topic_0000001396921694_p41387304324"></a><a name="zh-cn_topic_0000001396921694_p41387304324"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p613853073215"><a name="zh-cn_topic_0000001396921694_p613853073215"></a><a name="zh-cn_topic_0000001396921694_p613853073215"></a>取值：4~64字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row124410190271"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p845171917274"><a name="zh-cn_topic_0000001396921694_p845171917274"></a><a name="zh-cn_topic_0000001396921694_p845171917274"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p134591917276"><a name="zh-cn_topic_0000001396921694_p134591917276"></a><a name="zh-cn_topic_0000001396921694_p134591917276"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p164501962718"><a name="zh-cn_topic_0000001396921694_p164501962718"></a><a name="zh-cn_topic_0000001396921694_p164501962718"></a>issuer</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p1441794043212"><a name="zh-cn_topic_0000001396921694_p1441794043212"></a><a name="zh-cn_topic_0000001396921694_p1441794043212"></a>含义：证书签发者</p>
<p id="zh-cn_topic_0000001396921694_p14177407322"><a name="zh-cn_topic_0000001396921694_p14177407322"></a><a name="zh-cn_topic_0000001396921694_p14177407322"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p11417194015322"><a name="zh-cn_topic_0000001396921694_p11417194015322"></a><a name="zh-cn_topic_0000001396921694_p11417194015322"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row15432172162717"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p1643292117278"><a name="zh-cn_topic_0000001396921694_p1643292117278"></a><a name="zh-cn_topic_0000001396921694_p1643292117278"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p16432112116275"><a name="zh-cn_topic_0000001396921694_p16432112116275"></a><a name="zh-cn_topic_0000001396921694_p16432112116275"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p343212172716"><a name="zh-cn_topic_0000001396921694_p343212172716"></a><a name="zh-cn_topic_0000001396921694_p343212172716"></a>subject</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p61971354133220"><a name="zh-cn_topic_0000001396921694_p61971354133220"></a><a name="zh-cn_topic_0000001396921694_p61971354133220"></a>含义：证书使用者</p>
<p id="zh-cn_topic_0000001396921694_p1819755413325"><a name="zh-cn_topic_0000001396921694_p1819755413325"></a><a name="zh-cn_topic_0000001396921694_p1819755413325"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p4197105414324"><a name="zh-cn_topic_0000001396921694_p4197105414324"></a><a name="zh-cn_topic_0000001396921694_p4197105414324"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row14951916132717"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p19495416132712"><a name="zh-cn_topic_0000001396921694_p19495416132712"></a><a name="zh-cn_topic_0000001396921694_p19495416132712"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p1349571617277"><a name="zh-cn_topic_0000001396921694_p1349571617277"></a><a name="zh-cn_topic_0000001396921694_p1349571617277"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p174958161276"><a name="zh-cn_topic_0000001396921694_p174958161276"></a><a name="zh-cn_topic_0000001396921694_p174958161276"></a>valid_not_before</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p207411610336"><a name="zh-cn_topic_0000001396921694_p207411610336"></a><a name="zh-cn_topic_0000001396921694_p207411610336"></a>含义：生效起始日期</p>
<p id="zh-cn_topic_0000001396921694_p97411860339"><a name="zh-cn_topic_0000001396921694_p97411860339"></a><a name="zh-cn_topic_0000001396921694_p97411860339"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p674116153314"><a name="zh-cn_topic_0000001396921694_p674116153314"></a><a name="zh-cn_topic_0000001396921694_p674116153314"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row17309171402719"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p1531021422716"><a name="zh-cn_topic_0000001396921694_p1531021422716"></a><a name="zh-cn_topic_0000001396921694_p1531021422716"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p0310171419270"><a name="zh-cn_topic_0000001396921694_p0310171419270"></a><a name="zh-cn_topic_0000001396921694_p0310171419270"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p1310171472710"><a name="zh-cn_topic_0000001396921694_p1310171472710"></a><a name="zh-cn_topic_0000001396921694_p1310171472710"></a>valid_not_after</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p326071613316"><a name="zh-cn_topic_0000001396921694_p326071613316"></a><a name="zh-cn_topic_0000001396921694_p326071613316"></a>含义：生效结束日期</p>
<p id="zh-cn_topic_0000001396921694_p16260121611339"><a name="zh-cn_topic_0000001396921694_p16260121611339"></a><a name="zh-cn_topic_0000001396921694_p16260121611339"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p626051613319"><a name="zh-cn_topic_0000001396921694_p626051613319"></a><a name="zh-cn_topic_0000001396921694_p626051613319"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row5771012122716"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p67701212279"><a name="zh-cn_topic_0000001396921694_p67701212279"></a><a name="zh-cn_topic_0000001396921694_p67701212279"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p18771812162712"><a name="zh-cn_topic_0000001396921694_p18771812162712"></a><a name="zh-cn_topic_0000001396921694_p18771812162712"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p57815129277"><a name="zh-cn_topic_0000001396921694_p57815129277"></a><a name="zh-cn_topic_0000001396921694_p57815129277"></a>serial_number</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p866932623316"><a name="zh-cn_topic_0000001396921694_p866932623316"></a><a name="zh-cn_topic_0000001396921694_p866932623316"></a>含义：证书序列号</p>
<p id="zh-cn_topic_0000001396921694_p866982673316"><a name="zh-cn_topic_0000001396921694_p866982673316"></a><a name="zh-cn_topic_0000001396921694_p866982673316"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p466962614335"><a name="zh-cn_topic_0000001396921694_p466962614335"></a><a name="zh-cn_topic_0000001396921694_p466962614335"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row491109112714"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p291289182712"><a name="zh-cn_topic_0000001396921694_p291289182712"></a><a name="zh-cn_topic_0000001396921694_p291289182712"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p7912109102712"><a name="zh-cn_topic_0000001396921694_p7912109102712"></a><a name="zh-cn_topic_0000001396921694_p7912109102712"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p14912196272"><a name="zh-cn_topic_0000001396921694_p14912196272"></a><a name="zh-cn_topic_0000001396921694_p14912196272"></a>is_import_crl</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p127651119133410"><a name="zh-cn_topic_0000001396921694_p127651119133410"></a><a name="zh-cn_topic_0000001396921694_p127651119133410"></a>含义：是否配置证书吊销列表</p>
<p id="zh-cn_topic_0000001396921694_p176512191349"><a name="zh-cn_topic_0000001396921694_p176512191349"></a><a name="zh-cn_topic_0000001396921694_p176512191349"></a>类型：bool</p>
<div class="p" id="zh-cn_topic_0000001396921694_p18765719123419"><a name="zh-cn_topic_0000001396921694_p18765719123419"></a><a name="zh-cn_topic_0000001396921694_p18765719123419"></a>取值：<a name="zh-cn_topic_0000001396921694_ul12991194233413"></a><a name="zh-cn_topic_0000001396921694_ul12991194233413"></a><ul id="zh-cn_topic_0000001396921694_ul12991194233413"><li>true：已配置证书吊销列表</li><li>false：未配置证书吊销列表</li></ul>
</div>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row1692111722716"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p992197182712"><a name="zh-cn_topic_0000001396921694_p992197182712"></a><a name="zh-cn_topic_0000001396921694_p992197182712"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p5921776271"><a name="zh-cn_topic_0000001396921694_p5921776271"></a><a name="zh-cn_topic_0000001396921694_p5921776271"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p189221877278"><a name="zh-cn_topic_0000001396921694_p189221877278"></a><a name="zh-cn_topic_0000001396921694_p189221877278"></a>signature_algorithm</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p106887558345"><a name="zh-cn_topic_0000001396921694_p106887558345"></a><a name="zh-cn_topic_0000001396921694_p106887558345"></a>含义：签名算法</p>
<p id="zh-cn_topic_0000001396921694_p06881255123413"><a name="zh-cn_topic_0000001396921694_p06881255123413"></a><a name="zh-cn_topic_0000001396921694_p06881255123413"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p20480756151413"><a name="zh-cn_topic_0000001396921694_p20480756151413"></a><a name="zh-cn_topic_0000001396921694_p20480756151413"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row10683105220262"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p106831452102613"><a name="zh-cn_topic_0000001396921694_p106831452102613"></a><a name="zh-cn_topic_0000001396921694_p106831452102613"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p136831952172613"><a name="zh-cn_topic_0000001396921694_p136831952172613"></a><a name="zh-cn_topic_0000001396921694_p136831952172613"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p768412529261"><a name="zh-cn_topic_0000001396921694_p768412529261"></a><a name="zh-cn_topic_0000001396921694_p768412529261"></a>fingerprint</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p18880169133513"><a name="zh-cn_topic_0000001396921694_p18880169133513"></a><a name="zh-cn_topic_0000001396921694_p18880169133513"></a>含义：指纹信息</p>
<p id="zh-cn_topic_0000001396921694_p98807911359"><a name="zh-cn_topic_0000001396921694_p98807911359"></a><a name="zh-cn_topic_0000001396921694_p98807911359"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p1594424718142"><a name="zh-cn_topic_0000001396921694_p1594424718142"></a><a name="zh-cn_topic_0000001396921694_p1594424718142"></a>取值：1024字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row84796483363"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p1147964811369"><a name="zh-cn_topic_0000001396921694_p1147964811369"></a><a name="zh-cn_topic_0000001396921694_p1147964811369"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p124791048103616"><a name="zh-cn_topic_0000001396921694_p124791048103616"></a><a name="zh-cn_topic_0000001396921694_p124791048103616"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p1047944893611"><a name="zh-cn_topic_0000001396921694_p1047944893611"></a><a name="zh-cn_topic_0000001396921694_p1047944893611"></a>key_usage</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p195261858153616"><a name="zh-cn_topic_0000001396921694_p195261858153616"></a><a name="zh-cn_topic_0000001396921694_p195261858153616"></a>含义：密钥用法</p>
<p id="zh-cn_topic_0000001396921694_p145261058103612"><a name="zh-cn_topic_0000001396921694_p145261058103612"></a><a name="zh-cn_topic_0000001396921694_p145261058103612"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p011005918137"><a name="zh-cn_topic_0000001396921694_p011005918137"></a><a name="zh-cn_topic_0000001396921694_p011005918137"></a>取值：256字节</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396921694_row961385518261"><td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396921694_p186131355192612"><a name="zh-cn_topic_0000001396921694_p186131355192612"></a><a name="zh-cn_topic_0000001396921694_p186131355192612"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396921694_p10614195510261"><a name="zh-cn_topic_0000001396921694_p10614195510261"></a><a name="zh-cn_topic_0000001396921694_p10614195510261"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396921694_p5614115513266"><a name="zh-cn_topic_0000001396921694_p5614115513266"></a><a name="zh-cn_topic_0000001396921694_p5614115513266"></a>public_key_length_bits</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396921694_p74312208354"><a name="zh-cn_topic_0000001396921694_p74312208354"></a><a name="zh-cn_topic_0000001396921694_p74312208354"></a>含义：公钥长度</p>
<p id="zh-cn_topic_0000001396921694_p643162043518"><a name="zh-cn_topic_0000001396921694_p643162043518"></a><a name="zh-cn_topic_0000001396921694_p643162043518"></a>类型：string</p>
<p id="zh-cn_topic_0000001396921694_p94312206356"><a name="zh-cn_topic_0000001396921694_p94312206356"></a><a name="zh-cn_topic_0000001396921694_p94312206356"></a>取值：256字节</p>
</td>
</tr>
</tbody>
</table>

## 下行消息接口<a name="ZH-CN_TOPIC_0000001577530676"></a>

OM接收从FusionDirector下发的配置信息并进行处理，部分接口会通过异步消息，通过上行消息通知FusionDirector配置结果。

### 配置导入<a name="ZH-CN_TOPIC_0000001578489776"></a>

配置导入功能包括：NTP服务配置，分区信息配置，主机名查询配置，域名解析配置、账号密码过期天数配置、近端访问配置、会话过期时间配置、证书过期提前告警时间配置、登录规则配置和无线网络配置。

分区配置会影响业务，建议在业务部署前完成配置。partitions/ntp\_server/device\_location等为配置消息字段。其中：

- 如果不带有partitions/ntp\_server等字段，则不对原有配置做修改。
- 如果带有partitions/ntp\_server等字段，且数组内容为空，表示删除原来所有的配置。
- 如果带有partitions/ntp\_server等字段，且数组内容不为空，表示删除原来配置，按新的配置内容重新配置。

partitions具体配置规则如下：

1. <a name="zh-cn_topic_0000001396761838_li4516167165810"></a>根据device\_location字段识别需要分区的设备；在device\_location指定范围的设备，先删除原来用户分区（系统分区不能删除）。不在device\_location指定范围的设备，保持原来配置。
2. capacity\_bytes的值小于512M，表示不创建分区，仅在[1](#zh-cn_topic_0000001396761838_li4516167165810)场景下，做删除分区使用；capacity\_bytes为0时，除capacity\_bytes和device\_location需要正确填写外，其它字段可以为空。
3. file\_system只能支持ext4，其它格式不支持。
4. mount\_path为分区挂载路径。该字段不能为空，如果为空，创建的分区无挂载路径，会导致无法正常使用。
5. 配置过程中出现失败，已经完成的配置不回滚，结果返回失败。
6. “/var/lib/docker”路径不允许删除，允许重新挂载；如果本次分区挂载路径不包含“/var/lib/docker”，不管本次是否要对“/var/lib/docker”所挂载的设备进行分区，“/var/lib/docker”挂载的分区都保留；反之，如果本次重新挂载“/var/lib/docker”，不管是否对“/var/lib/docker”原来挂载的设备进行分区，都会删除该分区。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "controller",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/profile"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "product": "Atlas 200I A2",
    "profile_name": "profile001",
    "config": {
        "ntp_server": {
            "service_enabled": true,
            "sync_net_manager": false,
            "preferred_server": "xx.xx.xx.xx",
            "alternate_server": "xx.xx.xx.xx"
        },
        "partitions": [{
            "capacity_bytes": 298999349248,
            "device": {
                "device_type": "SimpleStorage",
                "device_location": "HDD0"
            },
            "file_system": "ext4",
            "mount_path": "/home/data"
        }],
        "static_host_list": [{
            "ip_address": "xx.xx.xx.xx",
            "name": "fd.huawei.com"
        }],
        "name_server":[{
            "ip_address": "xx.xx.xx.xx"
        }],
        "security_policy":{
            "password_validity": "30",
            "web_access": true,
            "ssh_access": true,
            "session_timeout": 50,
            "cert_alarm_time": 20,
            "security_load": [{
                 "enable": "true",
                 "start_time": "00:00",
                 "end_time": "00:00",
                 "ip_addr": "xx.xx.xx.xx",
                 "mac_addr": "xx.xx.xx.xx.xx"
            }]
        },
        "lte_info": [
            {
                "apn_info": [
                    {
                        "apn_name": "123",
                        "apn_passwd": "",
                        "apn_user": "werfasew",
                        "auth_type": "2"
                    }
                ],
                "state_data": true,
                "state_lte": true
            }
        ]
    }
}
```

元素定义如下：

<a name="zh-cn_topic_0000001396761838_table57466186"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001396761838_row22120748"><th class="cellrowborder" valign="top" width="18.4981501849815%" id="mcps1.1.6.1.1"><p id="zh-cn_topic_0000001396761838_p46950130"><a name="zh-cn_topic_0000001396761838_p46950130"></a><a name="zh-cn_topic_0000001396761838_p46950130"></a>一级资源</p>
</th>
<th class="cellrowborder" valign="top" width="18.4981501849815%" id="mcps1.1.6.1.2"><p id="zh-cn_topic_0000001396761838_p44864185"><a name="zh-cn_topic_0000001396761838_p44864185"></a><a name="zh-cn_topic_0000001396761838_p44864185"></a>二级资源</p>
</th>
<th class="cellrowborder" valign="top" width="17.738226177382263%" id="mcps1.1.6.1.3"><p id="zh-cn_topic_0000001396761838_p10120360"><a name="zh-cn_topic_0000001396761838_p10120360"></a><a name="zh-cn_topic_0000001396761838_p10120360"></a>三级资源</p>
</th>
<th class="cellrowborder" valign="top" width="8.279172082791721%" id="mcps1.1.6.1.4"><p id="p7333641112613"><a name="p7333641112613"></a><a name="p7333641112613"></a>四级资源</p>
</th>
<th class="cellrowborder" valign="top" width="36.986301369863014%" id="mcps1.1.6.1.5"><p id="zh-cn_topic_0000001396761838_p29020705"><a name="zh-cn_topic_0000001396761838_p29020705"></a><a name="zh-cn_topic_0000001396761838_p29020705"></a>说明</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001396761838_row18773878"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p44289175"><a name="zh-cn_topic_0000001396761838_p44289175"></a><a name="zh-cn_topic_0000001396761838_p44289175"></a>product</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p30653461"><a name="zh-cn_topic_0000001396761838_p30653461"></a><a name="zh-cn_topic_0000001396761838_p30653461"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p67011287"><a name="zh-cn_topic_0000001396761838_p67011287"></a><a name="zh-cn_topic_0000001396761838_p67011287"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p5333114110263"><a name="p5333114110263"></a><a name="p5333114110263"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p30891081"><a name="zh-cn_topic_0000001396761838_p30891081"></a><a name="zh-cn_topic_0000001396761838_p30891081"></a>含义：产品名称</p>
<p id="zh-cn_topic_0000001396761838_p53314289202"><a name="zh-cn_topic_0000001396761838_p53314289202"></a><a name="zh-cn_topic_0000001396761838_p53314289202"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p1044482492118"><a name="zh-cn_topic_0000001396761838_p1044482492118"></a><a name="zh-cn_topic_0000001396761838_p1044482492118"></a>取值：1~64字节，支持数字、大小写字母、-、.、_和空格，不能包含“..”，且必须以数字或字母开头和结尾</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row1444760"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p49916750"><a name="zh-cn_topic_0000001396761838_p49916750"></a><a name="zh-cn_topic_0000001396761838_p49916750"></a>profile_name</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p16724956"><a name="zh-cn_topic_0000001396761838_p16724956"></a><a name="zh-cn_topic_0000001396761838_p16724956"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p12544235"><a name="zh-cn_topic_0000001396761838_p12544235"></a><a name="zh-cn_topic_0000001396761838_p12544235"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1533311415266"><a name="p1533311415266"></a><a name="p1533311415266"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p27264427"><a name="zh-cn_topic_0000001396761838_p27264427"></a><a name="zh-cn_topic_0000001396761838_p27264427"></a>含义：配置文件名称</p>
<p id="zh-cn_topic_0000001396761838_p174698517222"><a name="zh-cn_topic_0000001396761838_p174698517222"></a><a name="zh-cn_topic_0000001396761838_p174698517222"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p94699511220"><a name="zh-cn_topic_0000001396761838_p94699511220"></a><a name="zh-cn_topic_0000001396761838_p94699511220"></a>取值：1~32字节，支持数字、大小写字母、-、.、_，不能包含“..”，且必须以数字或字母开头和结尾</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row62623536"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p39341669"><a name="zh-cn_topic_0000001396761838_p39341669"></a><a name="zh-cn_topic_0000001396761838_p39341669"></a>config</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p32558606"><a name="zh-cn_topic_0000001396761838_p32558606"></a><a name="zh-cn_topic_0000001396761838_p32558606"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p20001428"><a name="zh-cn_topic_0000001396761838_p20001428"></a><a name="zh-cn_topic_0000001396761838_p20001428"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p433364111262"><a name="p433364111262"></a><a name="p433364111262"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p31540820"><a name="zh-cn_topic_0000001396761838_p31540820"></a><a name="zh-cn_topic_0000001396761838_p31540820"></a>含义：配置信息</p>
<p id="zh-cn_topic_0000001396761838_p912425582217"><a name="zh-cn_topic_0000001396761838_p912425582217"></a><a name="zh-cn_topic_0000001396761838_p912425582217"></a>类型：dict</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row48741925"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p55781875"><a name="zh-cn_topic_0000001396761838_p55781875"></a><a name="zh-cn_topic_0000001396761838_p55781875"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p22038001"><a name="zh-cn_topic_0000001396761838_p22038001"></a><a name="zh-cn_topic_0000001396761838_p22038001"></a>ntp_server</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p40247623"><a name="zh-cn_topic_0000001396761838_p40247623"></a><a name="zh-cn_topic_0000001396761838_p40247623"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p53331841182615"><a name="p53331841182615"></a><a name="p53331841182615"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p58386717"><a name="zh-cn_topic_0000001396761838_p58386717"></a><a name="zh-cn_topic_0000001396761838_p58386717"></a>含义：NTP服务配置</p>
<p id="zh-cn_topic_0000001396761838_p1267410416237"><a name="zh-cn_topic_0000001396761838_p1267410416237"></a><a name="zh-cn_topic_0000001396761838_p1267410416237"></a>类型：dict</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row26520167"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p649879"><a name="zh-cn_topic_0000001396761838_p649879"></a><a name="zh-cn_topic_0000001396761838_p649879"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p52640211"><a name="zh-cn_topic_0000001396761838_p52640211"></a><a name="zh-cn_topic_0000001396761838_p52640211"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p35998724"><a name="zh-cn_topic_0000001396761838_p35998724"></a><a name="zh-cn_topic_0000001396761838_p35998724"></a>service_enabled</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p8333741162611"><a name="p8333741162611"></a><a name="p8333741162611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p31542116"><a name="zh-cn_topic_0000001396761838_p31542116"></a><a name="zh-cn_topic_0000001396761838_p31542116"></a>含义：NTP使能开关</p>
<p id="zh-cn_topic_0000001396761838_p1733820182320"><a name="zh-cn_topic_0000001396761838_p1733820182320"></a><a name="zh-cn_topic_0000001396761838_p1733820182320"></a>类型：bool</p>
<p id="zh-cn_topic_0000001396761838_p193372014232"><a name="zh-cn_topic_0000001396761838_p193372014232"></a><a name="zh-cn_topic_0000001396761838_p193372014232"></a>取值：true或false</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row58151339"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p12638008"><a name="zh-cn_topic_0000001396761838_p12638008"></a><a name="zh-cn_topic_0000001396761838_p12638008"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p17045709"><a name="zh-cn_topic_0000001396761838_p17045709"></a><a name="zh-cn_topic_0000001396761838_p17045709"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p38525203"><a name="zh-cn_topic_0000001396761838_p38525203"></a><a name="zh-cn_topic_0000001396761838_p38525203"></a>sync_net_manager</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p63331241142619"><a name="p63331241142619"></a><a name="p63331241142619"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p31878864"><a name="zh-cn_topic_0000001396761838_p31878864"></a><a name="zh-cn_topic_0000001396761838_p31878864"></a>含义：是否同步网管时间</p>
<p id="zh-cn_topic_0000001396761838_p1151044319232"><a name="zh-cn_topic_0000001396761838_p1151044319232"></a><a name="zh-cn_topic_0000001396761838_p1151044319232"></a>类型：bool</p>
<p id="zh-cn_topic_0000001396761838_p25101243142314"><a name="zh-cn_topic_0000001396761838_p25101243142314"></a><a name="zh-cn_topic_0000001396761838_p25101243142314"></a>取值：</p>
<a name="zh-cn_topic_0000001396761838_ul876005312237"></a><a name="zh-cn_topic_0000001396761838_ul876005312237"></a><ul id="zh-cn_topic_0000001396761838_ul876005312237"><li>true：表示同步网管时间</li><li>false：表示不同步网管时间</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row11457841"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p55669957"><a name="zh-cn_topic_0000001396761838_p55669957"></a><a name="zh-cn_topic_0000001396761838_p55669957"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p12972669"><a name="zh-cn_topic_0000001396761838_p12972669"></a><a name="zh-cn_topic_0000001396761838_p12972669"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p44153282"><a name="zh-cn_topic_0000001396761838_p44153282"></a><a name="zh-cn_topic_0000001396761838_p44153282"></a>preferred_server</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1233314117262"><a name="p1233314117262"></a><a name="p1233314117262"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p47828241"><a name="zh-cn_topic_0000001396761838_p47828241"></a><a name="zh-cn_topic_0000001396761838_p47828241"></a>含义：首选NTP服务器</p>
<p id="zh-cn_topic_0000001396761838_p107501645247"><a name="zh-cn_topic_0000001396761838_p107501645247"></a><a name="zh-cn_topic_0000001396761838_p107501645247"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p157509432411"><a name="zh-cn_topic_0000001396761838_p157509432411"></a><a name="zh-cn_topic_0000001396761838_p157509432411"></a>取值：当前仅支持IPv4地址。</p>
<a name="zh-cn_topic_0000001396761838_ul1313551352416"></a><a name="zh-cn_topic_0000001396761838_ul1313551352416"></a><ul id="zh-cn_topic_0000001396761838_ul1313551352416"><li>如果该字段为空，表示删除配置</li><li>仅当service_enabled字段为true，并且sync_net_manager为false时该字段允许配置，同时不允许跟alternate_server使用相同的IP</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row377729"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p30596085"><a name="zh-cn_topic_0000001396761838_p30596085"></a><a name="zh-cn_topic_0000001396761838_p30596085"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p62363850"><a name="zh-cn_topic_0000001396761838_p62363850"></a><a name="zh-cn_topic_0000001396761838_p62363850"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p18307112"><a name="zh-cn_topic_0000001396761838_p18307112"></a><a name="zh-cn_topic_0000001396761838_p18307112"></a>alternate_server</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p8333104119268"><a name="p8333104119268"></a><a name="p8333104119268"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p55205276"><a name="zh-cn_topic_0000001396761838_p55205276"></a><a name="zh-cn_topic_0000001396761838_p55205276"></a>含义：备用NTP服务器</p>
<p id="zh-cn_topic_0000001396761838_p1058613310248"><a name="zh-cn_topic_0000001396761838_p1058613310248"></a><a name="zh-cn_topic_0000001396761838_p1058613310248"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p18586143315241"><a name="zh-cn_topic_0000001396761838_p18586143315241"></a><a name="zh-cn_topic_0000001396761838_p18586143315241"></a>取值：仅支持IPv4地址。</p>
<a name="zh-cn_topic_0000001396761838_ul185136431243"></a><a name="zh-cn_topic_0000001396761838_ul185136431243"></a><ul id="zh-cn_topic_0000001396761838_ul185136431243"><li>如果该字段为空，表示删除配置</li><li>仅当service_enabled字段为true，并且sync_net_manager为false时该字段允许配置，可以配置为空，但不允许跟preferred_server使用相同的IP</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row3302441"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p66171144"><a name="zh-cn_topic_0000001396761838_p66171144"></a><a name="zh-cn_topic_0000001396761838_p66171144"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p58262436"><a name="zh-cn_topic_0000001396761838_p58262436"></a><a name="zh-cn_topic_0000001396761838_p58262436"></a>partitions</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p21636836"><a name="zh-cn_topic_0000001396761838_p21636836"></a><a name="zh-cn_topic_0000001396761838_p21636836"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p123333419266"><a name="p123333419266"></a><a name="p123333419266"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p24034673"><a name="zh-cn_topic_0000001396761838_p24034673"></a><a name="zh-cn_topic_0000001396761838_p24034673"></a>含义：分区信息</p>
<p id="zh-cn_topic_0000001396761838_p711275814246"><a name="zh-cn_topic_0000001396761838_p711275814246"></a><a name="zh-cn_topic_0000001396761838_p711275814246"></a>取值：单个物理盘最多支持16个分区</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row5207424"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p19148163"><a name="zh-cn_topic_0000001396761838_p19148163"></a><a name="zh-cn_topic_0000001396761838_p19148163"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p7497408"><a name="zh-cn_topic_0000001396761838_p7497408"></a><a name="zh-cn_topic_0000001396761838_p7497408"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p3310301"><a name="zh-cn_topic_0000001396761838_p3310301"></a><a name="zh-cn_topic_0000001396761838_p3310301"></a>capacity_bytes</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p9333341152611"><a name="p9333341152611"></a><a name="p9333341152611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p42723544"><a name="zh-cn_topic_0000001396761838_p42723544"></a><a name="zh-cn_topic_0000001396761838_p42723544"></a>含义：分区容量</p>
<p id="zh-cn_topic_0000001396761838_p4767326192511"><a name="zh-cn_topic_0000001396761838_p4767326192511"></a><a name="zh-cn_topic_0000001396761838_p4767326192511"></a>类型：Int64</p>
<p id="zh-cn_topic_0000001396761838_p12767526192515"><a name="zh-cn_topic_0000001396761838_p12767526192515"></a><a name="zh-cn_topic_0000001396761838_p12767526192515"></a>取值：小于等于磁盘最大剩余空间，必须是MB的整数倍</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row26168421"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p39267319"><a name="zh-cn_topic_0000001396761838_p39267319"></a><a name="zh-cn_topic_0000001396761838_p39267319"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p26536286"><a name="zh-cn_topic_0000001396761838_p26536286"></a><a name="zh-cn_topic_0000001396761838_p26536286"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p1955586"><a name="zh-cn_topic_0000001396761838_p1955586"></a><a name="zh-cn_topic_0000001396761838_p1955586"></a>device</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1933394112264"><a name="p1933394112264"></a><a name="p1933394112264"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p12810015"><a name="zh-cn_topic_0000001396761838_p12810015"></a><a name="zh-cn_topic_0000001396761838_p12810015"></a>含义：存储设备</p>
<p id="zh-cn_topic_0000001396761838_p14963123912520"><a name="zh-cn_topic_0000001396761838_p14963123912520"></a><a name="zh-cn_topic_0000001396761838_p14963123912520"></a>类型：dict</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row34588159"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p50177466"><a name="zh-cn_topic_0000001396761838_p50177466"></a><a name="zh-cn_topic_0000001396761838_p50177466"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p37842950"><a name="zh-cn_topic_0000001396761838_p37842950"></a><a name="zh-cn_topic_0000001396761838_p37842950"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p45380106"><a name="zh-cn_topic_0000001396761838_p45380106"></a><a name="zh-cn_topic_0000001396761838_p45380106"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1833384112615"><a name="p1833384112615"></a><a name="p1833384112615"></a>device_type</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p43960413"><a name="zh-cn_topic_0000001396761838_p43960413"></a><a name="zh-cn_topic_0000001396761838_p43960413"></a>含义：磁盘媒介类型</p>
<p id="zh-cn_topic_0000001396761838_p689415310255"><a name="zh-cn_topic_0000001396761838_p689415310255"></a><a name="zh-cn_topic_0000001396761838_p689415310255"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p289410538255"><a name="zh-cn_topic_0000001396761838_p289410538255"></a><a name="zh-cn_topic_0000001396761838_p289410538255"></a>取值：仅支持SimpleStrorage和Volume</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row47577981"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p28611243"><a name="zh-cn_topic_0000001396761838_p28611243"></a><a name="zh-cn_topic_0000001396761838_p28611243"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p35809350"><a name="zh-cn_topic_0000001396761838_p35809350"></a><a name="zh-cn_topic_0000001396761838_p35809350"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p14876213"><a name="zh-cn_topic_0000001396761838_p14876213"></a><a name="zh-cn_topic_0000001396761838_p14876213"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p173331541162615"><a name="p173331541162615"></a><a name="p173331541162615"></a>device_location</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p26547505"><a name="zh-cn_topic_0000001396761838_p26547505"></a><a name="zh-cn_topic_0000001396761838_p26547505"></a>含义：磁盘位置</p>
<p id="zh-cn_topic_0000001396761838_p16811121019262"><a name="zh-cn_topic_0000001396761838_p16811121019262"></a><a name="zh-cn_topic_0000001396761838_p16811121019262"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p5811110162618"><a name="zh-cn_topic_0000001396761838_p5811110162618"></a><a name="zh-cn_topic_0000001396761838_p5811110162618"></a>取值：</p>
<a name="zh-cn_topic_0000001396761838_ul174771712616"></a><a name="zh-cn_topic_0000001396761838_ul174771712616"></a><ul id="zh-cn_topic_0000001396761838_ul174771712616"><li>如果device_type为Volume，该字段填Volume的Name</li><li>如果device_type为SimpleStorage，该字段填磁盘的物理位置</li><li>长度不超过256，支持数字、大小写字母和其他字符（-_.），不能包含..</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row7677910"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p17930993"><a name="zh-cn_topic_0000001396761838_p17930993"></a><a name="zh-cn_topic_0000001396761838_p17930993"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p43124341"><a name="zh-cn_topic_0000001396761838_p43124341"></a><a name="zh-cn_topic_0000001396761838_p43124341"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p3410696"><a name="zh-cn_topic_0000001396761838_p3410696"></a><a name="zh-cn_topic_0000001396761838_p3410696"></a>file_system</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p5333194112613"><a name="p5333194112613"></a><a name="p5333194112613"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p30331010"><a name="zh-cn_topic_0000001396761838_p30331010"></a><a name="zh-cn_topic_0000001396761838_p30331010"></a>含义：文件系统类型</p>
<p id="zh-cn_topic_0000001396761838_p7613182715263"><a name="zh-cn_topic_0000001396761838_p7613182715263"></a><a name="zh-cn_topic_0000001396761838_p7613182715263"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p8614162710260"><a name="zh-cn_topic_0000001396761838_p8614162710260"></a><a name="zh-cn_topic_0000001396761838_p8614162710260"></a>取值：当前仅支持ext4，不传时，默认为ext4</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row14462858"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p30640827"><a name="zh-cn_topic_0000001396761838_p30640827"></a><a name="zh-cn_topic_0000001396761838_p30640827"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p65987954"><a name="zh-cn_topic_0000001396761838_p65987954"></a><a name="zh-cn_topic_0000001396761838_p65987954"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p43424020"><a name="zh-cn_topic_0000001396761838_p43424020"></a><a name="zh-cn_topic_0000001396761838_p43424020"></a>mount_path</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1433320419268"><a name="p1433320419268"></a><a name="p1433320419268"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p27872063"><a name="zh-cn_topic_0000001396761838_p27872063"></a><a name="zh-cn_topic_0000001396761838_p27872063"></a>含义：分区挂载绝对路径</p>
<p id="zh-cn_topic_0000001396761838_p5620737122611"><a name="zh-cn_topic_0000001396761838_p5620737122611"></a><a name="zh-cn_topic_0000001396761838_p5620737122611"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p1620137182618"><a name="zh-cn_topic_0000001396761838_p1620137182618"></a><a name="zh-cn_topic_0000001396761838_p1620137182618"></a>取值：0~256字节，取值不为0时必须以'/'开头，字符串内不能带空格</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row39696990"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p61339649"><a name="zh-cn_topic_0000001396761838_p61339649"></a><a name="zh-cn_topic_0000001396761838_p61339649"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p2455652"><a name="zh-cn_topic_0000001396761838_p2455652"></a><a name="zh-cn_topic_0000001396761838_p2455652"></a>static_host_list</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p64690095"><a name="zh-cn_topic_0000001396761838_p64690095"></a><a name="zh-cn_topic_0000001396761838_p64690095"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1133354110267"><a name="p1133354110267"></a><a name="p1133354110267"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p35260959"><a name="zh-cn_topic_0000001396761838_p35260959"></a><a name="zh-cn_topic_0000001396761838_p35260959"></a>含义：主机名查询静态表</p>
<p id="zh-cn_topic_0000001396761838_p9599751142614"><a name="zh-cn_topic_0000001396761838_p9599751142614"></a><a name="zh-cn_topic_0000001396761838_p9599751142614"></a>取值：表示全量配置，直接覆盖系统已经配置的。</p>
<p id="zh-cn_topic_0000001396761838_p1625312172719"><a name="zh-cn_topic_0000001396761838_p1625312172719"></a><a name="zh-cn_topic_0000001396761838_p1625312172719"></a>最大支持配置128条，不包括系统自带localhost和预置的FusionDirector域名（可配置，未配置默认为fd.fusiondirector.huawei.com），这两部分不能修改和配置。</p>
<p id="zh-cn_topic_0000001396761838_p2025317118276"><a name="zh-cn_topic_0000001396761838_p2025317118276"></a><a name="zh-cn_topic_0000001396761838_p2025317118276"></a>允许多个IP对应同一个域名或一个域名对应多个IP，FusionDirector和设备均不做限制，由用户保证配置正确性。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row4804387"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p53611072"><a name="zh-cn_topic_0000001396761838_p53611072"></a><a name="zh-cn_topic_0000001396761838_p53611072"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p47529594"><a name="zh-cn_topic_0000001396761838_p47529594"></a><a name="zh-cn_topic_0000001396761838_p47529594"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p24691896"><a name="zh-cn_topic_0000001396761838_p24691896"></a><a name="zh-cn_topic_0000001396761838_p24691896"></a>ip_address</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p733364162619"><a name="p733364162619"></a><a name="p733364162619"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p2735458"><a name="zh-cn_topic_0000001396761838_p2735458"></a><a name="zh-cn_topic_0000001396761838_p2735458"></a>含义：IP地址</p>
<p id="zh-cn_topic_0000001396761838_p99521512274"><a name="zh-cn_topic_0000001396761838_p99521512274"></a><a name="zh-cn_topic_0000001396761838_p99521512274"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p89521522719"><a name="zh-cn_topic_0000001396761838_p89521522719"></a><a name="zh-cn_topic_0000001396761838_p89521522719"></a>取值：支持IPv4，允许配置相同的IP指向不同的域名/主机名</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row62153071"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p1233985"><a name="zh-cn_topic_0000001396761838_p1233985"></a><a name="zh-cn_topic_0000001396761838_p1233985"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p32843947"><a name="zh-cn_topic_0000001396761838_p32843947"></a><a name="zh-cn_topic_0000001396761838_p32843947"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p43114070"><a name="zh-cn_topic_0000001396761838_p43114070"></a><a name="zh-cn_topic_0000001396761838_p43114070"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1333384122610"><a name="p1333384122610"></a><a name="p1333384122610"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p7555725"><a name="zh-cn_topic_0000001396761838_p7555725"></a><a name="zh-cn_topic_0000001396761838_p7555725"></a>含义：域名/主机名</p>
<p id="zh-cn_topic_0000001396761838_p5160142502718"><a name="zh-cn_topic_0000001396761838_p5160142502718"></a><a name="zh-cn_topic_0000001396761838_p5160142502718"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p416062510273"><a name="zh-cn_topic_0000001396761838_p416062510273"></a><a name="zh-cn_topic_0000001396761838_p416062510273"></a>取值：域名可以由英文字母（不区分大小写）、数字、连接符"-"以及"."组成，最长支持253字节，此处不区分主机名还是域名。</p>
<p id="zh-cn_topic_0000001396761838_p3384132152710"><a name="zh-cn_topic_0000001396761838_p3384132152710"></a><a name="zh-cn_topic_0000001396761838_p3384132152710"></a>localhost、localhost.localdomain、localhost4、localhost4.localdomain4、localhost6、localhost6.localdomain6、FusionDirector域名（可配置，未配置默认为fd.fusiondirector.huawei.com），这几个名称保留，不能配置，不区分大小写。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row30788716"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p10858082"><a name="zh-cn_topic_0000001396761838_p10858082"></a><a name="zh-cn_topic_0000001396761838_p10858082"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p7089456"><a name="zh-cn_topic_0000001396761838_p7089456"></a><a name="zh-cn_topic_0000001396761838_p7089456"></a>name_server</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p37375103"><a name="zh-cn_topic_0000001396761838_p37375103"></a><a name="zh-cn_topic_0000001396761838_p37375103"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p033319419262"><a name="p033319419262"></a><a name="p033319419262"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p2267583"><a name="zh-cn_topic_0000001396761838_p2267583"></a><a name="zh-cn_topic_0000001396761838_p2267583"></a>含义：域名解析服务器</p>
<p id="zh-cn_topic_0000001396761838_p146484572718"><a name="zh-cn_topic_0000001396761838_p146484572718"></a><a name="zh-cn_topic_0000001396761838_p146484572718"></a>取值：表示全量配置，直接覆盖系统已经配置的。最多支持配置3条</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row16336996"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p48228313"><a name="zh-cn_topic_0000001396761838_p48228313"></a><a name="zh-cn_topic_0000001396761838_p48228313"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p14179266"><a name="zh-cn_topic_0000001396761838_p14179266"></a><a name="zh-cn_topic_0000001396761838_p14179266"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p7669896"><a name="zh-cn_topic_0000001396761838_p7669896"></a><a name="zh-cn_topic_0000001396761838_p7669896"></a>ip_address</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1333141162614"><a name="p1333141162614"></a><a name="p1333141162614"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p57649352"><a name="zh-cn_topic_0000001396761838_p57649352"></a><a name="zh-cn_topic_0000001396761838_p57649352"></a>含义：IP地址</p>
<p id="zh-cn_topic_0000001396761838_p1247692132819"><a name="zh-cn_topic_0000001396761838_p1247692132819"></a><a name="zh-cn_topic_0000001396761838_p1247692132819"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p247613216286"><a name="zh-cn_topic_0000001396761838_p247613216286"></a><a name="zh-cn_topic_0000001396761838_p247613216286"></a>取值：支持IPv4地址</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row73111234314"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p173114216438"><a name="zh-cn_topic_0000001396761838_p173114216438"></a><a name="zh-cn_topic_0000001396761838_p173114216438"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p19316234315"><a name="zh-cn_topic_0000001396761838_p19316234315"></a><a name="zh-cn_topic_0000001396761838_p19316234315"></a>security_policy</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p19318294314"><a name="zh-cn_topic_0000001396761838_p19318294314"></a><a name="zh-cn_topic_0000001396761838_p19318294314"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p733394118269"><a name="p733394118269"></a><a name="p733394118269"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p1319218435"><a name="zh-cn_topic_0000001396761838_p1319218435"></a><a name="zh-cn_topic_0000001396761838_p1319218435"></a>账号和密码过期天数</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row1191153144311"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p891219319432"><a name="zh-cn_topic_0000001396761838_p891219319432"></a><a name="zh-cn_topic_0000001396761838_p891219319432"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p179121734437"><a name="zh-cn_topic_0000001396761838_p179121734437"></a><a name="zh-cn_topic_0000001396761838_p179121734437"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p29124314433"><a name="zh-cn_topic_0000001396761838_p29124314433"></a><a name="zh-cn_topic_0000001396761838_p29124314433"></a>password_validity</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p03338419268"><a name="p03338419268"></a><a name="p03338419268"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p4267624124413"><a name="zh-cn_topic_0000001396761838_p4267624124413"></a><a name="zh-cn_topic_0000001396761838_p4267624124413"></a>含义：账号和密码过期天数</p>
<p id="zh-cn_topic_0000001396761838_p19267424164417"><a name="zh-cn_topic_0000001396761838_p19267424164417"></a><a name="zh-cn_topic_0000001396761838_p19267424164417"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761838_p13267424104419"><a name="zh-cn_topic_0000001396761838_p13267424104419"></a><a name="zh-cn_topic_0000001396761838_p13267424104419"></a>取值：范围0-365，仅支持数字。</p>
</td>
</tr>
<tr id="row125841172611"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p112594113263"><a name="p112594113263"></a><a name="p112594113263"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p10259117264"><a name="p10259117264"></a><a name="p10259117264"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p1125931122613"><a name="p1125931122613"></a><a name="p1125931122613"></a>web_access</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p633464152611"><a name="p633464152611"></a><a name="p633464152611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p12259015266"><a name="p12259015266"></a><a name="p12259015266"></a>含义：是否打开web近端接入开关，不传该字段表示不更改此项配置</p>
<p id="p161815398307"><a name="p161815398307"></a><a name="p161815398307"></a>类型：bool</p>
<p id="p197742443308"><a name="p197742443308"></a><a name="p197742443308"></a>取值：true或者false</p>
</td>
</tr>
<tr id="row1410213516262"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p11102145112619"><a name="p11102145112619"></a><a name="p11102145112619"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p1510210572616"><a name="p1510210572616"></a><a name="p1510210572616"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p310320520264"><a name="p310320520264"></a><a name="p310320520264"></a>ssh_access</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1133411415268"><a name="p1133411415268"></a><a name="p1133411415268"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p21031256266"><a name="p21031256266"></a><a name="p21031256266"></a>含义：是否打开ssh近端接入开关，不传该字段表示不更改此项配置</p>
<p id="p6789759103015"><a name="p6789759103015"></a><a name="p6789759103015"></a>类型：bool</p>
<p id="p1030615394318"><a name="p1030615394318"></a><a name="p1030615394318"></a>取值：true或者false</p>
</td>
</tr>
<tr id="row1065412812619"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p1465417882618"><a name="p1465417882618"></a><a name="p1465417882618"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p365416817266"><a name="p365416817266"></a><a name="p365416817266"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p46545819268"><a name="p46545819268"></a><a name="p46545819268"></a>session_timeout</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p4334134112611"><a name="p4334134112611"></a><a name="p4334134112611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p1765418812264"><a name="p1765418812264"></a><a name="p1765418812264"></a>含义：会话超时时间，单位为分钟</p>
<p id="p4189185511217"><a name="p4189185511217"></a><a name="p4189185511217"></a>类型：string</p>
<p id="p10189155122117"><a name="p10189155122117"></a><a name="p10189155122117"></a>取值：5~120</p>
</td>
</tr>
<tr id="row113821712162617"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p18383191213261"><a name="p18383191213261"></a><a name="p18383191213261"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p738391222619"><a name="p738391222619"></a><a name="p738391222619"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p438312128267"><a name="p438312128267"></a><a name="p438312128267"></a>cert_alarm_time</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p7334194114265"><a name="p7334194114265"></a><a name="p7334194114265"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p538331210267"><a name="p538331210267"></a><a name="p538331210267"></a>含义：证书过期提前告警时间，单位为天</p>
<p id="p357913569216"><a name="p357913569216"></a><a name="p357913569216"></a>类型：string</p>
<p id="p357995692110"><a name="p357995692110"></a><a name="p357995692110"></a>取值：7~180</p>
</td>
</tr>
<tr id="row1248081742617"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p12480111792614"><a name="p12480111792614"></a><a name="p12480111792614"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p184803176267"><a name="p184803176267"></a><a name="p184803176267"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p174803173265"><a name="p174803173265"></a><a name="p174803173265"></a>security_load</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p9334104152615"><a name="p9334104152615"></a><a name="p9334104152615"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p448081715265"><a name="p448081715265"></a><a name="p448081715265"></a>含义：登录规则</p>
<p id="p64341359162114"><a name="p64341359162114"></a><a name="p64341359162114"></a>类型：list</p>
<p id="p19434859182120"><a name="p19434859182120"></a><a name="p19434859182120"></a>取值：最大支持30个</p>
</td>
</tr>
<tr id="row8388721112920"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p17388182112912"><a name="p17388182112912"></a><a name="p17388182112912"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p19388102111297"><a name="p19388102111297"></a><a name="p19388102111297"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p338917211296"><a name="p338917211296"></a><a name="p338917211296"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p638912119297"><a name="p638912119297"></a><a name="p638912119297"></a>enable</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p1638914212291"><a name="p1638914212291"></a><a name="p1638914212291"></a>含义：是否使能本条登录规则黑名单</p>
<p id="p169321282218"><a name="p169321282218"></a><a name="p169321282218"></a>类型：string</p>
<p id="p16693171217229"><a name="p16693171217229"></a><a name="p16693171217229"></a>取值：true or false</p>
</td>
</tr>
<tr id="row14137525172918"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p213710254297"><a name="p213710254297"></a><a name="p213710254297"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p11137172516296"><a name="p11137172516296"></a><a name="p11137172516296"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p4137162510293"><a name="p4137162510293"></a><a name="p4137162510293"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p7137122572913"><a name="p7137122572913"></a><a name="p7137122572913"></a>start_time</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p15137925132917"><a name="p15137925132917"></a><a name="p15137925132917"></a>含义：登录规则黑名单生效开始时间</p>
<p id="p62216149229"><a name="p62216149229"></a><a name="p62216149229"></a>类型：string</p>
<p id="p12210141226"><a name="p12210141226"></a><a name="p12210141226"></a>取值：合法时间格式</p>
</td>
</tr>
<tr id="row996514272296"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p189650279297"><a name="p189650279297"></a><a name="p189650279297"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p14965727202912"><a name="p14965727202912"></a><a name="p14965727202912"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p796552716296"><a name="p796552716296"></a><a name="p796552716296"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1696542732912"><a name="p1696542732912"></a><a name="p1696542732912"></a>end_time</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p89661027142912"><a name="p89661027142912"></a><a name="p89661027142912"></a>含义：登录规则黑名单生效结束时间</p>
<p id="p1534411517221"><a name="p1534411517221"></a><a name="p1534411517221"></a>类型：string</p>
<p id="p93441915152215"><a name="p93441915152215"></a><a name="p93441915152215"></a>取值：合法时间格式</p>
</td>
</tr>
<tr id="row17124531182910"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p61241631142914"><a name="p61241631142914"></a><a name="p61241631142914"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p18124163116298"><a name="p18124163116298"></a><a name="p18124163116298"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p1812420319291"><a name="p1812420319291"></a><a name="p1812420319291"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1012443110293"><a name="p1012443110293"></a><a name="p1012443110293"></a>ip_addr</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p5124231162910"><a name="p5124231162910"></a><a name="p5124231162910"></a>含义：允许登录的IP地址</p>
<p id="p42446171224"><a name="p42446171224"></a><a name="p42446171224"></a>类型：string</p>
<p id="p224481712219"><a name="p224481712219"></a><a name="p224481712219"></a>取值：合法的IPv4地址，可以有xxx.xxx.xxx.xxx或者xxx.xxx.xxx.xxx/mask两种形式，xxx.xxx.xxx.xxx为单个IP地址，xxx.xxx.xxx.xxx/mask为IP地址段</p>
</td>
</tr>
<tr id="row17407145452912"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="p4407954182913"><a name="p4407954182913"></a><a name="p4407954182913"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="p240775432910"><a name="p240775432910"></a><a name="p240775432910"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="p144081954142918"><a name="p144081954142918"></a><a name="p144081954142918"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1940845432920"><a name="p1940845432920"></a><a name="p1940845432920"></a>mac_addr</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="p540812542295"><a name="p540812542295"></a><a name="p540812542295"></a>含义：允许登录的mac地址</p>
<p id="p1331719183223"><a name="p1331719183223"></a><a name="p1331719183223"></a>类型：string</p>
<p id="p331711188223"><a name="p331711188223"></a><a name="p331711188223"></a>取值：合法的mac地址</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row354193814426"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p195443894218"><a name="zh-cn_topic_0000001396761838_p195443894218"></a><a name="zh-cn_topic_0000001396761838_p195443894218"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p1054638104219"><a name="zh-cn_topic_0000001396761838_p1054638104219"></a><a name="zh-cn_topic_0000001396761838_p1054638104219"></a>lte_info</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p65413388422"><a name="zh-cn_topic_0000001396761838_p65413388422"></a><a name="zh-cn_topic_0000001396761838_p65413388422"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p33341241132620"><a name="p33341241132620"></a><a name="p33341241132620"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p105463819426"><a name="zh-cn_topic_0000001396761838_p105463819426"></a><a name="zh-cn_topic_0000001396761838_p105463819426"></a>含义：无线网络配置</p>
<p id="zh-cn_topic_0000001396761838_p2631198134417"><a name="zh-cn_topic_0000001396761838_p2631198134417"></a><a name="zh-cn_topic_0000001396761838_p2631198134417"></a>取值：有且只有1条配置</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row9551938114219"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p14557388420"><a name="zh-cn_topic_0000001396761838_p14557388420"></a><a name="zh-cn_topic_0000001396761838_p14557388420"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p135511389427"><a name="zh-cn_topic_0000001396761838_p135511389427"></a><a name="zh-cn_topic_0000001396761838_p135511389427"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p55517382429"><a name="zh-cn_topic_0000001396761838_p55517382429"></a><a name="zh-cn_topic_0000001396761838_p55517382429"></a>state_lte</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p133343416261"><a name="p133343416261"></a><a name="p133343416261"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p5559388421"><a name="zh-cn_topic_0000001396761838_p5559388421"></a><a name="zh-cn_topic_0000001396761838_p5559388421"></a>含义：无线网络的开关状态。</p>
<p id="zh-cn_topic_0000001396761838_p4790144114615"><a name="zh-cn_topic_0000001396761838_p4790144114615"></a><a name="zh-cn_topic_0000001396761838_p4790144114615"></a>类型：bool</p>
<a name="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_ul362575195815"></a><a name="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_ul362575195815"></a><ul id="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_ul362575195815"><li>true：打开。</li><li>false：关闭。</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row205563814427"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p155563854211"><a name="zh-cn_topic_0000001396761838_p155563854211"></a><a name="zh-cn_topic_0000001396761838_p155563854211"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p195512381429"><a name="zh-cn_topic_0000001396761838_p195512381429"></a><a name="zh-cn_topic_0000001396761838_p195512381429"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p16558386428"><a name="zh-cn_topic_0000001396761838_p16558386428"></a><a name="zh-cn_topic_0000001396761838_p16558386428"></a>state_data</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p7334144152611"><a name="p7334144152611"></a><a name="p7334144152611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_p16425824154317"><a name="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_p16425824154317"></a><a name="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_p16425824154317"></a>含义：移动数据的开关状态。</p>
<p id="zh-cn_topic_0000001396761838_p1233172518474"><a name="zh-cn_topic_0000001396761838_p1233172518474"></a><a name="zh-cn_topic_0000001396761838_p1233172518474"></a>类型：bool</p>
<a name="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_ul92951745105814"></a><a name="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_ul92951745105814"></a><ul id="zh-cn_topic_0000001396761838_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_ul92951745105814"><li>true：打开。</li><li>false：关闭。</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row55514381423"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p955103810425"><a name="zh-cn_topic_0000001396761838_p955103810425"></a><a name="zh-cn_topic_0000001396761838_p955103810425"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p555183824212"><a name="zh-cn_topic_0000001396761838_p555183824212"></a><a name="zh-cn_topic_0000001396761838_p555183824212"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p1555133814215"><a name="zh-cn_topic_0000001396761838_p1555133814215"></a><a name="zh-cn_topic_0000001396761838_p1555133814215"></a>apn_info</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p12334164132611"><a name="p12334164132611"></a><a name="p12334164132611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p105573817424"><a name="zh-cn_topic_0000001396761838_p105573817424"></a><a name="zh-cn_topic_0000001396761838_p105573817424"></a>含义：APN配置信息</p>
<p id="zh-cn_topic_0000001396761838_p715112580476"><a name="zh-cn_topic_0000001396761838_p715112580476"></a><a name="zh-cn_topic_0000001396761838_p715112580476"></a>类型：数组。</p>
<p id="zh-cn_topic_0000001396761838_p314112231484"><a name="zh-cn_topic_0000001396761838_p314112231484"></a><a name="zh-cn_topic_0000001396761838_p314112231484"></a>取值：最多1条配置。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row23972018496"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p23992018497"><a name="zh-cn_topic_0000001396761838_p23992018497"></a><a name="zh-cn_topic_0000001396761838_p23992018497"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p1539220114918"><a name="zh-cn_topic_0000001396761838_p1539220114918"></a><a name="zh-cn_topic_0000001396761838_p1539220114918"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p23922017491"><a name="zh-cn_topic_0000001396761838_p23922017491"></a><a name="zh-cn_topic_0000001396761838_p23922017491"></a>apn_info[].apn_name</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1233484112611"><a name="p1233484112611"></a><a name="p1233484112611"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p19490143218529"><a name="zh-cn_topic_0000001396761838_p19490143218529"></a><a name="zh-cn_topic_0000001396761838_p19490143218529"></a>含义：拨号时使用的APN名称。</p>
<p id="zh-cn_topic_0000001396761838_p27211939175219"><a name="zh-cn_topic_0000001396761838_p27211939175219"></a><a name="zh-cn_topic_0000001396761838_p27211939175219"></a>类型：字符串。</p>
<p id="zh-cn_topic_0000001396761838_p7391620114911"><a name="zh-cn_topic_0000001396761838_p7391620114911"></a><a name="zh-cn_topic_0000001396761838_p7391620114911"></a>取值：可由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（_-.@）组成，最大长度为39字符。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row16398205497"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p63962024917"><a name="zh-cn_topic_0000001396761838_p63962024917"></a><a name="zh-cn_topic_0000001396761838_p63962024917"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p539152094910"><a name="zh-cn_topic_0000001396761838_p539152094910"></a><a name="zh-cn_topic_0000001396761838_p539152094910"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p1339220144911"><a name="zh-cn_topic_0000001396761838_p1339220144911"></a><a name="zh-cn_topic_0000001396761838_p1339220144911"></a>apn_info[].apn_passwd</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p16334184110268"><a name="p16334184110268"></a><a name="p16334184110268"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p113972074915"><a name="zh-cn_topic_0000001396761838_p113972074915"></a><a name="zh-cn_topic_0000001396761838_p113972074915"></a>含义：拨号时使用的APN密码。</p>
<p id="zh-cn_topic_0000001396761838_p127566131523"><a name="zh-cn_topic_0000001396761838_p127566131523"></a><a name="zh-cn_topic_0000001396761838_p127566131523"></a>类型：字符串。</p>
<p id="zh-cn_topic_0000001396761838_p52561182521"><a name="zh-cn_topic_0000001396761838_p52561182521"></a><a name="zh-cn_topic_0000001396761838_p52561182521"></a>取值：最大长度为64字符，由数字、大小写字母及除英文逗号和英文引号的其他英文特殊符号组成。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row193972020490"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p13398206499"><a name="zh-cn_topic_0000001396761838_p13398206499"></a><a name="zh-cn_topic_0000001396761838_p13398206499"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p139202018497"><a name="zh-cn_topic_0000001396761838_p139202018497"></a><a name="zh-cn_topic_0000001396761838_p139202018497"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p16391620104913"><a name="zh-cn_topic_0000001396761838_p16391620104913"></a><a name="zh-cn_topic_0000001396761838_p16391620104913"></a>apn_info[].apn_user</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p1833484114266"><a name="p1833484114266"></a><a name="p1833484114266"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p1239172034913"><a name="zh-cn_topic_0000001396761838_p1239172034913"></a><a name="zh-cn_topic_0000001396761838_p1239172034913"></a>含义：拨号时使用的APN用户名。</p>
<p id="zh-cn_topic_0000001396761838_p47391552125311"><a name="zh-cn_topic_0000001396761838_p47391552125311"></a><a name="zh-cn_topic_0000001396761838_p47391552125311"></a>类型：字符串。</p>
<p id="zh-cn_topic_0000001396761838_p17270175615319"><a name="zh-cn_topic_0000001396761838_p17270175615319"></a><a name="zh-cn_topic_0000001396761838_p17270175615319"></a>取值：可由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（-_.@）组成，最大长度为64字符。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761838_row83916201493"><td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001396761838_p93932054916"><a name="zh-cn_topic_0000001396761838_p93932054916"></a><a name="zh-cn_topic_0000001396761838_p93932054916"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="18.4981501849815%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001396761838_p73992015498"><a name="zh-cn_topic_0000001396761838_p73992015498"></a><a name="zh-cn_topic_0000001396761838_p73992015498"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="17.738226177382263%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001396761838_p83992044916"><a name="zh-cn_topic_0000001396761838_p83992044916"></a><a name="zh-cn_topic_0000001396761838_p83992044916"></a>apn_info[].auth_type</p>
</td>
<td class="cellrowborder" valign="top" width="8.279172082791721%" headers="mcps1.1.6.1.4 "><p id="p733444182612"><a name="p733444182612"></a><a name="p733444182612"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="36.986301369863014%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001396761838_p1239102044914"><a name="zh-cn_topic_0000001396761838_p1239102044914"></a><a name="zh-cn_topic_0000001396761838_p1239102044914"></a>含义：身份验证类型。</p>
<p id="zh-cn_topic_0000001396761838_p21821617135420"><a name="zh-cn_topic_0000001396761838_p21821617135420"></a><a name="zh-cn_topic_0000001396761838_p21821617135420"></a>类型：字符串。</p>
<p id="zh-cn_topic_0000001396761838_p88411919464"><a name="zh-cn_topic_0000001396761838_p88411919464"></a><a name="zh-cn_topic_0000001396761838_p88411919464"></a>取值为0、1、2、3，其含义分别为：</p>
<a name="zh-cn_topic_0000001396761838_ul10841394462"></a><a name="zh-cn_topic_0000001396761838_ul10841394462"></a><ul id="zh-cn_topic_0000001396761838_ul10841394462"><li>0：NONE</li><li>1：PAP</li><li>2：CHAP</li><li>3：PAP or CHAP</li></ul>
<div class="note" id="note19237556151912"><a name="note19237556151912"></a><a name="note19237556151912"></a><span class="notetitle">  [!NOTE] 说明 </span><div class="notebody"><p id="p9237155614197"><a name="p9237155614197"></a><a name="p9237155614197"></a>4G模块支持的auth_type取值为0、1、2；5G模块支持的auth_type的取值为0、1、2、3。</p>
</div></div>
</td>
</tr>
</tbody>
</table>

**返回结果**

```json
{
    "header":{
        "msg_id":"02bb6421-45b4-4f0c-914c-ef3183a37000",
        "parent_msg_id": "",
        "timestamp":1652278267211,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/profile"
    },
    "content":"SUCCESS"|"FAILED: input param check failed"
}
```

### 配置生效<a name="ZH-CN_TOPIC_0000001578489836"></a>

该配置接口通过[上报配置生效进度](#上报配置生效进度)章节的上行接口异步返回配置生效进度和结果。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "controller",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/profile_effect"
    },
    "content": {
    }
}
```

**参数说明**

content:

```json
{
    "profile_name": "profile001"
}
```

通过系统状态中的“inactive\_configuration”字段检查配置是否生效成功。

元素定义如下：

|一级资源|描述|类型|取值范围|
|--|--|--|--|
|profile_name|配置文件名称|string|仅支持[a-z0-9A-Z-_.]，字符长度为1~32，不能包含“..”。|

**返回结果**

```json
{
    "header":{
        "msg_id":"02bb6421-45b4-4f0c-914c-ef3183a37000",
        "parent_msg_id": "",
        "timestamp":1652278267211,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/profile_effect"
    },
    "content":"{"profile_name": "/home/data/config/redfish/4997c320618ff4c1282da3cb8cdf1c.prf", "percentage": "100%", "result": "success", "reason": ""}"
}
```

### 修改自定义电子标签<a name="ZH-CN_TOPIC_0000001628490513"></a>

修改自定义电子标签信息，可通过[上报系统静态信息](#上报系统静态信息)检查是否修改成功。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/tag"
    },
    "content": {
    }
}
```

**参数说明**

content:

```json
{
    "asset_tag": "xxxxx"
}
```

通过通用接口返回结果，成功触发一次静态信息上报。上层超时时间建议配置为60s。

元素定义如下：

|一级资源|描述|类型|取值范围|
|--|--|--|--|
|asset_tag|自定义电子标签信息|string|取值范围为1~256字节，不允许为空，不支持中文，必须是可见的ASCII字符，包括a~z，A~Z，0~9和英文标点符号（即ASCII码值从0x20～0x7E的字符）|

**返回结果**

```json
{
    "header":{
        "msg_id":"8a44d9c0-05c9-4325-b530-fa5c4a5145f2",
        "parent_msg_id": "",
        "timestamp":1652269263083,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/config_result"
    },
    "content":"{"topic": "tag", "percentage": "100%", "result": "success", "reason": ""}"
}
```

### 复位主机系统<a name="ZH-CN_TOPIC_0000001578449868"></a>

复位主机系统，当前支持平滑复位一种方式。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/restart"
    },
    "content": {
        "restart_method": "Graceful"
    }
}
```

**参数说明**

content:

```json
{
    "restart_method": "Graceful"
}
```

元素定义如下：

|一级资源|描述|类型|取值范围|
|--|--|--|--|
|restart_method|复位方式|string|取值为Graceful，表示平滑复位。<br>当系统进行升级和配置导入等动作时，如果下发Graceful进行复位，复位系统会失败。</br> [!NOTE] 说明<br>在执行升级、升级生效、系统复位和恢复出厂操作时，再执行系统复位操作，系统复位操作会执行失败。</br>|

**返回结果**

```json
{
    "header":{
        "msg_id":"14885400-5b70-4539-99c1-56c0a4404aa7",
        "parent_msg_id": "",
        "timestamp":1652278452956,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/restart_result"
    },
    "content":"{"restartable": "true", "reason": "System is restartable"}"
}
```

### 固件升级<a name="ZH-CN_TOPIC_0000001577810536"></a>

异步返回升级进度和结果。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/install"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "member_list": [{
        "operator": "install",
        "type": "firmware",
        "install_method": "force | normal",
        "enable_method": "now | delay",
        "name": "firmware",
        "https_server": {
            "image": "GET https://FDAddr:port/fw.zip",
            "user_name": "userName",
            "password": "password"
        },
        "check_type": "sha256",
        "check_code": "xxxxxxxxxxxxx"
    }]
}
```

元素定义如下：

<a name="zh-cn_topic_0000001396761846_table54056813713"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001396761846_row134051181473"><th class="cellrowborder" valign="top" width="19.56%" id="mcps1.1.5.1.1"><p id="zh-cn_topic_0000001396761846_p32826444"><a name="zh-cn_topic_0000001396761846_p32826444"></a><a name="zh-cn_topic_0000001396761846_p32826444"></a>一级资源</p>
</th>
<th class="cellrowborder" valign="top" width="20.44%" id="mcps1.1.5.1.2"><p id="zh-cn_topic_0000001396761846_p2052333676"><a name="zh-cn_topic_0000001396761846_p2052333676"></a><a name="zh-cn_topic_0000001396761846_p2052333676"></a>二级资源</p>
</th>
<th class="cellrowborder" valign="top" width="20%" id="mcps1.1.5.1.3"><p id="zh-cn_topic_0000001396761846_p87041510113416"><a name="zh-cn_topic_0000001396761846_p87041510113416"></a><a name="zh-cn_topic_0000001396761846_p87041510113416"></a>三级资源</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.5.1.4"><p id="zh-cn_topic_0000001396761846_p41696274"><a name="zh-cn_topic_0000001396761846_p41696274"></a><a name="zh-cn_topic_0000001396761846_p41696274"></a>说明</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001396761846_row540658174"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p2406981973"><a name="zh-cn_topic_0000001396761846_p2406981973"></a><a name="zh-cn_topic_0000001396761846_p2406981973"></a>member_list</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p94065817712"><a name="zh-cn_topic_0000001396761846_p94065817712"></a><a name="zh-cn_topic_0000001396761846_p94065817712"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p143117683411"><a name="zh-cn_topic_0000001396761846_p143117683411"></a><a name="zh-cn_topic_0000001396761846_p143117683411"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p74061085713"><a name="zh-cn_topic_0000001396761846_p74061085713"></a><a name="zh-cn_topic_0000001396761846_p74061085713"></a>含义：安装或升级参数</p>
<p id="zh-cn_topic_0000001396761846_p991610111319"><a name="zh-cn_topic_0000001396761846_p991610111319"></a><a name="zh-cn_topic_0000001396761846_p991610111319"></a>类型：list</p>
<p id="p101311655591"><a name="p101311655591"></a><a name="p101311655591"></a>取值：取值范围为1~256，目前固件升级只选取列表的第一个元素</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row1940616814718"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p13406118276"><a name="zh-cn_topic_0000001396761846_p13406118276"></a><a name="zh-cn_topic_0000001396761846_p13406118276"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p20406188978"><a name="zh-cn_topic_0000001396761846_p20406188978"></a><a name="zh-cn_topic_0000001396761846_p20406188978"></a>operator</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p3432062348"><a name="zh-cn_topic_0000001396761846_p3432062348"></a><a name="zh-cn_topic_0000001396761846_p3432062348"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p1040620820710"><a name="zh-cn_topic_0000001396761846_p1040620820710"></a><a name="zh-cn_topic_0000001396761846_p1040620820710"></a>含义：操作类型</p>
<p id="zh-cn_topic_0000001396761846_p16282420103419"><a name="zh-cn_topic_0000001396761846_p16282420103419"></a><a name="zh-cn_topic_0000001396761846_p16282420103419"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p13673153193419"><a name="zh-cn_topic_0000001396761846_p13673153193419"></a><a name="zh-cn_topic_0000001396761846_p13673153193419"></a>取值：install（安装）</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row783021316813"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p183113131689"><a name="zh-cn_topic_0000001396761846_p183113131689"></a><a name="zh-cn_topic_0000001396761846_p183113131689"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p58311713889"><a name="zh-cn_topic_0000001396761846_p58311713889"></a><a name="zh-cn_topic_0000001396761846_p58311713889"></a>type</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p1743215618341"><a name="zh-cn_topic_0000001396761846_p1743215618341"></a><a name="zh-cn_topic_0000001396761846_p1743215618341"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p108311613986"><a name="zh-cn_topic_0000001396761846_p108311613986"></a><a name="zh-cn_topic_0000001396761846_p108311613986"></a>含义：安装或升级类型</p>
<p id="zh-cn_topic_0000001396761846_p63814549348"><a name="zh-cn_topic_0000001396761846_p63814549348"></a><a name="zh-cn_topic_0000001396761846_p63814549348"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p16375109123511"><a name="zh-cn_topic_0000001396761846_p16375109123511"></a><a name="zh-cn_topic_0000001396761846_p16375109123511"></a>取值：firmware</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row11840424482"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p1484010241818"><a name="zh-cn_topic_0000001396761846_p1484010241818"></a><a name="zh-cn_topic_0000001396761846_p1484010241818"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p584016249813"><a name="zh-cn_topic_0000001396761846_p584016249813"></a><a name="zh-cn_topic_0000001396761846_p584016249813"></a>install_method</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p114327673413"><a name="zh-cn_topic_0000001396761846_p114327673413"></a><a name="zh-cn_topic_0000001396761846_p114327673413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p78406241081"><a name="zh-cn_topic_0000001396761846_p78406241081"></a><a name="zh-cn_topic_0000001396761846_p78406241081"></a>含义：安装类型</p>
<p id="zh-cn_topic_0000001396761846_p118319863618"><a name="zh-cn_topic_0000001396761846_p118319863618"></a><a name="zh-cn_topic_0000001396761846_p118319863618"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p28316814369"><a name="zh-cn_topic_0000001396761846_p28316814369"></a><a name="zh-cn_topic_0000001396761846_p28316814369"></a>取值：</p>
<a name="zh-cn_topic_0000001396761846_ul2377314153615"></a><a name="zh-cn_topic_0000001396761846_ul2377314153615"></a><ul id="zh-cn_topic_0000001396761846_ul2377314153615"><li>force：强制升级</li><li>normal：平滑升级</li><li>""（空字符串）固件升级时允许传递空字符串</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row287727788"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p11884274819"><a name="zh-cn_topic_0000001396761846_p11884274819"></a><a name="zh-cn_topic_0000001396761846_p11884274819"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p58892716811"><a name="zh-cn_topic_0000001396761846_p58892716811"></a><a name="zh-cn_topic_0000001396761846_p58892716811"></a>enable_method</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p1943210614344"><a name="zh-cn_topic_0000001396761846_p1943210614344"></a><a name="zh-cn_topic_0000001396761846_p1943210614344"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p0889279813"><a name="zh-cn_topic_0000001396761846_p0889279813"></a><a name="zh-cn_topic_0000001396761846_p0889279813"></a>含义：生效方式</p>
<p id="zh-cn_topic_0000001396761846_p158632020183619"><a name="zh-cn_topic_0000001396761846_p158632020183619"></a><a name="zh-cn_topic_0000001396761846_p158632020183619"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p12863152019367"><a name="zh-cn_topic_0000001396761846_p12863152019367"></a><a name="zh-cn_topic_0000001396761846_p12863152019367"></a>取值：</p>
<a name="zh-cn_topic_0000001396761846_ul882412261367"></a><a name="zh-cn_topic_0000001396761846_ul882412261367"></a><ul id="zh-cn_topic_0000001396761846_ul882412261367"><li>now：立即升级</li><li>delay：延迟生效，延迟生效需要配合延迟升级的接口使用</li><li>""（空字符串）固件升级时允许传递空字符串</li></ul>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row31821858786"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p3182135813813"><a name="zh-cn_topic_0000001396761846_p3182135813813"></a><a name="zh-cn_topic_0000001396761846_p3182135813813"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p818218582812"><a name="zh-cn_topic_0000001396761846_p818218582812"></a><a name="zh-cn_topic_0000001396761846_p818218582812"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p1143216143413"><a name="zh-cn_topic_0000001396761846_p1143216143413"></a><a name="zh-cn_topic_0000001396761846_p1143216143413"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p418318581988"><a name="zh-cn_topic_0000001396761846_p418318581988"></a><a name="zh-cn_topic_0000001396761846_p418318581988"></a>含义：固件名称</p>
<p id="zh-cn_topic_0000001396761846_p12608137113610"><a name="zh-cn_topic_0000001396761846_p12608137113610"></a><a name="zh-cn_topic_0000001396761846_p12608137113610"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p17608337133610"><a name="zh-cn_topic_0000001396761846_p17608337133610"></a><a name="zh-cn_topic_0000001396761846_p17608337133610"></a>取值：1~256字节，仅支持“a-zA-Z0-9-_.”，不能包含“..”</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row182410334345"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p182493333419"><a name="zh-cn_topic_0000001396761846_p182493333419"></a><a name="zh-cn_topic_0000001396761846_p182493333419"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p9824143318342"><a name="zh-cn_topic_0000001396761846_p9824143318342"></a><a name="zh-cn_topic_0000001396761846_p9824143318342"></a>https_server</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p1882453314342"><a name="zh-cn_topic_0000001396761846_p1882453314342"></a><a name="zh-cn_topic_0000001396761846_p1882453314342"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p198246339347"><a name="zh-cn_topic_0000001396761846_p198246339347"></a><a name="zh-cn_topic_0000001396761846_p198246339347"></a>含义：文件服务器</p>
<p id="zh-cn_topic_0000001396761846_p15266184243615"><a name="zh-cn_topic_0000001396761846_p15266184243615"></a><a name="zh-cn_topic_0000001396761846_p15266184243615"></a>类型：dict</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row2012215619356"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p171671016409"><a name="zh-cn_topic_0000001396761846_p171671016409"></a><a name="zh-cn_topic_0000001396761846_p171671016409"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p41228623519"><a name="zh-cn_topic_0000001396761846_p41228623519"></a><a name="zh-cn_topic_0000001396761846_p41228623519"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p1640693016365"><a name="zh-cn_topic_0000001396761846_p1640693016365"></a><a name="zh-cn_topic_0000001396761846_p1640693016365"></a>image</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p140603003614"><a name="zh-cn_topic_0000001396761846_p140603003614"></a><a name="zh-cn_topic_0000001396761846_p140603003614"></a>含义：升级文件</p>
<p id="zh-cn_topic_0000001396761846_p195391938190"><a name="zh-cn_topic_0000001396761846_p195391938190"></a><a name="zh-cn_topic_0000001396761846_p195391938190"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p2539334193"><a name="zh-cn_topic_0000001396761846_p2539334193"></a><a name="zh-cn_topic_0000001396761846_p2539334193"></a>取值：不超过256字节，不能包含“..”，以一串非空白字符+空格开头，然后是"https"加一串不包含特定字符（@、\n、!、\\、|、;、&amp;、$、&lt;、&gt;、` 或空格）的字符串</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row487522114351"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p101790018406"><a name="zh-cn_topic_0000001396761846_p101790018406"></a><a name="zh-cn_topic_0000001396761846_p101790018406"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p1392152124018"><a name="zh-cn_topic_0000001396761846_p1392152124018"></a><a name="zh-cn_topic_0000001396761846_p1392152124018"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p145965512361"><a name="zh-cn_topic_0000001396761846_p145965512361"></a><a name="zh-cn_topic_0000001396761846_p145965512361"></a>user_name</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p745915559362"><a name="zh-cn_topic_0000001396761846_p745915559362"></a><a name="zh-cn_topic_0000001396761846_p745915559362"></a>含义：用户名称</p>
<p id="zh-cn_topic_0000001396761846_p176403228192"><a name="zh-cn_topic_0000001396761846_p176403228192"></a><a name="zh-cn_topic_0000001396761846_p176403228192"></a>类型：string</p>
<p id="p17591181119276"><a name="p17591181119276"></a><a name="p17591181119276"></a>取值：1~64字节，仅支持“a-zA-Z0-9-_”</p>
<p id="zh-cn_topic_0000001396761846_p3640522191919"><a name="zh-cn_topic_0000001396761846_p3640522191919"></a><a name="zh-cn_topic_0000001396761846_p3640522191919"></a>从FusionDirector下载文件时使用的账号，详细信息请参考<span id="ph95490235553"><a name="ph95490235553"></a><a name="ph95490235553"></a>《<a href="https://support.huawei.com/enterprise/zh/doc/EDOC1100316872/426cffd9?idPath=23710424|251364417|251364851|252309137|23015464" target="_blank" rel="noopener noreferrer">FusionDirector 维护指南</a>》</span></p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row26181240103612"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p5182170194015"><a name="zh-cn_topic_0000001396761846_p5182170194015"></a><a name="zh-cn_topic_0000001396761846_p5182170194015"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p1239515214405"><a name="zh-cn_topic_0000001396761846_p1239515214405"></a><a name="zh-cn_topic_0000001396761846_p1239515214405"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p8459115543612"><a name="zh-cn_topic_0000001396761846_p8459115543612"></a><a name="zh-cn_topic_0000001396761846_p8459115543612"></a>password</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p2459115512365"><a name="zh-cn_topic_0000001396761846_p2459115512365"></a><a name="zh-cn_topic_0000001396761846_p2459115512365"></a>含义：密码</p>
<p id="zh-cn_topic_0000001396761846_p17319123112191"><a name="zh-cn_topic_0000001396761846_p17319123112191"></a><a name="zh-cn_topic_0000001396761846_p17319123112191"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p23191131141910"><a name="zh-cn_topic_0000001396761846_p23191131141910"></a><a name="zh-cn_topic_0000001396761846_p23191131141910"></a>取值：长度范围为8~64字节，复杂度由上层保证</p>
<p id="p23191131141910"><a name="p23191131141910"></a><a name="p23191131141910"></a>FusionDirector下载文件使用的账号的密码，详细信息请参考《<a href="https://support.huawei.com/enterprise/zh/doc/EDOC1100316872/426cffd9?idPath=23710424|251364417|251364851|252309137|23015464" target="_blank" rel="noopener noreferrer">FusionDirector 维护指南</a>》</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row5448121615011"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p104489166503"><a name="zh-cn_topic_0000001396761846_p104489166503"></a><a name="zh-cn_topic_0000001396761846_p104489166503"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p19448716195013"><a name="zh-cn_topic_0000001396761846_p19448716195013"></a><a name="zh-cn_topic_0000001396761846_p19448716195013"></a>check_type</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p1593071210510"><a name="zh-cn_topic_0000001396761846_p1593071210510"></a><a name="zh-cn_topic_0000001396761846_p1593071210510"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p1993081214519"><a name="zh-cn_topic_0000001396761846_p1993081214519"></a><a name="zh-cn_topic_0000001396761846_p1993081214519"></a>含义：校验码类型</p>
<p id="zh-cn_topic_0000001396761846_p159301212135117"><a name="zh-cn_topic_0000001396761846_p159301212135117"></a><a name="zh-cn_topic_0000001396761846_p159301212135117"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p193081218518"><a name="zh-cn_topic_0000001396761846_p193081218518"></a><a name="zh-cn_topic_0000001396761846_p193081218518"></a>取值：sha256（当前仅支持sha256）</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761846_row87131830135219"><td class="cellrowborder" valign="top" width="19.56%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001396761846_p16713163015213"><a name="zh-cn_topic_0000001396761846_p16713163015213"></a><a name="zh-cn_topic_0000001396761846_p16713163015213"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="20.44%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001396761846_p271393085214"><a name="zh-cn_topic_0000001396761846_p271393085214"></a><a name="zh-cn_topic_0000001396761846_p271393085214"></a>check_code</p>
</td>
<td class="cellrowborder" valign="top" width="20%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001396761846_p17713113035214"><a name="zh-cn_topic_0000001396761846_p17713113035214"></a><a name="zh-cn_topic_0000001396761846_p17713113035214"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001396761846_p1270882695311"><a name="zh-cn_topic_0000001396761846_p1270882695311"></a><a name="zh-cn_topic_0000001396761846_p1270882695311"></a>含义：固件校验码</p>
<p id="zh-cn_topic_0000001396761846_p248911783214"><a name="zh-cn_topic_0000001396761846_p248911783214"></a><a name="zh-cn_topic_0000001396761846_p248911783214"></a>类型：string</p>
<p id="zh-cn_topic_0000001396761846_p117452249519"><a name="zh-cn_topic_0000001396761846_p117452249519"></a><a name="zh-cn_topic_0000001396761846_p117452249519"></a>取值：空字符串或者64字节（Sha256sum生成的校验码，格式符合正则模式'^[0-9a-f]{64}$'，如：7e642fa557533508b589d7fcaef922b443ff4b5df16b2ad91581bd2b3d7c1fc3）</p>
</td>
</tr>
</tbody>
</table>

**返回结果**

```json
{
    "header":{
        "msg_id":"e1d7ec35-ceac-4648-a581-056f5690d8f9",
        "parent_msg_id": "",
        "timestamp":1652279813323,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/upgrade_progress"
    },
    "content":"{"members": [{"operator": "install", "name": "firmware", "version": "NA", "percentage": "100", "result": "success", "reason": ""}]}"
}
```

### 固件生效<a name="ZH-CN_TOPIC_0000001628849877"></a>

当前均通过复位系统生效固件。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/firmware_effective"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "active": "inband"
}
```

元素定义如下：

|一级资源|描述|类型|取值范围|
|--|--|--|--|
|active|生效方式|string|当前仅支持inband。具体需要通过可升级固件列表确认。对于生效系统是否成功，可以通过系统上报的静态信息里的可升级固件列表确认。|

**返回结果**

```json
{
    "header":{
        "msg_id":"9791bbb6-1d96-4e82-82cf-8e4a3a92ad5d",
        "parent_msg_id": "",
        "timestamp":1652280713614,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/restart_result"
    },
    "content":"{"restartable": "true", "reason": "effective firmware now"}"
}
```

### 信息收集<a name="ZH-CN_TOPIC_0000001628490461"></a>

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/info_collect"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "type": "all",
    "module": "all",
    "https_server": {
        "url": "POST https://FDAddr/log.tar.gz",
        "user_name": "userName",
        "password": "password"
    }
}
```

<a name="zh-cn_topic_0000001447161509_table8935126"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001447161509_row8178206"><th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.1"><p id="zh-cn_topic_0000001447161509_p58454987"><a name="zh-cn_topic_0000001447161509_p58454987"></a><a name="zh-cn_topic_0000001447161509_p58454987"></a>一级资源</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.2"><p id="zh-cn_topic_0000001447161509_p37233496"><a name="zh-cn_topic_0000001447161509_p37233496"></a><a name="zh-cn_topic_0000001447161509_p37233496"></a>二级资源</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.3"><p id="zh-cn_topic_0000001447161509_p63123181"><a name="zh-cn_topic_0000001447161509_p63123181"></a><a name="zh-cn_topic_0000001447161509_p63123181"></a>描述</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.6.1.4"><p id="zh-cn_topic_0000001447161509_p12704051"><a name="zh-cn_topic_0000001447161509_p12704051"></a><a name="zh-cn_topic_0000001447161509_p12704051"></a>类型</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.6.1.5"><p id="zh-cn_topic_0000001447161509_p22395217"><a name="zh-cn_topic_0000001447161509_p22395217"></a><a name="zh-cn_topic_0000001447161509_p22395217"></a>取值范围</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001447161509_row5991562"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001447161509_p15554548"><a name="zh-cn_topic_0000001447161509_p15554548"></a><a name="zh-cn_topic_0000001447161509_p15554548"></a>type</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001447161509_p51958903"><a name="zh-cn_topic_0000001447161509_p51958903"></a><a name="zh-cn_topic_0000001447161509_p51958903"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001447161509_p47921594"><a name="zh-cn_topic_0000001447161509_p47921594"></a><a name="zh-cn_topic_0000001447161509_p47921594"></a>日志类型</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001447161509_p56443877"><a name="zh-cn_topic_0000001447161509_p56443877"></a><a name="zh-cn_topic_0000001447161509_p56443877"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001447161509_p8551329"><a name="zh-cn_topic_0000001447161509_p8551329"></a><a name="zh-cn_topic_0000001447161509_p8551329"></a>当前只支持all类型，包含操作日志和运行日志</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161509_row9853101"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001447161509_p59903684"><a name="zh-cn_topic_0000001447161509_p59903684"></a><a name="zh-cn_topic_0000001447161509_p59903684"></a>module</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001447161509_p20360256"><a name="zh-cn_topic_0000001447161509_p20360256"></a><a name="zh-cn_topic_0000001447161509_p20360256"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001447161509_p38568073"><a name="zh-cn_topic_0000001447161509_p38568073"></a><a name="zh-cn_topic_0000001447161509_p38568073"></a>模块名称</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001447161509_p37006214"><a name="zh-cn_topic_0000001447161509_p37006214"></a><a name="zh-cn_topic_0000001447161509_p37006214"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001447161509_p8525123101816"><a name="zh-cn_topic_0000001447161509_p8525123101816"></a><a name="zh-cn_topic_0000001447161509_p8525123101816"></a>当前只支持all类型，包含NPU、操作系统驱动、<span id="ph10430464338"><a name="ph10430464338"></a><a name="ph10430464338"></a>OM</span>日志</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161509_row1964735418125"><td class="cellrowborder" rowspan="3" valign="top" width="15%" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001447161509_p19360517181317"><a name="zh-cn_topic_0000001447161509_p19360517181317"></a><a name="zh-cn_topic_0000001447161509_p19360517181317"></a>https_server</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001447161509_p06474549122"><a name="zh-cn_topic_0000001447161509_p06474549122"></a><a name="zh-cn_topic_0000001447161509_p06474549122"></a>url</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001447161509_p126474540129"><a name="zh-cn_topic_0000001447161509_p126474540129"></a><a name="zh-cn_topic_0000001447161509_p126474540129"></a>上传日志url连接</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001447161509_p964785431219"><a name="zh-cn_topic_0000001447161509_p964785431219"></a><a name="zh-cn_topic_0000001447161509_p964785431219"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.6.1.5 "><p id="zh-cn_topic_0000001447161509_p26471545127"><a name="zh-cn_topic_0000001447161509_p26471545127"></a><a name="zh-cn_topic_0000001447161509_p26471545127"></a>最长2048字节，^POST https[^@\n!\\|;&amp;$&lt;&gt;` ]+$，并且不包含..</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161509_row12281558181212"><td class="cellrowborder" valign="top" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001447161509_p182811358171216"><a name="zh-cn_topic_0000001447161509_p182811358171216"></a><a name="zh-cn_topic_0000001447161509_p182811358171216"></a>user_name</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001447161509_p1281165817126"><a name="zh-cn_topic_0000001447161509_p1281165817126"></a><a name="zh-cn_topic_0000001447161509_p1281165817126"></a>用户名称</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001447161509_p42816589121"><a name="zh-cn_topic_0000001447161509_p42816589121"></a><a name="zh-cn_topic_0000001447161509_p42816589121"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001447161509_p884916451192"><a name="zh-cn_topic_0000001447161509_p884916451192"></a><a name="zh-cn_topic_0000001447161509_p884916451192"></a>^[a-zA-Z][a-zA-Z0-9-_]{1,64}[a-zA-Z0-9]$</p>
<p id="p884916451192"><a name="p884916451192"></a><a name="p884916451192"></a>上传日志到FusionDirector的账号，详细信息请参考《<a href="https://support.huawei.com/enterprise/zh/doc/EDOC1100316872/426cffd9?idPath=23710424|251364417|251364851|252309137|23015464" target="_blank" rel="noopener noreferrer">FusionDirector 维护指南</a>》</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001447161509_row12853719133"><td class="cellrowborder" valign="top" headers="mcps1.1.6.1.1 "><p id="zh-cn_topic_0000001447161509_p48531110137"><a name="zh-cn_topic_0000001447161509_p48531110137"></a><a name="zh-cn_topic_0000001447161509_p48531110137"></a>password</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.2 "><p id="zh-cn_topic_0000001447161509_p118531313132"><a name="zh-cn_topic_0000001447161509_p118531313132"></a><a name="zh-cn_topic_0000001447161509_p118531313132"></a>密码</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.3 "><p id="zh-cn_topic_0000001447161509_p88531211137"><a name="zh-cn_topic_0000001447161509_p88531211137"></a><a name="zh-cn_topic_0000001447161509_p88531211137"></a>string</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.6.1.4 "><p id="zh-cn_topic_0000001447161509_p38531713132"><a name="zh-cn_topic_0000001447161509_p38531713132"></a><a name="zh-cn_topic_0000001447161509_p38531713132"></a>长度范围为8~20字节，复杂度由上层保证</p>
<p id="p1153891916266"><a name="p1153891916266"></a><a name="p1153891916266"></a>上传日志到FusionDirector的账号，详细信息请参考《<a href="https://support.huawei.com/enterprise/zh/doc/EDOC1100316872/426cffd9?idPath=23710424|251364417|251364851|252309137|23015464" target="_blank" rel="noopener noreferrer">FusionDirector 维护指南</a>》</p>
</td>
</tr>
</tbody>
</table>

**返回结果**

```json
{
    "header":{
        "msg_id":"10b44ba5-5df9-4279-9281-c0ac90a6ec09",
        "parent_msg_id": "",
        "timestamp":1652187944413,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/info_collect_process"
    },
    "content":"{"type": "all", "module": "all", "percentage": "100%", "result": "success", "reason": ""}"
}
```

### 重上报硬件告警<a name="ZH-CN_TOPIC_0000001578489812"></a>

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/rearm"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "resource": "alarm"
}
```

重上报的告警信息，应全量上报。

|一级资源|二级资源|描述|类型|取值范围|
|--|--|--|--|--|
|resource|-|资源类型|string|当前只支持“alarm”|

### 配置主机名<a name="ZH-CN_TOPIC_0000001577810548"></a>

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/config_hostname"
    },
    "content": {
    }
}
```

**参数说明**

```json
{
    "hostname": "g000121002000220010-ipc01"
}
```

|一级资源|描述|类型|取值范围|
|--|--|--|--|
|hostname|主机名|string|只支持配置一个名字，新配置直接覆盖旧配置，1~63字节，不允许为空。主机名配置规范如下：<li>主机名只允许包含ASCII字符里的数字0-9，字母a-z或A-Z，连字符“-”。其他字符都不允许，例如不允许出现其他标点符号、空格、下划线和中文字符。</li><li>主机名的开头和结尾不允许是连字符。</li><li>主机名建议不要以数字开头，也不要使用只包含可解释为16进制字符的字符串，例如"beef"。</li><li>建议不要使用计算机领域的特殊词汇，例如up。</li>配置主机名后，通过通用接口返回结果，主动触发一次静态信息上报。|

**返回结果**

```json
{
    "header":{
        "msg_id":"5d7f1e5f-2e8d-41dc-b620-5318eb6a7962",
        "parent_msg_id": "",
        "timestamp":1652261771062,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/config_result"
    },
    "content":"{"topic": "config_hostname", "percentage": "100%", "result": "success", "reason": ""}"
}
```

### 重置设备账号密码<a name="ZH-CN_TOPIC_0000001628729957"></a>

**消息示例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/passthrough/account_modify"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:  
```json
{
     "account": account,
     "new_password": new_password
}
```

**表 1** 

|一级资源|描述|类型|取值范围|
|--|--|--|--|
|account|用户名|string|取值范围：admin执行该功能会重置设备Web登录账号密码，详细参考[《FusionDirector 操作指南》](https://support.huawei.com/enterprise/zh/doc/EDOC1100317179/426cffd9?idPath=23710424\|251364417\|251364851\|252309137\|23015464)。|
|new_password|新修改密码|string|长度为8~20的字符串。<li>如果其他接口启用了密码复杂度检查功能，则设置和修改的密码必须遵循密码复杂度的规则。</li><li>如果其他接口未启用密码复杂度检查功能，则设置和修改的密码可以为任意字符。</li>|

```json
返回结果
{
    "header":{
        "msg_id":"8a44d9c0-05c9-4325-b530-fa5c4a5145f2",
        "parent_msg_id": "",
        "timestamp":1652269263083,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/config_result"
    },
    "content":"{"topic": "passthrough/account_modify", "percentage": "100%", "result": "success", "reason": ""}"
}
```

### 配置网管信息<a name="ZH-CN_TOPIC_0000001628490477"></a>

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/netmanager"
    },
    "content": {
    }
}
```

**参数说明**

content:

```json
{
    "address": "127.0.0.1",
    "account": "root",
    "password": "123456",
    "test": true
}
```

<a name="zh-cn_topic_0000001397081518_table37656977"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001397081518_row11892013"><th class="cellrowborder" valign="top" width="15%" id="mcps1.1.5.1.1"><p id="zh-cn_topic_0000001397081518_p23728993"><a name="zh-cn_topic_0000001397081518_p23728993"></a><a name="zh-cn_topic_0000001397081518_p23728993"></a>一级资源</p>
</th>
<th class="cellrowborder" valign="top" width="30%" id="mcps1.1.5.1.2"><p id="zh-cn_topic_0000001397081518_p43000310"><a name="zh-cn_topic_0000001397081518_p43000310"></a><a name="zh-cn_topic_0000001397081518_p43000310"></a>描述</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.5.1.3"><p id="zh-cn_topic_0000001397081518_p60473108"><a name="zh-cn_topic_0000001397081518_p60473108"></a><a name="zh-cn_topic_0000001397081518_p60473108"></a>类型</p>
</th>
<th class="cellrowborder" valign="top" width="40%" id="mcps1.1.5.1.4"><p id="zh-cn_topic_0000001397081518_p66483554"><a name="zh-cn_topic_0000001397081518_p66483554"></a><a name="zh-cn_topic_0000001397081518_p66483554"></a>取值范围</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001397081518_row61481074"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081518_p13911116"><a name="zh-cn_topic_0000001397081518_p13911116"></a><a name="zh-cn_topic_0000001397081518_p13911116"></a>address</p>
</td>
<td class="cellrowborder" valign="top" width="30%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081518_p53058602"><a name="zh-cn_topic_0000001397081518_p53058602"></a><a name="zh-cn_topic_0000001397081518_p53058602"></a>网管IP地址</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081518_p2779509"><a name="zh-cn_topic_0000001397081518_p2779509"></a><a name="zh-cn_topic_0000001397081518_p2779509"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001397081518_p23813652"><a name="zh-cn_topic_0000001397081518_p23813652"></a><a name="zh-cn_topic_0000001397081518_p23813652"></a>目前仅支持IPv4地址或为空，如果address为空，表示不需要修改。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081518_row12996279"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081518_p46065648"><a name="zh-cn_topic_0000001397081518_p46065648"></a><a name="zh-cn_topic_0000001397081518_p46065648"></a>account</p>
</td>
<td class="cellrowborder" valign="top" width="30%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081518_p40329969"><a name="zh-cn_topic_0000001397081518_p40329969"></a><a name="zh-cn_topic_0000001397081518_p40329969"></a>网管账号</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081518_p45502051"><a name="zh-cn_topic_0000001397081518_p45502051"></a><a name="zh-cn_topic_0000001397081518_p45502051"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001397081518_p61787501"><a name="zh-cn_topic_0000001397081518_p61787501"></a><a name="zh-cn_topic_0000001397081518_p61787501"></a>1~256个字符，当需要修改address或address为空时，需要下发网管账号和网管密码。表示FusionDirector对接账号，默认账号为EdgeAccount。采用默认EdgeAccount对接成功之后，对接账号会自动采用FusionDirector下发的新的一机一密重新对接。一机一密账户和密码由FusionDirector的业务微服务自生成，详细参考<span id="ph95490235553"><a name="ph95490235553"></a><a name="ph95490235553"></a>《<a href="https://support.huawei.com/enterprise/zh/doc/EDOC1100316872/426cffd9?idPath=23710424%7C251364417%7C251364851%7C252309137%7C23015464" target="_blank" rel="noopener noreferrer">FusionDirector 维护指南</a>》</span>。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081518_row19216603"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081518_p13041021"><a name="zh-cn_topic_0000001397081518_p13041021"></a><a name="zh-cn_topic_0000001397081518_p13041021"></a>password</p>
</td>
<td class="cellrowborder" valign="top" width="30%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081518_p49689804"><a name="zh-cn_topic_0000001397081518_p49689804"></a><a name="zh-cn_topic_0000001397081518_p49689804"></a>网管密码</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081518_p65451183"><a name="zh-cn_topic_0000001397081518_p65451183"></a><a name="zh-cn_topic_0000001397081518_p65451183"></a>string</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001397081518_p67054479"><a name="zh-cn_topic_0000001397081518_p67054479"></a><a name="zh-cn_topic_0000001397081518_p67054479"></a>表示FusionDirector对接密码。取值为8~32个字符。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001397081518_row66619401"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001397081518_p27462382"><a name="zh-cn_topic_0000001397081518_p27462382"></a><a name="zh-cn_topic_0000001397081518_p27462382"></a>test</p>
</td>
<td class="cellrowborder" valign="top" width="30%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001397081518_p1934938194313"><a name="zh-cn_topic_0000001397081518_p1934938194313"></a><a name="zh-cn_topic_0000001397081518_p1934938194313"></a>是否在网管生效前测试网管是否可用</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001397081518_p60501746"><a name="zh-cn_topic_0000001397081518_p60501746"></a><a name="zh-cn_topic_0000001397081518_p60501746"></a>bool</p>
</td>
<td class="cellrowborder" valign="top" width="40%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001397081518_p16558187439"><a name="zh-cn_topic_0000001397081518_p16558187439"></a><a name="zh-cn_topic_0000001397081518_p16558187439"></a>取值为：</p>
<a name="zh-cn_topic_0000001397081518_ul64091522104318"></a><a name="zh-cn_topic_0000001397081518_ul64091522104318"></a><ul id="zh-cn_topic_0000001397081518_ul64091522104318"><li>true：需要测试</li><li>false：不需要测试</li></ul>
</td>
</tr>
</tbody>
</table>

**返回结果**

```json
{
    "header":{
        "msg_id":"5a955ef7-3bbd-4603-ad7e-1e9e5dd09862",
        "parent_msg_id": "",
        "timestamp":1652261182356,
        "sync": false
    },
    "route":{
        "source":"hardware",
        "group":"hub",
        "operation":"update",
        "resource":"websocket/config_result"
    },
    "content":"{"topic": "netmanager", "percentage": "100%", "result": "success", "reason": ""}"
}
```

### 配置容器属性<a name="ZH-CN_TOPIC_0000001628172324"></a>

配置容器属性，FusionDirector部署容器之前会通过此消息配置容器相关属性。

**消息示例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": true
    },
    "route": {
        "source": "controller",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/container_info"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "operation": "update",
    "source": "all",
    "pod_name": "dockerapplication-bdf3242b-aec1-4100-af91-afa2b8fde88a",
    "pod_uid": "bdf3242b-aec1-4100-af91-afa2b8fde88a",
    "uuid": "447c3c43-4bbe-4dd1-b493-d6dc442b4c72",
    "container": [{
        "modelfile": [{
            "name": "XXX",
            "version": "1.0",
            "active_type": "hot_update| cold_update"  
        }]
    }]
}
```

<a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_table1060881312819"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row4608913192818"><th class="cellrowborder" valign="top" width="15%" id="mcps1.1.5.1.1"><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p23052717"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p23052717"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p23052717"></a>一级资源</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.5.1.2"><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p55330787"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p55330787"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p55330787"></a>二级资源</p>
</th>
<th class="cellrowborder" valign="top" width="15%" id="mcps1.1.5.1.3"><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p52608733"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p52608733"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p52608733"></a>三级资源</p>
</th>
<th class="cellrowborder" valign="top" width="55.00000000000001%" id="mcps1.1.5.1.4"><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p33448963"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p33448963"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p33448963"></a>说明</p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row7609413172812"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p57480694"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p57480694"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p57480694"></a>operation</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p25424598"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p25424598"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p25424598"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p46126530"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p46126530"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p46126530"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p45261475"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p45261475"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p45261475"></a>含义：操作类型</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1328319013240"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1328319013240"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1328319013240"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1368711416247"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1368711416247"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1368711416247"></a>取值为update，表示模型文件更新操作</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row126091131285"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8520131155411"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8520131155411"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8520131155411"></a>source</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p208061746125415"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p208061746125415"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p208061746125415"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p168061646105416"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p168061646105416"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p168061646105416"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p31379588"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p31379588"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p31379588"></a>含义：目标类型</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p3728220112611"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p3728220112611"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p3728220112611"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p27281320152617"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p27281320152617"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p27281320152617"></a>取值为all，表示操作下发消息中的所有模型文件</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row3609913152814"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p165205135413"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p165205135413"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p165205135413"></a>pod_name</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p68711197564"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p68711197564"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p68711197564"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p15732240"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p15732240"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p15732240"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p66351956"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p66351956"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p66351956"></a>含义：Pod的名称</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8656135192616"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8656135192616"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8656135192616"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p146561235112617"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p146561235112617"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p146561235112617"></a>取值：格式符合正则模式^[0-9a-z][0-9a-z-]{1,128}$</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row1760916135287"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p145209115548"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p145209115548"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p145209115548"></a>pod_uid</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p36359009"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p36359009"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p36359009"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p59398588"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p59398588"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p59398588"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p46556328"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p46556328"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p46556328"></a>含义：k8s pod的ID</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2598844142610"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2598844142610"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2598844142610"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1659884412266"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1659884412266"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1659884412266"></a>取值：格式符合正则模式 ^[0-9a-f]{8}(-[0-9a-f]{4}){3}-[0-9a-f]{12}$</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row176102013112813"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1976915053017"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1976915053017"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1976915053017"></a>uuid</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p53482885"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p53482885"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p53482885"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p37146447"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p37146447"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p37146447"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p56072221"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p56072221"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p56072221"></a>含义：服务ID</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p132154511266"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p132154511266"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p132154511266"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p121392048124617"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p121392048124617"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p121392048124617"></a>取值：格式符合正则模式 ^[0-9a-f]{8}(-[0-9a-f]{4}){3}-[0-9a-f]{12}$</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row10484432913"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8416446297"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8416446297"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p8416446297"></a>container</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p10267424103218"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p10267424103218"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p10267424103218"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2681101933213"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2681101933213"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2681101933213"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1112124813115"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1112124813115"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1112124813115"></a>含义：容器信息</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p6836142213275"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p6836142213275"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p6836142213275"></a>类型：list</p>
<p id="zh-cn_topic_0000001501722602_p349945642718"><a name="zh-cn_topic_0000001501722602_p349945642718"></a><a name="zh-cn_topic_0000001501722602_p349945642718"></a>取值：1~10</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row172271954132910"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1752933083211"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1752933083211"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1752933083211"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p52272541292"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p52272541292"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p52272541292"></a>modelfile</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1468210191327"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1468210191327"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1468210191327"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p910374843117"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p910374843117"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p910374843117"></a>含义：模型文件信息</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p5897165516273"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p5897165516273"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p5897165516273"></a>类型：list</p>
<p id="zh-cn_topic_0000001501722602_p37081913162817"><a name="zh-cn_topic_0000001501722602_p37081913162817"></a><a name="zh-cn_topic_0000001501722602_p37081913162817"></a>取值：0~256</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row6981640203011"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p852943015329"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p852943015329"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p852943015329"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p69573347328"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p69573347328"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p69573347328"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p129913406301"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p129913406301"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p129913406301"></a>name</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1410074812319"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1410074812319"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1410074812319"></a>含义：模型文件名称</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1581412982812"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1581412982812"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1581412982812"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p98141090282"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p98141090282"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p98141090282"></a>取值：1~256字节，格式符合正则模式“^[a-zA-Z0-9_.-]{1,256}$”，且不包含两个“.”。仅支持.om、.tar.gz、.zip三种类型文件。</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row141871842103016"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p19529113083216"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p19529113083216"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p19529113083216"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p17957103415329"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p17957103415329"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p17957103415329"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2018764293018"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2018764293018"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p2018764293018"></a>version</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p99815489317"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p99815489317"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p99815489317"></a>含义：模型文件版本</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p17894151322816"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p17894151322816"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p17894151322816"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p198941213172818"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p198941213172818"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p198941213172818"></a>取值：格式符合正则模式^[a-z0-9]([a-z0-9._-]{0,62}[a-z0-9]){0,1}$</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_row935125717302"><td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.1 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p125291830193215"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p125291830193215"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p125291830193215"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.2 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p3957134113216"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p3957134113216"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p3957134113216"></a>-</p>
</td>
<td class="cellrowborder" valign="top" width="15%" headers="mcps1.1.5.1.3 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1335657163014"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1335657163014"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p1335657163014"></a>active_type</p>
</td>
<td class="cellrowborder" valign="top" width="55.00000000000001%" headers="mcps1.1.5.1.4 "><p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p197584818319"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p197584818319"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p197584818319"></a>含义：模型文件生效方式</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p10576616202817"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p10576616202817"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p10576616202817"></a>类型：string</p>
<p id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p357614160287"><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p357614160287"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_p357614160287"></a>取值：</p>
<a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_ul1463622110287"></a><a name="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_ul1463622110287"></a><ul id="zh-cn_topic_0000001501722602_zh-cn_topic_0000001438819989_ul1463622110287"><li>hot_update：热生效，不需要重启容器</li><li>cold_update，冷生效，需要重启容器</li></ul>
</td>
</tr>
</tbody>
</table>

**边侧收到消息后返回响应**

```json
{
    "header":{
    "msg_id":"ea3116aa-fd5c-446d-8c5a-2793dd09402e",
    "parent_msg_id":"706cd361-5b54-4d32-8771-550633378db4",
    "timestamp":1652182722100,
    "sync": true,
    "resourceversion": "1.0"
    },
    "route":{
        "source":"controller",
        "group":"hardware",
        "operation":"REPORT",
        "resource":"websocket/container_info"
    },
    "content":"OK"
}
```

**此外，边侧还会向云侧发送[返回配置结果](#返回配置结果)消息，上报进度**

```json
{
    "header": {
        "msg_id": "bf93c5ee-065c-4da6-b6fa-65073d29fc3f",
        "parent_msg_id": "",
        "timestamp": 1683623267,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "resource": "websocket/config_result",
        "operation": "update"
    },
    "content": "{\"topic\": \"container_info\", \"percentage\": \"100%\", \"result\": \"success\", \"reason\": \"\"}"
}
```

### 查询设备上证书的信息<a name="ZH-CN_TOPIC_0000001577810504"></a>

查询设备上的证书信息，包括对接FusionDirector的根证书、吊销列表的导入情况。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": true
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "query",
        "resource": "websocket/cert_query"
    },
    "content": ""
}
```

**参数说明<a name="zh-cn_topic_0000001447121505_section127701821325"></a>**

content内容为空，不需要增加参数，仅用于查询证书信息。

返回结果

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "hardware",
        "group": "hub",
        "operation": "update",
        "resource": "websocket/cert_info"
    },
    "content": {
          "root_certificate":{
              "cert_is_full": true/false, // 证书数量是否已满，false表示未满，true表示已满
              "cert_lists":[{
                  "cert_type":"FDRootCert",
                  "cert_name":"",
                  "issuer":"CN=xxx.ca.com, OU=IT, O=xxx, L=ShenZhen, S=GuangDong, C=CN",
                  "subject":"CN=xxx.ca.com, OU=IT, O=xxx, L=ShenZhen, S=GuangDong, C=CN",
                  "valid_not_before":"Jan 07 2017 GMT",
                  "valid_not_after":"Jan 05 2027 GMT",
                  "serial_number":"ff ff ff ff ff ff ff ff",
                  "is_import_crl":false,
                  "signature_algorithm":"sha256WithRSAEncryption",
                  "fingerprint":"ffffffffffffffffffffffffff",
                  "key_usage":"Signing, CRL Sign",
                  "public_key_length_bits":"2048"
              }]
          }
    }
}
```

### 导入根证书<a name="ZH-CN_TOPIC_0000001578449900"></a>

导入根证书。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/cert_update"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "cert_name":"ca_cert_name",
    "cert_type":"FDRootCert",
    "type":"text",
    "content":""
}
```

|一级资源名称|二级资源名称|三级资源名称|说明|
|--|--|--|--|
|cert_name|-|-|含义：证书名称<br>类型：string<br>取值：，长度4~64，只能包含大小写字母、数字、下划线、点，且不能包含..|
|cert_type|-|-|含义：指定证书类型<br>类型：string<br>取值：FDRootCert|
|type|-|-|含义：导入证书的方式<br>类型：string<br>取值：text（表明value值是证书内容）|
|content|-|-|含义：内容<br>类型：string<br>取值：证书内容的Base64编码，最长20480字节|

**返回结果**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/config_result"
    },
    "content": {
        "topic": "cert_update",
        "percentage": "100%",
        "result": "success",
        "reason": "import fd cert success"
    }
}
```

### 导入证书吊销列表<a name="ZH-CN_TOPIC_0000001628490517"></a>

导入证书吊销列表。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/crl_update"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "cert_type":"FDRootCert",
    "type":"text",
    "content":""
}
```

|一级资源名称|二级资源名称|三级资源名称|说明|
|--|--|--|--|
|cert_type|-|-|含义：指定吊销列表类型<br>类型：string<br>取值：FDRootCert|
|type|-|-|含义：导入吊销列表的方式<br>类型：string<br>取值：text（表明value值是吊销列表内容）|
|content|-|-|含义：内容<br>类型：string<br>取值：吊销列表内容的Base64编码，最长8192字节|

**返回结果**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "update",
        "resource": "websocket/config_result"
    },
    "content": {
        "topic": "crl_update",
        "percentage": "100%",
        "result": "success",
        "reason": "import fd crl success"
    }
}
```

### 删除根证书<a name="ZH-CN_TOPIC_0000001577810520"></a>

删除根证书。

**消息实例**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "delete",
        "resource": "websocket/cert_delete"
    },
    "content": {
    }
}
```

**参数说明**

content消息内容描述如下:

```json
{
    "cert_type":"FDRootCert",
    "cert_name":"ca_cert_name"
}
```

元素定义如下：

|**一级资源**|**二级资源**|**三级资源**|**说明**|
|--|--|--|--|
|cert_type|-|-|含义：指定证书类型<br>类型：string<br>取值：FDRootCert|
|cert_name|-|-|含义：证书名称<br>类型：string<br>取值：长度4~64，只能包含大小写字母、数字、下划线、点，且不能包含..|

**返回结果**

```json
{
    "header": {
        "msg_id": "741aec66-99b2-4d97-aaf2-63d27f53bbc5",
        "parent_msg_id": "",
        "timestamp": 1550107087319,
        "sync": false
    },
    "route": {
        "source": "EdgeManager",
        "group": "hardware",
        "operation": "delete",
        "resource": "websocket/config_result"
    },
    "content": {
        "topic": "cert_delete",
        "percentage": "100%",
        "result": "success",
        "reason": "delete cert success"
    }
}
```

## 附录<a name="ZH-CN_TOPIC_0000001628729929"></a>

### 错误码说明<a id="错误码说明"></a>

<a name="zh-cn_topic_0000001396761802_table12092567"></a>
<table><thead align="left"><tr id="zh-cn_topic_0000001396761802_row23141319"><th class="cellrowborder" valign="top" width="25%" id="mcps1.1.4.1.1"><p id="zh-cn_topic_0000001396761802_p62507566"><a name="zh-cn_topic_0000001396761802_p62507566"></a><a name="zh-cn_topic_0000001396761802_p62507566"></a><strong id="zh-cn_topic_0000001396761802_b25697189"><a name="zh-cn_topic_0000001396761802_b25697189"></a><a name="zh-cn_topic_0000001396761802_b25697189"></a>错误码分类</strong></p>
</th>
<th class="cellrowborder" valign="top" width="25%" id="mcps1.1.4.1.2"><p id="zh-cn_topic_0000001396761802_p1097574"><a name="zh-cn_topic_0000001396761802_p1097574"></a><a name="zh-cn_topic_0000001396761802_p1097574"></a><strong id="zh-cn_topic_0000001396761802_b9878170"><a name="zh-cn_topic_0000001396761802_b9878170"></a><a name="zh-cn_topic_0000001396761802_b9878170"></a>错误码</strong></p>
</th>
<th class="cellrowborder" valign="top" width="50%" id="mcps1.1.4.1.3"><p id="zh-cn_topic_0000001396761802_p61934319"><a name="zh-cn_topic_0000001396761802_p61934319"></a><a name="zh-cn_topic_0000001396761802_p61934319"></a><strong id="zh-cn_topic_0000001396761802_b20537961"><a name="zh-cn_topic_0000001396761802_b20537961"></a><a name="zh-cn_topic_0000001396761802_b20537961"></a>错误原因</strong></p>
</th>
</tr>
</thead>
<tbody><tr id="zh-cn_topic_0000001396761802_row52962172"><td class="cellrowborder" rowspan="2" valign="top" width="25%" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p62077519"><a name="zh-cn_topic_0000001396761802_p62077519"></a><a name="zh-cn_topic_0000001396761802_p62077519"></a>通用错误</p>
</td>
<td class="cellrowborder" valign="top" width="25%" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p62223114"><a name="zh-cn_topic_0000001396761802_p62223114"></a><a name="zh-cn_topic_0000001396761802_p62223114"></a>取值范围：100~150</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.1.4.1.3 "><p id="zh-cn_topic_0000001396761802_p6907478"><a name="zh-cn_topic_0000001396761802_p6907478"></a><a name="zh-cn_topic_0000001396761802_p6907478"></a>-</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row62167306"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p59128765"><a name="zh-cn_topic_0000001396761802_p59128765"></a><a name="zh-cn_topic_0000001396761802_p59128765"></a>0100</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p24700694"><a name="zh-cn_topic_0000001396761802_p24700694"></a><a name="zh-cn_topic_0000001396761802_p24700694"></a>消息格式错误</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row60352534"><td class="cellrowborder" rowspan="15" valign="top" width="25%" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p56717061"><a name="zh-cn_topic_0000001396761802_p56717061"></a><a name="zh-cn_topic_0000001396761802_p56717061"></a>系统升级错误</p>
</td>
<td class="cellrowborder" valign="top" width="25%" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p30679217"><a name="zh-cn_topic_0000001396761802_p30679217"></a><a name="zh-cn_topic_0000001396761802_p30679217"></a>取值范围：150~180</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.1.4.1.3 "><p id="zh-cn_topic_0000001396761802_p1988609"><a name="zh-cn_topic_0000001396761802_p1988609"></a><a name="zh-cn_topic_0000001396761802_p1988609"></a>-</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row17897482"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p51979046"><a name="zh-cn_topic_0000001396761802_p51979046"></a><a name="zh-cn_topic_0000001396761802_p51979046"></a>159</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p49553217"><a name="zh-cn_topic_0000001396761802_p49553217"></a><a name="zh-cn_topic_0000001396761802_p49553217"></a>系统升级文件不存在</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row43325774"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p54369312"><a name="zh-cn_topic_0000001396761802_p54369312"></a><a name="zh-cn_topic_0000001396761802_p54369312"></a>160</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p41838160"><a name="zh-cn_topic_0000001396761802_p41838160"></a><a name="zh-cn_topic_0000001396761802_p41838160"></a>固件未升级</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row61515915"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p13215972"><a name="zh-cn_topic_0000001396761802_p13215972"></a><a name="zh-cn_topic_0000001396761802_p13215972"></a>162</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p63860827"><a name="zh-cn_topic_0000001396761802_p63860827"></a><a name="zh-cn_topic_0000001396761802_p63860827"></a>升级UBOOT失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row37876538"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p3846436"><a name="zh-cn_topic_0000001396761802_p3846436"></a><a name="zh-cn_topic_0000001396761802_p3846436"></a>163</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p43125934"><a name="zh-cn_topic_0000001396761802_p43125934"></a><a name="zh-cn_topic_0000001396761802_p43125934"></a>升级内核失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row52589093"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p30372437"><a name="zh-cn_topic_0000001396761802_p30372437"></a><a name="zh-cn_topic_0000001396761802_p30372437"></a>164</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p44248315"><a name="zh-cn_topic_0000001396761802_p44248315"></a><a name="zh-cn_topic_0000001396761802_p44248315"></a>升级文件系统失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row62690523"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p2296932"><a name="zh-cn_topic_0000001396761802_p2296932"></a><a name="zh-cn_topic_0000001396761802_p2296932"></a>165</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p51833826"><a name="zh-cn_topic_0000001396761802_p51833826"></a><a name="zh-cn_topic_0000001396761802_p51833826"></a>升级miniD固件失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row63851251"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p34532426"><a name="zh-cn_topic_0000001396761802_p34532426"></a><a name="zh-cn_topic_0000001396761802_p34532426"></a>166</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p45663112"><a name="zh-cn_topic_0000001396761802_p45663112"></a><a name="zh-cn_topic_0000001396761802_p45663112"></a>升级参数错误</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row8314828"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p61194181"><a name="zh-cn_topic_0000001396761802_p61194181"></a><a name="zh-cn_topic_0000001396761802_p61194181"></a>167</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p57781598"><a name="zh-cn_topic_0000001396761802_p57781598"></a><a name="zh-cn_topic_0000001396761802_p57781598"></a>Golden区升级失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row50272334"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p63826715"><a name="zh-cn_topic_0000001396761802_p63826715"></a><a name="zh-cn_topic_0000001396761802_p63826715"></a>168</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p2581465"><a name="zh-cn_topic_0000001396761802_p2581465"></a><a name="zh-cn_topic_0000001396761802_p2581465"></a>Hpm包校验失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row23233190"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p28733661"><a name="zh-cn_topic_0000001396761802_p28733661"></a><a name="zh-cn_topic_0000001396761802_p28733661"></a>169</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p45725242"><a name="zh-cn_topic_0000001396761802_p45725242"></a><a name="zh-cn_topic_0000001396761802_p45725242"></a>升级文件不存在</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row8873997"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p38910882"><a name="zh-cn_topic_0000001396761802_p38910882"></a><a name="zh-cn_topic_0000001396761802_p38910882"></a>170</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p64773703"><a name="zh-cn_topic_0000001396761802_p64773703"></a><a name="zh-cn_topic_0000001396761802_p64773703"></a>升级文件解压失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row46092416"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p19806663"><a name="zh-cn_topic_0000001396761802_p19806663"></a><a name="zh-cn_topic_0000001396761802_p19806663"></a>171</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p60835855"><a name="zh-cn_topic_0000001396761802_p60835855"></a><a name="zh-cn_topic_0000001396761802_p60835855"></a>升级超时</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row10651784"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p26032948"><a name="zh-cn_topic_0000001396761802_p26032948"></a><a name="zh-cn_topic_0000001396761802_p26032948"></a>172</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p28294057"><a name="zh-cn_topic_0000001396761802_p28294057"></a><a name="zh-cn_topic_0000001396761802_p28294057"></a>升级版本不支持</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row53319923"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p60621081"><a name="zh-cn_topic_0000001396761802_p60621081"></a><a name="zh-cn_topic_0000001396761802_p60621081"></a>173</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p11360500"><a name="zh-cn_topic_0000001396761802_p11360500"></a><a name="zh-cn_topic_0000001396761802_p11360500"></a>升级失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row2140457"><td class="cellrowborder" rowspan="20" valign="top" width="25%" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p39159352"><a name="zh-cn_topic_0000001396761802_p39159352"></a><a name="zh-cn_topic_0000001396761802_p39159352"></a>消息错误码</p>
</td>
<td class="cellrowborder" valign="top" width="25%" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p17790939"><a name="zh-cn_topic_0000001396761802_p17790939"></a><a name="zh-cn_topic_0000001396761802_p17790939"></a>600</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.1.4.1.3 "><p id="zh-cn_topic_0000001396761802_p31779963"><a name="zh-cn_topic_0000001396761802_p31779963"></a><a name="zh-cn_topic_0000001396761802_p31779963"></a>通用错误</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row17584213"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p9886032"><a name="zh-cn_topic_0000001396761802_p9886032"></a><a name="zh-cn_topic_0000001396761802_p9886032"></a>601</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p62571134"><a name="zh-cn_topic_0000001396761802_p62571134"></a><a name="zh-cn_topic_0000001396761802_p62571134"></a>输入参数不是json</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row26269302"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p17333423"><a name="zh-cn_topic_0000001396761802_p17333423"></a><a name="zh-cn_topic_0000001396761802_p17333423"></a>602</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p61830038"><a name="zh-cn_topic_0000001396761802_p61830038"></a><a name="zh-cn_topic_0000001396761802_p61830038"></a>输入参数存在“/”或者“.”等非法字符</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row19599437"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p11326743"><a name="zh-cn_topic_0000001396761802_p11326743"></a><a name="zh-cn_topic_0000001396761802_p11326743"></a>603</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p45051025"><a name="zh-cn_topic_0000001396761802_p45051025"></a><a name="zh-cn_topic_0000001396761802_p45051025"></a>输入参数有误（缺少字段或者存在未定义字段）</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row2806048"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p22655280"><a name="zh-cn_topic_0000001396761802_p22655280"></a><a name="zh-cn_topic_0000001396761802_p22655280"></a>604</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p23138374"><a name="zh-cn_topic_0000001396761802_p23138374"></a><a name="zh-cn_topic_0000001396761802_p23138374"></a>资源被占用</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row12411575534"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p424114573530"><a name="zh-cn_topic_0000001396761802_p424114573530"></a><a name="zh-cn_topic_0000001396761802_p424114573530"></a>605</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p122416571537"><a name="zh-cn_topic_0000001396761802_p122416571537"></a><a name="zh-cn_topic_0000001396761802_p122416571537"></a>新版本不支持HA安装</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row6918782"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p28539623"><a name="zh-cn_topic_0000001396761802_p28539623"></a><a name="zh-cn_topic_0000001396761802_p28539623"></a>610</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p30008149"><a name="zh-cn_topic_0000001396761802_p30008149"></a><a name="zh-cn_topic_0000001396761802_p30008149"></a>配置文件不存在</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row1637886"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p8755509"><a name="zh-cn_topic_0000001396761802_p8755509"></a><a name="zh-cn_topic_0000001396761802_p8755509"></a>611</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p38107619"><a name="zh-cn_topic_0000001396761802_p38107619"></a><a name="zh-cn_topic_0000001396761802_p38107619"></a>ntp错误</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row7424255"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p56615896"><a name="zh-cn_topic_0000001396761802_p56615896"></a><a name="zh-cn_topic_0000001396761802_p56615896"></a>621</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p22484833"><a name="zh-cn_topic_0000001396761802_p22484833"></a><a name="zh-cn_topic_0000001396761802_p22484833"></a>卷错误</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row1036905"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p25139478"><a name="zh-cn_topic_0000001396761802_p25139478"></a><a name="zh-cn_topic_0000001396761802_p25139478"></a>631</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p23031876"><a name="zh-cn_topic_0000001396761802_p23031876"></a><a name="zh-cn_topic_0000001396761802_p23031876"></a>存储设备不存在，请复位后再试</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row5960297"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p48153984"><a name="zh-cn_topic_0000001396761802_p48153984"></a><a name="zh-cn_topic_0000001396761802_p48153984"></a>632</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p8158669"><a name="zh-cn_topic_0000001396761802_p8158669"></a><a name="zh-cn_topic_0000001396761802_p8158669"></a>存储设备正在被使用，请停止占用程序稍后再试/复位后再试</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row6319160"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p53841325"><a name="zh-cn_topic_0000001396761802_p53841325"></a><a name="zh-cn_topic_0000001396761802_p53841325"></a>633</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p66180034"><a name="zh-cn_topic_0000001396761802_p66180034"></a><a name="zh-cn_topic_0000001396761802_p66180034"></a>获取分区列表失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row58749395"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p48581114"><a name="zh-cn_topic_0000001396761802_p48581114"></a><a name="zh-cn_topic_0000001396761802_p48581114"></a>634</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p42756146"><a name="zh-cn_topic_0000001396761802_p42756146"></a><a name="zh-cn_topic_0000001396761802_p42756146"></a>docker分区处理失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row49260995"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p5104719"><a name="zh-cn_topic_0000001396761802_p5104719"></a><a name="zh-cn_topic_0000001396761802_p5104719"></a>635</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p10829108"><a name="zh-cn_topic_0000001396761802_p10829108"></a><a name="zh-cn_topic_0000001396761802_p10829108"></a>分区大小超出容量限制，请减少待分区大小</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row30353110"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p34760668"><a name="zh-cn_topic_0000001396761802_p34760668"></a><a name="zh-cn_topic_0000001396761802_p34760668"></a>636</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p64150695"><a name="zh-cn_topic_0000001396761802_p64150695"></a><a name="zh-cn_topic_0000001396761802_p64150695"></a>分区损坏，请尝试复位后重试/更换硬盘设备</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row40485347"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p7484118"><a name="zh-cn_topic_0000001396761802_p7484118"></a><a name="zh-cn_topic_0000001396761802_p7484118"></a>637</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p2233842"><a name="zh-cn_topic_0000001396761802_p2233842"></a><a name="zh-cn_topic_0000001396761802_p2233842"></a>挂载路径非空，请确保为空目录或者不存在路径</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row20104586"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p37272126"><a name="zh-cn_topic_0000001396761802_p37272126"></a><a name="zh-cn_topic_0000001396761802_p37272126"></a>640</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p66252229"><a name="zh-cn_topic_0000001396761802_p66252229"></a><a name="zh-cn_topic_0000001396761802_p66252229"></a>固件下载失败，请确认网络是否正常</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row59399152"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p16666522"><a name="zh-cn_topic_0000001396761802_p16666522"></a><a name="zh-cn_topic_0000001396761802_p16666522"></a>641</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p7811030"><a name="zh-cn_topic_0000001396761802_p7811030"></a><a name="zh-cn_topic_0000001396761802_p7811030"></a>固件升级失败，请收集日志</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row3190406"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p61397177"><a name="zh-cn_topic_0000001396761802_p61397177"></a><a name="zh-cn_topic_0000001396761802_p61397177"></a>650</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p7115427"><a name="zh-cn_topic_0000001396761802_p7115427"></a><a name="zh-cn_topic_0000001396761802_p7115427"></a>日志收集失败</p>
</td>
</tr>
<tr id="zh-cn_topic_0000001396761802_row64038849"><td class="cellrowborder" valign="top" headers="mcps1.1.4.1.1 "><p id="zh-cn_topic_0000001396761802_p57403762"><a name="zh-cn_topic_0000001396761802_p57403762"></a><a name="zh-cn_topic_0000001396761802_p57403762"></a>660</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.1.4.1.2 "><p id="zh-cn_topic_0000001396761802_p19193141"><a name="zh-cn_topic_0000001396761802_p19193141"></a><a name="zh-cn_topic_0000001396761802_p19193141"></a>HA内部异常</p>
</td>
</tr>
</tbody>
</table>
