# RESTful接口<a name="ZH-CN_TOPIC_0000001633467953"></a>

## RESTful接口说明<a name="ZH-CN_TOPIC_0000001577530792"></a>

OM SDK提供标准的RESTful接口供开发者进行查看或者调用；支持开发者进行硬件基础管理功能的二次开发或集成。

Redfish是一种基于HTTPS服务的管理标准，利用RESTful接口实现设备管理。每个HTTPS操作都以UTF-8编码的JSON的形式，提交或返回一个资源。就像Web应用程序向浏览器返回HTML一样，RESTful接口会通过同样的传输机制（HTTPS），以JSON的形式向客户端返回数据。

**资源操作<a name="zh-cn_topic_0000001082477678_zh-cn_topic_0178823233_section11673575"></a>**

**表 1**  Redfish接口支持的操作

|操作|说明|
|--|--|
|GET资源URI|返回所请求的资源描述。|
|POST资源URI|创建新资源或执行指定资源的方法。|
|PATCH资源URI|修改当前资源属性。|
|DELETE资源URI|删除指定资源。|

**返回状态码<a name="zh-cn_topic_0000001082477678_zh-cn_topic_0178823233_section37953311"></a>**

**表 2**  状态码说明

|状态码|说明|
|--|--|
|200|请求成功。|
|201|资源成功创建。|
|202|创建任务执行成功。|
|206|部分成功。|
|400|请求非法，客户端侧发生错误并返回错误消息。|
|401|无效的用户请求。|
|403|服务端拒绝请求。|
|404|访问请求资源不存在。|
|405|不支持的操作。|
|409|请求资源的状态之间存在冲突。|
|413|请求实体过大。|
|500|服务端内部错误。|
|501|所请求的功能当前尚未实现。|
|502|网关错误。|
|503|服务不可用。|
|504|网关超时。|

**URL参数<a name="section20778152719278"></a>**

调用接口时，需要输入具体接口的URL。URL都需要传入以下参数，其他的URL参数请参见每个接口的URL参数说明。

**表 3**  URL参数

|参数名|是否必选|参数说明|取值要求|
|--|--|--|--|
|device_ip|必选|登录设备的IP地址。|IPv4或IPv6地址。|

**请求头参数<a name="section13745311172616"></a>**

本文档涉及到的请求头参数说明如下。

**表 4**  请求头

|参数名|是否必选|参数说明|取值要求|
|--|--|--|--|
|X-Auth-Token|必选|请求消息的鉴权参数。|可通过/redfish/v1/SessionService/Sessions创建会话时获得。|
|Content-Type|必选|请求消息的格式。|支持的消息格式包括：<li>application/json</li><li>application/json;charset=utf-8</li><li>multipart/form-data：部分以表单形式上传文件的接口，需要选择该格式</li>|
|AutoRefresh|可选|用于更新会话超时起始记录时间。|字符类型，取值为"true"或者"false"。为"true"时不会更新会话超时起始记录时间，为"false"时会更新会话超时起始记录时间为当前系统运行时间的时间戳。<br>[!NOTE] 说明 <br>不传此参数时默认为true。</br>|

> [!NOTE] 说明
> HTTPS协议规定，请求头字段名称不区分大小写，例如AutoRefresh和autorefresh实际效果一致，本文中涉及到的所有HTTPS请求头字段均符合此要求。

## 公共固定资源的操作<a name="ZH-CN_TOPIC_0000001577530784"></a>

### 查询Redfish版本信息<a name="ZH-CN_TOPIC_0000001628729981"></a>

**命令功能<a name="zh-cn_topic_0000001455150021_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section2794612"></a>**

查询当前使用的Redfish协议的版本号。

**命令格式<a name="zh-cn_topic_0000001455150021_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section25151514"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001455150021_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section24006753"></a>**

无

**使用实例<a name="zh-cn_topic_0000001455150021_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section14734188"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "v1": "/redfish/v1/"
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001455150021_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section65498832"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|v1|字符串|Redfish版本信息。|

### 查询当前根目录服务资源<a name="ZH-CN_TOPIC_0000001578489844"></a>

**命令功能<a name="zh-cn_topic_0000001454870385_zh-cn_topic_0000001129668043_section4248051"></a>**

查询服务器当前根服务资源。

**命令格式<a name="zh-cn_topic_0000001454870385_zh-cn_topic_0000001129668043_section38232461"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001454870385_zh-cn_topic_0000001129668043_section9821607"></a>**

无

**使用实例<a name="zh-cn_topic_0000001454870385_zh-cn_topic_0000001129668043_section21285601"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#ServiceRoot",
    "@odata.id": "/redfish/v1/",
    "@odata.type": "#ServiceRoot.v1_9_0.ServiceRoot",
    "Id": "RootService",
    "Name": "Root Service",
    "RedfishVersion": "1.0.2",
    "UUID": "--",
    "SessionService": {
        "@odata.id": "/redfish/v1/SessionService"
    },
    "AccountService": {
        "@odata.id": "/redfish/v1/AccountService"
    },
    "Systems": {
        "@odata.id": "/redfish/v1/Systems"
    },
    "UpdateService": {
        "@odata.id": "/redfish/v1/UpdateService"
    },
    "Oem": {
        "NetManager": {
            "@odata.id": "/redfish/v1/NetManager"
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001454870385_zh-cn_topic_0000001129668043_section57352685"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|根服务资源模型的OData描述信息。|
|@odata.id|字符串|根服务资源节点的访问路径。|
|@odata.type|字符串|根服务资源类型。|
|Id|字符串|根服务资源的ID。|
|Name|字符串|根服务资源的名称。|
|RedfishVersion|字符串|Redfish的详细版本信息。|
|UUID|字符串|设备的全局唯一标识符。|
|SessionService|对象|会话服务资源。|
|AccountService|对象|账户服务资源。|
|Systems|对象|系统资源。|
|UpdateService|对象|更新服务资源。|
|Oem|对象|自定义属性。|
|NetManager|对象|更新网络配置资源。|
|NetManager@odata\.id|字符串|更新网络配置资源访问路径。|

### 查询Metadata文档<a name="ZH-CN_TOPIC_0000001628730005"></a>

**命令功能<a name="zh-cn_topic_0000001454990217_zh-cn_topic_0000001129668043_section4248051"></a>**

查询Redfish规范里的元数据文档。

**命令格式<a name="zh-cn_topic_0000001454990217_zh-cn_topic_0000001129668043_section38232461"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/$metadata**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001454990217_zh-cn_topic_0000001129668043_section9821607"></a>**

无

**使用实例<a name="zh-cn_topic_0000001454990217_zh-cn_topic_0000001129668043_section21285601"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/$metadata
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```xml
<?xmlversion="1.0"encoding="UTF-8"?>
<!-- Copyright -->
<edmx:Edmxxmlns:edmx=""Version="4.0">
<edmx:Reference>
<edmx:IncludeNamespace="ServiceRoot"/>
<edmx:IncludeNamespace="ServiceRoot.v1_0_2"/>
</edmx:Reference>
<edmx:Reference>
<edmx:IncludeNamespace="AccountService"/>
<edmx:IncludeNamespace="AccountService.v1_0_2"/>
</edmx:Reference>
<edmx:Reference>
<edmx:IncludeNamespace="Chassis"/>
<edmx:IncludeNamespace="Chassis.v1_2_0"/>
</edmx:Reference>
<edmx:Reference>
<edmx:IncludeNamespace="ChassisCollection"/>
</edmx:Reference>
<edmx:Reference>
<edmx:IncludeNamespace="ComputerSystem"/>
<edmx:IncludeNamespace="ComputerSystem.v1_1_0"/>
</edmx:Reference>
<edmx:Reference>
<edmx:IncludeNamespace="ComputerSystemCollection"/>
</edmx:Reference>
...
```

响应码：200

### 查询所有资源的Schema文件<a id="查询所有资源的schema文件"></a>

**命令功能<a name="zh-cn_topic_0000001404510466_zh-cn_topic_0000001129668043_section4248051"></a>**

查询服务器当前所有资源的Schema文件。

**命令格式<a name="zh-cn_topic_0000001404510466_zh-cn_topic_0000001129668043_section38232461"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/JSONSchemas**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001404510466_zh-cn_topic_0000001129668043_section9821607"></a>**

无

**使用实例<a name="zh-cn_topic_0000001404510466_zh-cn_topic_0000001129668043_section21285601"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/JSONSchemas
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#JsonSchemas",
    "@odata.id": "/redfish/v1/JSONSchemas",
    "@odata.type": "#JsonSchemaFileCollection.JsonSchemaFileCollection",
    "Name": "Schema Repository",
    "Description": "Schema Repository",
    "Members@odata.count": 35,
    "Members": [
        {
            "@odata.id": "redfish/v1/Josonschemas/Session.v1_4_0"
        },
        {
            "@odata.id": "redfish/v1/Josonschemas/SessionService.v1_1_8"
        },
        {
            "@odata.id": "redfish/v1/Josonschemas/MessageRegistry.v1_0_0"
        }
        ...
    ]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001404510466_zh-cn_topic_0000001129668043_section57352685"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|Schemas资源模型的OData描述信息。|
|@odata.id|字符串|Schemas资源节点的访问路径。|
|@odata.type|字符串|Schemas资源的类型。|
|Name|字符串|Schemas资源的名称。|
|Description|字符串|Schemas资源的描述。|
|Members@odata.count|数字|当前Schemas资源的数量。|
|Members|列表|Schemas资源列表|
|@odata.id|字符串|单个Schemas资源节点的访问路径。|

### 查询单个文件Schema文件的归档地址<a name="ZH-CN_TOPIC_0000001577530768"></a>

**命令功能<a name="zh-cn_topic_0000001404990186_zh-cn_topic_0000001129668043_section4248051"></a>**

查询服务器当前的单个Schema文件归档地址。

**命令格式<a name="zh-cn_topic_0000001404990186_zh-cn_topic_0000001129668043_section38232461"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/JSONSchemas/_<member\_id_**\>

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001404990186_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<member_id>|必选|Schema文件名。|需要查询的Schema文件名，范围包含在[查询所有资源的Schema文件](#查询所有资源的schema文件)输出里面。|

**使用指南<a name="zh-cn_topic_0000001404990186_zh-cn_topic_0000001129668043_section9821607"></a>**

无

**使用实例<a name="zh-cn_topic_0000001404990186_zh-cn_topic_0000001129668043_section21285601"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/JSONSchemas/Session.v1_4_0
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#JsonSchemasFile/Members/$entity",
    "@odata.id": "/redfish/v1/JSONSchemas/Session.v1_4_0",
    "@odata.type": "#JsonSchemaFile.v1_1_4.JsonSchemaFile",
    "Id": "Session.v1_4_0",
    "Description": "Session Schema File Location",
    "Name": "Session Schema File",
    "Languages": [
        "en"
    ],
    "Schema": "#Session.v1_4_0.Session",
    "Location": [
        {
            "Language": "en",
            "PublicationUri": "http://redfish.dmtf.org/schemas/v1/Session.v1_4_0.json"
        }
    ]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001404990186_zh-cn_topic_0000001129668043_section57352685"></a>**

**表 2**  操作输出说明

|**字段**|**类型**|**说明**|
|--|--|--|
|@odata.context|字符串|消息归档资源模型的OData描述信息。|
|@odata.id|字符串|消息归档资源节点的访问路径。|
|@odata.type|字符串|消息归档资源类型。|
|Id|字符串|消息归档资源的ID。|
|Name|字符串|归档资源的名称。|
|Description|字符串|归档资源的描述信息。|
|Languages|数组|可用模式的语言代码。|
|Schema|字符串|归档文件名称。|
|Location|列表|归档文件路径。|
|Language|字符串|Schema文件的编码语言。|
|Uri（可选）|字符串|本地可访问的Schema文件URI。|
|PublicationUri（可选）|字符串|Redfish官网公开访问的Schema文件URI。|

### 查询OData服务文档<a name="ZH-CN_TOPIC_0000001628610525"></a>

**命令功能<a name="zh-cn_topic_0000001404830230_zh-cn_topic_0000001129668043_section4248051"></a>**

查询OData服务文档。

**命令格式<a name="zh-cn_topic_0000001404830230_zh-cn_topic_0000001129668043_section38232461"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/odata**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001404830230_zh-cn_topic_0000001129668043_section9821607"></a>**

无

**使用实例<a name="zh-cn_topic_0000001404830230_zh-cn_topic_0000001129668043_section21285601"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/odata
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata",
    "value": [
        {
            "name": "Systems",
            "kind": "Singleton",
            "url": "/redfish/v1/Systems"
        },
        {
            "name": "AccountService",
            "kind": "Singleton",
            "url": "/redfish/v1/AccountService"
        },
        {
            "name": "SessionService",
            "kind": "Singleton",
            "url": "/redfish/v1/SessionService"
        },
        {
            "name": "UpdateService",
            "kind": "Singleton",
            "url": "/redfish/v1/UpdateService"
        },
        {
            "name": "NetManager",
            "kind": "Singleton",
            "url": "/redfish/v1/NetManager"
        },

    ]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001404830230_zh-cn_topic_0000001129668043_section57352685"></a>**

**表 1**  操作输出说明

|**字段**|**类型**|**说明**|
|--|--|--|
|@odata.context|字符串|OData服务文档资源模型的OData描述信息。|
|value|列表|OData服务文档资源列表。|
|name|字符串|OData服务文档资源名称。|
|kind|字符串|OData服务文档资源类型。|
|url|字符串|OData服务文档资源访问路径。|

## 硬件管理<a name="ZH-CN_TOPIC_0000001628610529"></a>

### 查询外部设备集合信息<a name="ZH-CN_TOPIC_0000001578449956"></a>

**命令功能<a name="zh-cn_topic_0000001447281821_zh-cn_topic_0000001082925752_zh-cn_topic_0178823241_section6966727"></a>**

查询外部设备集合资源的信息。

**命令格式<a name="zh-cn_topic_0000001447281821_zh-cn_topic_0000001082925752_zh-cn_topic_0178823241_section62700551"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/ExtendedDevices**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447281821_zh-cn_topic_0000001082925752_zh-cn_topic_0178823241_section45579845"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447281821_zh-cn_topic_0000001082925752_zh-cn_topic_0178823241_section7565421"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/ExtendedDevices
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#EdgeSystem/ExtendedDevice/$entity",
    "@odata.id": "/redfish/v1/Systems/ExtendedDevices",
    "@odata.type": "MindXEdgeExtendedDevice.v1_0_0.MindXEdgeExtendedDevice",
    "Name": "ExtendedDevice Collection",
    "Members@odata.count": 3,
    "Members": [{
        "@odata.id": "/redfish/v1/System/ExtendedDevices/eth0"
        },
        {
        "@odata.id": "/redfish/v1/System/ExtendedDevices/eth1"
        },
        {
        "@odata.id": "/redfish/v1/System/ExtendedDevices/disk0"
    }]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447281821_zh-cn_topic_0000001082925752_zh-cn_topic_0178823241_section979931"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|外部设备资源模型的OData描述信息。|
|@odata.id|字符串|外部设备资源的访问路径。|
|@odata.type|字符串|外部设备资源类型。|
|Name|字符串|外部设备资源的名称。|
|Members@odata.count|数字|当前外部设备资源数量。|
|Members|数组|外部设备资源列表。|
|@odata.id|字符串|单个外部设备资源节点的访问路径。|

### 查询外部设备资源信息<a name="ZH-CN_TOPIC_0000001578489848"></a>

**命令功能<a name="zh-cn_topic_0000001446921673_zh-cn_topic_0000001082765804_zh-cn_topic_0178823242_section48747036"></a>**

查询外部设备资源的信息。

**命令格式<a name="zh-cn_topic_0000001446921673_zh-cn_topic_0000001082765804_zh-cn_topic_0178823242_section36070140"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/ExtendedDevices/**_<extend\_id\>_

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001446921673_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|extend_id|必选|外部扩展设备的ID。|外部设备名称，取值为2~64位字符串，支持大小写字母、数字、-、_、.和空格，可通过**redfish/v1/EdgeSystem/ExtendedDevices**获取。|

**使用指南<a name="zh-cn_topic_0000001446921673_zh-cn_topic_0000001082765804_zh-cn_topic_0178823242_section36000200"></a>**

无

**使用实例<a name="zh-cn_topic_0000001446921673_zh-cn_topic_0000001082765804_zh-cn_topic_0178823242_section55566351"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/ExtendedDevices/disk0
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/ExtendedDevices/Members/$entity",
    "@odata.id": "/redfish/v1/Systems/ExtendedDevices/disk0",
    "@odata.type": "#MindXEdgeExtendedDevice.v1_0_0.MindXEdgeExtendedDevice",
    "Id": "disk0",
    "Name": "disk0",
    "DeviceClass": "DISK",
    "DeviceName": "/dev/sda",
    "Manufacturer": "HGST",
    "Model": "XXXX",
    "SerialNumber": "XXXX",
    "Location": "PCIE3-0",
    "FirmwareVersion": "XXXX",
    "Status": {
        "State": "Enabled",
        "Health": "OK"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001446921673_zh-cn_topic_0000001082765804_zh-cn_topic_0178823242_section30335113"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|指定外部设备资源模型的OData描述信息。|
|@odata.id|字符串|指定外部设备资源节点的访问路径。|
|@odata.type|字符串|指定外部设备资源类型。|
|Id|字符串|指定外部设备资源的唯一标识。|
|Name|字符串|指定外部设备资源的名称。|
|DeviceClass|字符串|外部设备的类别，例如：<li>HDD</li><li>USB</li>|
|DeviceName|字符串|外部设备在系统中的设备名。|
|Manufacturer|字符串|外部设备的制造厂商。|
|Model|字符串|外部设备的型号。|
|SerialNumber|字符串|外部设备的序列号。|
|FirmwareVersion|字符串|外部设备的固件版本。|
|Location|字符串|外部设备在系统中的位置。|
|Status|对象|外部设备的状态。<li>Health：系统资源健康状态</li><li>State：系统资源使能状态</li>|

### 查询外部设备模组资源集合<a name="ZH-CN_TOPIC_0000001578489888"></a>

**命令功能<a name="section33170234613"></a>**

查询外部设备模组资源集合。当前OM SDK支持自定义模组扩展，具体接口扩展功能可参照模组扩展章节开发。

**命令格式<a name="section14287487617"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Modules**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="section18372260715"></a>**

无

**使用实例<a name="section891751818711"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Modules
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Modules/$entity",
    "@odata.id": "/redfish/v1/Systems/Modules",
    "@odata.type": "#MindXEdgeModuleCollection.MindXEdgeModuleCollection",
    "Name": "Device Module Collection",
    "Members@odata.count": 0,
    "Members": []
}
```

响应码：200

**输出说明<a name="section869116331879"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|外部设备模组资源集合模型的OData描述信息。|
|@odata.id|字符串|外部设备模组资源集合的访问路径。|
|@odata.type|字符串|外部设备模组资源集合的类型。|
|Name|字符串|外部设备模组资源集合的名称。|
|Members@odata.count|数字|外部设备模组的数量。|

### 查询外部设备模组信息<a name="ZH-CN_TOPIC_0000001577530772"></a>

**命令功能<a name="section33170234613"></a>**

查询外部设备模组信息。当前OM SDK支持自定义模组扩展，具体接口扩展功能可参照模组扩展章节开发。

**命令格式<a name="section14287487617"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Modules/**_<module\_id\>_

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<module_id>|必选|模组名称。|匹配任何由字母、数字、空格、减号（-）或下划线组成的字符串，长度1~127。<br>该参数根据第三方扩展而来，当前OM SDK无具体可访问模组名称。|

**使用指南<a name="section18372260715"></a>**

无

**使用实例<a name="section891751818711"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Modules/XXX
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例（当前OM SDK无可访问外设模组，当前只会返回以下错误）：

```json
{
    "error": {
        "code": "Base.1.0.GeneralError",
        "message": "A GeneralError has occurred. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that a general error has occurred.",
                "Message": "XXX does not exist",
                "Severity": "Critical",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None",
                "Oem": {
                    "status": null
                }
            }
        ]
    }
}
```

响应码：404

**输出说明<a name="section869116331879"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|消息详情。|
|@Message.ExtendedInfo|对象|扩展消息|
|@odata.type|字符串|会话资源类型。|
|Description|字符串|详细信息。|
|Message|字符串|返回消息。|
|Severity|字符串|严重性，Redfish支持的严重级别。<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|传递的参数数量。|
|ParamTypes|数组|传递的参数类型。|
|Resolution|字符串|解决方案描述。|
|Oem|对象|扩展字段|
|status|字符串|扩展字段状态信息|

### 查询外部设备信息<a name="ZH-CN_TOPIC_0000001578449916"></a>

**命令功能<a name="section33170234613"></a>**

查询外部设备信息。当前OM SDK支持自定义模组扩展，具体接口扩展功能可参照模组扩展章节开发。

**命令格式<a name="section14287487617"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Modules/**_<module\_id\>_/_<device\_id\>_

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="section353557665"></a>**

**表 1**  URL参数

<a name="table1083016142054"></a>
<table><thead align="left"><tr id="row1192510141851"><th class="cellrowborder" valign="top" width="25%" id="mcps1.2.5.1.1"><p id="p79258145517"><a name="p79258145517"></a><a name="p79258145517"></a>参数名</p>
</th>
<th class="cellrowborder" valign="top" width="25%" id="mcps1.2.5.1.2"><p id="p122681949195918"><a name="p122681949195918"></a><a name="p122681949195918"></a>是否必选</p>
</th>
<th class="cellrowborder" valign="top" width="25%" id="mcps1.2.5.1.3"><p id="p992514141452"><a name="p992514141452"></a><a name="p992514141452"></a>参数说明</p>
</th>
<th class="cellrowborder" valign="top" width="25%" id="mcps1.2.5.1.4"><p id="p1392520141254"><a name="p1392520141254"></a><a name="p1392520141254"></a>取值</p>
</th>
</tr>
</thead>
<tbody><tr id="row0675132212719"><td class="cellrowborder" valign="top" width="25%" headers="mcps1.2.5.1.1 "><p id="p1467502202720"><a name="p1467502202720"></a><a name="p1467502202720"></a>&lt;module_id&gt;</p>
</td>
<td class="cellrowborder" valign="top" width="25%" headers="mcps1.2.5.1.2 "><p id="p18268949155919"><a name="p18268949155919"></a><a name="p18268949155919"></a>必选</p>
</td>
<td class="cellrowborder" valign="top" width="25%" headers="mcps1.2.5.1.3 "><p id="p667552272715"><a name="p667552272715"></a><a name="p667552272715"></a>外部模组名称。</p>
</td>
<td class="cellrowborder" rowspan="2" valign="top" width="25%" headers="mcps1.2.5.1.4 "><p id="p10675152212279"><a name="p10675152212279"></a><a name="p10675152212279"></a>匹配任何由字母、数字、空格、减号（-）或下划线组成的字符串，长度1~127。</p>
<p id="p458715213184"><a name="p458715213184"></a><a name="p458715213184"></a>该参数根据第三方扩展而来，当前<span id="ph7957054121915"><a name="ph7957054121915"></a><a name="ph7957054121915"></a>OM SDK</span>无具体可访问模组名称。</p>
</td>
</tr>
<tr id="row12115146123318"><td class="cellrowborder" valign="top" headers="mcps1.2.5.1.1 "><p id="p131167613333"><a name="p131167613333"></a><a name="p131167613333"></a>&lt;device_id&gt;</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.5.1.2 "><p id="p026874955911"><a name="p026874955911"></a><a name="p026874955911"></a>必选</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.5.1.3 "><p id="p13116186113315"><a name="p13116186113315"></a><a name="p13116186113315"></a>外部设备名称。</p>
</td>
</tr>
</tbody>
</table>

**使用指南<a name="section18372260715"></a>**

无

**使用实例<a name="section891751818711"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Modules/XXX/XXX-XXX
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例（当前OM SDK无可访问外设设备，当前只会返回以下错误）：

```json
{
    "error": {
        "code": "Base.1.0.GeneralError",
        "message": "A GeneralError has occurred. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that a general error has occurred.",
                "Message": "XXX does not exist",
                "Severity": "Critical",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None",
                "Oem": {
                    "status": null
                }
            }
        ]
    }
}
```

响应码：404

**输出说明<a name="section869116331879"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|消息详情。|
|@Message.ExtendedInfo|对象|扩展消息|
|@odata.type|字符串|会话资源类型。|
|Description|字符串|详细信息。|
|Message|字符串|返回消息。|
|Severity|字符串|严重性，Redfish支持的严重级别。<li>OK</li><li>Warning</li><li>Critical|
|NumberOfArgs|数字|传递的参数数量。|
|ParamTypes|数组|传递的参数类型。|
|Resolution|字符串|解决方案描述。|
|Oem|对象|扩展字段|
|status|字符串|扩展字段状态信息|

### 修改外部设备信息<a name="ZH-CN_TOPIC_0000001577810580"></a>

**命令功能<a name="section33170234613"></a>**

修改外部设备信息。当前OM SDK支持自定义模组扩展，具体接口扩展功能可参照模组扩展章节开发。

**命令格式<a name="section14287487617"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Modules/<module\_id\>/<device\_id\>**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
   "Attributes":{
     "status": "inactive"
   }
}
```

**URL参数<a name="section353557665"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<module_id>|必选|外部模组名称。|匹配任何由字母、数字、空格、减号（-）或下划线组成的字符串，长度1~127。<br>注：该参数根据第三方扩展而来，当前OM SDK无具体可访问模组名称。</br>|
|<device_id>|必选|外部设备名称。|匹配任何由字母、数字、空格、减号（-）或下划线组成的字符串，长度1~127。<br>该参数根据第三方扩展而来，当前OM SDK无具体可访问模组名称。</br>|

**请求参数<a name="section175001020201512"></a>**

**表 2**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|Attributes|必选|修改的设备属性与属性值。|对象，取值为修改的设备属性。|

**使用指南<a name="section18372260715"></a>**

无

**使用实例<a name="section891751818711"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/Modules/XXX/XXX-XXX
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例（当前OM SDK无可访问外设设备，当前只会返回以下错误）：

```json
{
    "error": {
        "code": "Base.1.0.GeneralError",
        "message": "A GeneralError has occurred. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that a general error has occurred.",
                "Message": "XXX does not exist",
                "Severity": "Critical",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None",
                "Oem": {
                    "status": null
                }
            }
        ]
    }
}
```

响应码：404

**输出说明<a name="section869116331879"></a>**

**表 3**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|消息详情。|
|@Message.ExtendedInfo|对象|扩展消息|
|@odata.type|字符串|会话资源类型。|
|Description|字符串|详细信息。|
|Message|字符串|返回消息。|
|Severity|字符串|严重性，Redfish支持的严重级别。<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|传递的参数数量。|
|ParamTypes|数组|传递的参数类型。|
|Resolution|字符串|解决方案描述。|
|Oem|对象|扩展字段。|
|status|字符串|扩展字段状态信息。|

## 软件管理<a name="ZH-CN_TOPIC_0000001578489900"></a>

### 查询升级资源集合<a name="ZH-CN_TOPIC_0000001577810564"></a>

**命令功能<a name="zh-cn_topic_0000001400456992_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section118980111424"></a>**

查询固件升级资源集合。

**命令格式<a name="zh-cn_topic_0000001400456992_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section5863740104310"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/UpdateService**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001400456992_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section07201647164418"></a>**

无

**使用说明<a name="zh-cn_topic_0000001400456992_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section6834157194419"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/UpdateService
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#UpdateService",
    "@odata.id": "/redfish/v1/UpdateService",
    "@odata.type": "#UpdateService.v1_0_0.UpdateService",
    "Actions": {
        "#UpdateService.Reset": {
            "target": "/redfish/v1/UpdateService/Actions/UpdateService.Reset"
        },
        "#UpdateService.SimpleUpdate": {
            "target": "/redfish/v1/UpdateService/Actions/UpdateService.SimpleUpdate"
        }
    },
    "FirmwareInventory": {
        "@odata.id": "/redfish/v1/UpdateService/FirmwareInventory"
    },
    "Id": "UpdateService",
    "Name": "Update Service",
    "ServiceEnabled": null,
    "Status": {
        "Health": null,
        "Status": null
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001400456992_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section115261420458"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|升级任务资源模型的OData描述信息。|
|@odata.id|字符串|当前任务资源的访问路径。<br> [!NOTE] 说明</br>您可以访问该资源，获取该任务的详细信息。|
|@odata.type|字符串|升级任务资源的类型。|
|Actions.#UpdateService.Reset.target|字符串|固件生效URL。|
|Actions.#UpdateService.SimpleUpdate.target|字符串|升级固件URL。|
|FirmwareInventory.@odata\.id|字符串|文件上传URL。|
|Id|字符串|当前任务资源ID。|
|Name|字符串|当前任务资源名称。|
|ServiceEnabled|字符串|升级服务使能开关。|
|Status.Health|字符串|升级服务健康状态。|
|Status.Status|字符串|升级服务状态。|

### 升级固件<a name="ZH-CN_TOPIC_0000001578449940"></a>

**命令功能<a name="zh-cn_topic_0000001396761842_zh-cn_topic_0000001129385269_zh-cn_topic_0178823227_section62478092"></a>**

升级固件。

**命令格式<a name="zh-cn_topic_0000001396761842_zh-cn_topic_0000001129385269_zh-cn_topic_0178823227_section25431924"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/UpdateService/Actions/UpdateService.SimpleUpdate**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "ImageURI": filename,
    "TransferProtocol": protocol
}
```

**请求参数<a name="zh-cn_topic_0000001396761842_section1163011281024"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|ImageURI|必选|升级包的名称|自定义软件包名称，具体名称由用户提供。字符串，文件名支持长度为1~255个字符，由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（_.-）组成，且不能包含..|
|TransferProtocol|必选|上传升级包时使用的协议|https|

**使用指南<a name="zh-cn_topic_0000001396761842_zh-cn_topic_0000001129385269_zh-cn_topic_0178823227_section46719949"></a>**

无

**使用实例<a name="zh-cn_topic_0000001396761842_zh-cn_topic_0000001129385269_zh-cn_topic_0178823227_section17826361"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/UpdateService/Actions/UpdateService.SimpleUpdate
```

请求头：

```http
X-Auth-Token: auth_value 
Content-Type: application/json
```

请求消息体：

```json
{
    "ImageURI":"xxx_{_Version}_linux-aarch64.zip",
    "TransferProtocol":"https"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#TaskService/Tasks/Members/$entity",
    "@odata.type": "#Task.v1_0_2.Task",
    "@odata.id": "/redfish/v1/TaskService/Tasks/1",
    "Id": "1",
    "Name": "Upgrade Task",
    "TaskState": "Running",
    "StartTime": "2023-03-26 14:35:34",
    "Messages": [{
        "upgradeState": "Running"
    }],
    "PercentComplete": 0,
    "Module": "",
    "Version": ""
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001396761842_zh-cn_topic_0000001129385269_zh-cn_topic_0178823227_section26219523"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|升级任务资源模型的OData描述信息。|
|@odata.type|字符串|升级任务资源的类型。|
|@odata.id|字符串|当前升级资源的任务描述。|
|Id|数字|升级任务资源的ID。|
|Name|字符串|升级任务资源的名称。|
|TaskState|字符串|升级任务资源的状态。<li>New</li><li>Starting</li><li>Running</li><li>Failed</li>|
|StartTime|字符串|升级任务的起始时间。|
|Messages|对象|升级任务的相关信息。|
|Messages.upgradeState|字符串|升级任务状态详细描述|
|PercentComplete|数字|升级进度。|
|Module|字符串|固件类型。|
|Version|字符串|升级包版本。|

### 查询固件升级状态信息<a name="ZH-CN_TOPIC_0000001628490529"></a>

**命令功能<a name="zh-cn_topic_0000001447121545_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section118980111424"></a>**

查询固件升级状态信息。

**命令格式<a name="zh-cn_topic_0000001447121545_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section5863740104310"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/UpdateService/Actions/UpdateService.SimpleUpdate**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447121545_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section07201647164418"></a>**

无

**使用说明<a name="zh-cn_topic_0000001447121545_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section6834157194419"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/UpdateService/Actions/UpdateService.SimpleUpdate
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#TaskService/Tasks/Members/$entity",
    "@odata.type": "#Task.v1_6_0.Task",
    "@odata.id": "/redfish/v1/TaskService/Tasks/1",
    "Id": "1",
    "Name": "Upgrade Task", 
    "TaskState": "New",
    "StartTime": "",
    "Messages": {
        "upgradeState": "ERR.0-1-Not upgraded"
    },
    "PercentComplete": 0,
    "Module": "",
    "Version": ""
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447121545_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section115261420458"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|升级任务资源模型的OData描述信息。|
|@odata.id|字符串|当前任务资源的访问路径。<br> [!NOTE] 说明</br>您可以访问该资源，获取该任务的详细信息。|
|@odata.type|字符串|升级任务资源的类型。|
|Id|字符串|升级任务资源的ID。|
|Name|字符串|升级任务资源的名称。|
|TaskState|字符串|升级任务资源的状态。<li>New</li><li>Success</li><li>Running</li><li>Failed|
|StartTime|字符串|升级任务的起始时间。|
|Messages|对象|升级任务的相关信息。|
|Messages.upgradeState|字符串|升级任务状态详细描述。|
|PercentComplete|数字|升级进度。|
|Module|字符串|固件类型。|
|Version|字符串|升级包版本。|

### 文件上传<a name="ZH-CN_TOPIC_0000001628849909"></a>

**命令功能<a name="zh-cn_topic_0000001396921746_zh-cn_topic_0000001129668041_zh-cn_topic_0178823228_section58703041"></a>**

通过接口进行文件上传，上传成功后文件被统一放在“/run/web”目录下。

- zip文件放在“/run/web/zip”目录下。
- crl，cer，crt文件放在“/run/web/cert”目录下。
- conf文件放在“/run/web/conf”目录下。
- ini文件放在“/run/web/ini”目录下。

**命令格式<a name="zh-cn_topic_0000001396921746_zh-cn_topic_0000001129668041_zh-cn_topic_0178823228_section58565325"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/UpdateService/FirmwareInventory**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "imgfile": imgfile,
    "size": size
}
```

**请求消息<a name="zh-cn_topic_0000001396921746_section212563019519"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值要求|
|--|--|--|--|
|imgfile|必选|传输文件名称。通过Form-Data传输。|上传文件的文件名需要满足长度为1~255个字符，由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（_.-）组成，且不能包含连续两个点（..）。支持的文件类型如下：<li>zip</li><li>cer</li><li>crl</li><li>crt</li><li>conf</li><li>ini</li>当KEY的值是imgfile时，对应VALUE的值选择对应文件。|
|size|可选|上传文件大小。通过Form-Data传输。|取值为数字，支持的取值大小如下。<li>zip：文件大小需小于512MB。</li><li>cer，crl，crt，ini：最大支持10KB。</li><li>conf：最大支持20M。</li>|

**使用指南<a name="zh-cn_topic_0000001396921746_zh-cn_topic_0000001129668041_zh-cn_topic_0178823228_section46170875"></a>**

无

**使用实例<a name="zh-cn_topic_0000001396921746_zh-cn_topic_0000001129668041_zh-cn_topic_0178823228_section12884695"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/UpdateService/FirmwareInventory
```

请求头：

```http
X-Auth-Token: auth_value 
```

请求消息体：

```http
{
    "imgfile": 4.conf,
    "size": 202
}
```

响应样例：

```http
{
    "message": "Upload [4.conf] file successfully.",
    "status": 202
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001396921746_zh-cn_topic_0000001129668041_zh-cn_topic_0178823228_section48853398"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|status|数字|请求返回码。|
|message|字符串|详细信息。|

### 查询日志服务集合资源信息<a name="ZH-CN_TOPIC_0000001628730025"></a>

**命令功能<a name="zh-cn_topic_0000001129668051_zh-cn_topic_0178823256_section22359686"></a>**

查询当前日志服务集合资源信息。

**命令格式<a name="zh-cn_topic_0000001129668051_zh-cn_topic_0178823256_section67019453"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/LogServices**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001129668051_zh-cn_topic_0178823256_section59866645"></a>**

无

**使用实例<a name="zh-cn_topic_0000001129668051_zh-cn_topic_0178823256_section1928895"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/LogServices
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/LogServices/$entity",
    "@odata.id": "/redfish/v1/Systems/LogServices",
    "@odata.type": "#LogServiceCollection.LogServiceCollection",
    "Name": "LogService Collection",
    "Members@odata.count": 4,
    "Members": [
        {
            "@odata.id": "/redfish/v1/Systems/LogServices/NPU"
        },
        {
            "@odata.id": "/redfish/v1/Systems/LogServices/MindXOM"
        },
        {
            "@odata.id": "/redfish/v1/Systems/LogServices/OSDivers"
        },
    ],
    "Oem": {
        "progress": {
            "@odata.id": "/redfish/v1/Systems/LogServices/progress"
        },
        "Actions": {
            "#download": {
                "target": "/redfish/v1/Systems/LogServices/Actions/download"
            }
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001129668051_zh-cn_topic_0178823256_section17360061"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|日志服务集合资源模型的OData描述信息。|
|@odata.id|字符串|日志服务集合资源的访问路径。|
|@odata.type|字符串|日志服务集合资源的类型。|
|Name|字符串|日志服务集合资源的名称。|
|Members@odata.count|数字|当前日志服务资源数量。|
|Members|对象|日志服务资源列表。|
|Members@odata\.id|字符串|单个日志服务资源节点的访问路径。|
|Oem|对象|自定义字段。|
|progress|对象|日志下载进度信息。|
|progress@odata\.id|字符串|日志下载进度信息的访问路径。|
|Actions|对象|日志下载信息。|
|target|字符串|下载日志信息访问路径。|

### 下载日志信息<a name="ZH-CN_TOPIC_0000001628730017"></a>

**命令功能<a name="zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section52444064"></a>**

下载日志信息。

**命令格式<a name="zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section2234534"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/LogServices/Actions/download**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "name": resourceID
}
```

**请求参数<a name="zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section20110814"></a>**

**表 1**  参数说明

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|name|必选|日志服务资源的ID。|字符串，当前可为：<li>OSDivers</li><li>NPU</li><li>MindXOM</li><li>MEF</li>且以1个空格分隔，详见下面使用实例。|

**使用指南<a name="zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section46779599"></a>**

无

**使用实例<a name="zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section18363212"></a>**

请求样例：

```http
POST https://10.10.10.10//redfish/v1/Systems/LogServices/Actions/download 
```

请求头：

```http
X-Auth-Token: auth_value
header_type: application/json
```

请求消息体：

```json
{
    "name":"NPU MindXOM OSDivers"
}
```

响应样例：无（直接导出文件）

响应码：200

**输出说明<a name="zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section31051180"></a>**

直接下载文件。

### 日志下载进度信息<a name="ZH-CN_TOPIC_0000001628730009"></a>

**命令功能<a name="zh-cn_topic_0000001453914320_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section118980111424"></a>**

查询日志下载进度信息。

**命令格式<a name="zh-cn_topic_0000001453914320_section196111572517"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/LogServices/progress**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001453914320_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section07201647164418"></a>**

无

**使用说明<a name="zh-cn_topic_0000001453914320_section885854072618"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/LogServices/progress
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/LogServices/progress",
    "@odata.type": "#Task.v1_6_0.Task",
    "@odata.id": "/redfish/v1/Systems/LogServices/progress",
    "Id": "Log Collection Task",
    "Name": "Log Collection Task",
    "Description": "Get download logs progress.",
    "TaskState": "Success",
    "PercentComplete": 0
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001453914320_zh-cn_topic_0000001129668055_zh-cn_topic_0180797872_section115261420458"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|资源模型的OData描述信息。|
|@odata.id|字符串|资源的访问路径。|
|@odata.type|字符串|资源的类型。|
|Id|字符串|资源Id|
|Name|字符串|资源的名称。|
|Description|字符串|资源的描述。|
|TaskState|字符串|日志下载的状态。|
|PercentComplete|数字|日志下载的进度。|

### 升级文件生效<a name="ZH-CN_TOPIC_0000001628610545"></a>

**命令功能<a name="zh-cn_topic_0000001525412293_section156424052620"></a>**

将已升级的固件文件生效。

**命令格式<a name="zh-cn_topic_0000001525412293_section1128111702617"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/UpdateService/Actions/UpdateService.Reset**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "ResetType":type
}
```

**请求参数<a name="zh-cn_topic_0000001525412293_section17284143414148"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|ResetType|必选|复位类型|字符串，支持的类型为GracefulRestart|

**使用指南<a name="zh-cn_topic_0000001525412293_section11875191416274"></a>**

无

**使用实例<a name="zh-cn_topic_0000001525412293_section1590762312714"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/UpdateService/Actions/UpdateService.Reset
```

请求头：

```http
X-Auth-Token: auth_value
header_type: application/json
```

请求消息体：

```json
{
    "ResetType":"GracefulRestart"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#TaskService/Tasks/Members/$entity",
    "@odata.type": "#Task.v1_6_0.Task",
    "@odata.id": "/redfish/v1/TaskService/Tasks/1",
    "Id": null,
    "Name": "Upgrade Task",
    "TaskState": null,
    "StartTime": null,
    "Messages": null,
    "PercentComplete": null,
    "Module": null,
    "Version": null
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001525412293_section790880172816"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|升级任务资源模型的OData描述信息。|
|@odata.type|字符串|升级任务资源类型。|
|@odata.id|字符串|当前任务资源的访问路径。<br> [!NOTE] 说明</br>您可以访问该资源，获取该任务的详细信息。|
|Id|字符串|升级任务资源的ID。|
|Name|字符串|硬盘资源集合的名称。|
|TaskState|字符串|升级任务资源的状态。<li>New</li><li>Success</li><li>Running</li><li>Failed</li>|
|StartTime|字符串|升级任务的起始时间。|
|Messages|对象|升级任务的相关信息。|
|PercentComplete|数字|升级进度。|
|Module|字符串|固件类型。|
|Version|字符串|升级包版本。|

### 恢复出厂设置<a name="ZH-CN_TOPIC_0000001628610513"></a>

**命令功能<a name="zh-cn_topic_0000001397241574_zh-cn_topic_0000001082925746_section34701638643"></a>**

远程恢复出厂设置。

**命令格式<a name="zh-cn_topic_0000001397241574_zh-cn_topic_0000001082925746_section24721538140"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Actions/RestoreDefaults.Reset**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "ethernet": ethernet,
    "root_pwd": password
}
```

**请求参数<a name="zh-cn_topic_0000001397241574_section810253516167"></a>**

**表 1**  参数说明

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|ethernet|必选|网口设备名称。|字符串，例如：eth0:1。长度为0~32字符，可由小写字母（a~z）、数字（0~9）和其他字符（:）组成，且不能以冒号开头；为空则表示不保留IP恢复出厂设置。|
|root_pwd|必选|root用户密码。|字符串，当前系统的root用户密码。|

**使用指南<a name="zh-cn_topic_0000001397241574_zh-cn_topic_0000001082925746_section1948613381749"></a>**

接口已默认开发，但功能暂未完全实现，需要开发者适配恢复出厂设置命令。

**使用实例<a name="zh-cn_topic_0000001397241574_zh-cn_topic_0000001082925746_section84875382419"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/RestoreDefaults/Reset
```

请求头：

```http
X-Auth-Token: auth_value
Content-Type: application/json
```

请求消息体：

```json
{
    "ethernet": "eth0",
    "root_pwd": "password"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Restore defaults system successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397241574_zh-cn_topic_0000001082925746_section124891438743"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|消息详情。|
|@odata.type|字符串|会话资源类型。|
|Description|字符串|详细信息。|
|Message|字符串|返回消息。|
|Severity|字符串|严重性，Redfish支持的严重级别。<li>OK</li><li>Warning</li><li>Critical|
|NumberOfArgs|数字|传递的参数数量。|
|ParamTypes|数组|传递的参数类型。|
|Resolution|字符串|解决方案描述。|

## 时间管理<a name="ZH-CN_TOPIC_0000001577530800"></a>

### 查询系统时间<a name="ZH-CN_TOPIC_0000001628490553"></a>

**命令功能<a name="zh-cn_topic_0000001397241550_zh-cn_topic_0000001129668047_section135421946192811"></a>**

查询当前系统时间。

**命令格式<a name="zh-cn_topic_0000001397241550_zh-cn_topic_0000001129668047_section6651103014298"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/Systems/SystemTime**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001397241550_zh-cn_topic_0000001129668047_zh-cn_topic_0178823230_section46657603"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397241550_zh-cn_topic_0000001129668047_section325861693816"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/SystemTime
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/SystemTime",
    "@odata.id": "/redfish/v1/Systems/SystemTime",
    "@odata.type": "#MindXEdgeSystemTime.MindXEdgeSystemTime",
    "Id": "SystemTime",
    "Name": "SystemTime",
    "Datetime": "Wed Dec 14 07:15:42 2022"
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397241550_zh-cn_topic_0000001129668047_section151814254014"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|系统时间模型的OData描述信息。|
|@odata.id|字符串|系统时间的访问路径。|
|@odata.type|字符串|系统时间的类型。|
|Name|字符串|系统时间的名称。|
|Id|字符串|系统时间的ID。|
|Datetime|字符串|系统当前时间。<br> [!NOTE] 说明</br>此接口可获取精度较高的系统时间。|

## 网络管理<a name="ZH-CN_TOPIC_0000001628849961"></a>

### 查询无线网络资源信息<a name="ZH-CN_TOPIC_0000001578489876"></a>

**命令功能<a name="zh-cn_topic_0000001424489004_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section54694405"></a>**

查询无线网络顶层资源信息。

**命令格式<a name="zh-cn_topic_0000001424489004_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section22487602"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/LTE**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001424489004_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section9556486"></a>**

接口已默认开发，功能根据无线网卡适配。

**使用实例<a name="zh-cn_topic_0000001424489004_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section18899510"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/LTE
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.type": "#MindXEdgeLTE.v1_0_0.MindXEdgeLTE",
    "@odata.context": "/redfish/v1/$metadata#Systems/LTE",
    "@odata.id": "/redfish/v1/Systems/LTE",
    "Id" : "LTE",
    "Name": "LTE",
    "StatusInfo": {
        "@odata.id": "/redfish/v1/Systems/LTE/StatusInfo"
    },
    "ConfigInfo": {
        "@odata.id": "/redfish/v1/Systems/LTE/ConfigInfo"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001424489004_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section35877865"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.type|字符串|无线网络资源类型。|
|@odata.context|字符串|无线网络资源模型的OData描述信息。|
|@odata.id|字符串|无线网络资源模型节点的访问路径。|
|Id|字符串|无线网络资源的唯一标识。|
|Name|字符串|无线网络资源的名称。|
|StatusInfo|对象|无线网络接口状态资源的访问路径。|
|ConfigInfo|对象|无线网络APN接口资源的访问路径。|

### 查询无线网络接口状态资源信息<a name="ZH-CN_TOPIC_0000001577530788"></a>

**命令功能<a name="zh-cn_topic_0000001446921681_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section54694405"></a>**

查询无线网络接口资源信息。

**命令格式<a name="zh-cn_topic_0000001446921681_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section22487602"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/LTE/StatusInfo**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001446921681_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section9556486"></a>**

接口已默认开发，功能根据无线网卡适配。

**使用实例<a name="zh-cn_topic_0000001446921681_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section18899510"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/LTE/StatusInfo
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.type": "#MindXEdgeLTE.v1_0_0.MindXEdgeLTE",
    "@odata.context": "/redfish/v1/$metadata#Systems/LTE/StatusInfo",
    "@odata.id": "/redfish/v1/Systems/LTE/StatusInfo",
    "Id": "LTE StatusInfo",
    "Name": "LTE StatusInfo",
    "default_gateway": false,
    "lte_enable": true,
    "sim_exist": true,
    "state_lte": true,
    "state_data": true,
    "network_signal_level": 4,
    "network_type": "4G",
    "ip_addr": "xx.xx.xx.xx"
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001446921681_zh-cn_topic_0000001129385263_zh-cn_topic_0198731001_section35877865"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.type|字符串|无线网络接口状态资源的类型。|
|@odata.context|字符串|无线网络接口状态资源的OData描述信息。|
|@odata.id|字符串|无线网络接口状态资源的访问路径。|
|Id|字符串|无线网络接口状态资源的唯一标识。|
|Name|字符串|无线网络接口状态资源的名称。|
|default_gateway|布尔值|eth0或eth1是否配置默认网关。默认为false。<li>true：配置默认网关。</li><li>false：未配置默认网关。</li>|
|lte_enable|布尔值|无线网络是否使能，默认值为false。<li>true：使能。</li><li>false：不可用。</li>|
|sim_exist|布尔值|SIM卡是否在位。<li>true：在位。</li><li>false：不在位。</li>|
|state_data|布尔值|移动数据的开关状态。<li>true：打开。</li><li>false：关闭。</li>|
|state_lte|布尔值|无线网络的开关状态。<li>true：打开。</li><li>false：关闭。</li>|
|network_signal_level|数字|信号的强度。<br>取值范围：0~5级。默认值为None，取值为None时代表移动数据开关处于关闭状态。取值为0时，代表网络断开，这时network_type的取值为null。</br>|
|network_type|字符串|网络状态。<br>取值范围：5G、4G、3G、2G。</br>当网络断开，取值为null。|
|ip_addr|字符串|无线网络拨号成功后，要显示的IP地址。|

### 配置无线网络状态资源信息<a name="ZH-CN_TOPIC_0000001628849929"></a>

**命令功能<a name="zh-cn_topic_0000001447281869_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_section1246702"></a>**

配置无线网络接口资源信息。

**命令格式<a name="zh-cn_topic_0000001447281869_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_section11220318"></a>**

操作类型：**PATCH**

**URL：https://**_device\_ip_**/redfish/v1/Systems/LTE/StatusInfo**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "state_lte":state_lte,
    "state_data":state_data
}
```

**请求参数<a name="zh-cn_topic_0000001447281869_section1130055014439"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|state_lte|必选|无线网络的开关状态。|布尔值<li>true：打开无线网络开关。</li><li>false：关闭无线网络开关。</li>|
|state_data|必选|移动数据的开关状态。|布尔值<li>true：打开移动数据。</li><li>false：关闭移动数据。</li>[!NOTE] 说明<br>当state_lte取值为false时，state_data取值为true无效。</br>|

**使用指南<a name="zh-cn_topic_0000001447281869_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_section36430550"></a>**

接口已默认开发，功能根据无线网卡适配。

**使用实例<a name="zh-cn_topic_0000001447281869_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_section59439495"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/LTE/StatusInfo
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "state_lte":true,
    "state_data":true
}
```

响应样例：

```json
{
    "@odata.type": "#MindXEdgeLTE.v1_0_0.MindXEdgeLTE",
    "@odata.context": "/redfish/v1/$metadata#Systems/LTE/StatusInfo",
    "@odata.id": "/redfish/v1/Systems/LTE/StatusInfo",
    "Id" : "LTE StatusInfo",
    "Name": "LTE StatusInfo",
    "default_gateway": false,
    "lte_enable": true,
    "sim_exist": true,
    "state_lte": true,
    "state_data": true,
    "network_signal_level": 4,
    "network_type": "4G",
    "ip_addr": "xx.xx.xx.xx"
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447281869_zh-cn_topic_0000001082925740_zh-cn_topic_0198836248_section65193414"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.type|字符串|无线网络接口状态资源类型。|
|@odata.context|字符串|无线网络接口状态资源模型的OData描述信息。|
|@odata.id|字符串|无线网络接口状态资源模型节点的访问路径。|
|Id|字符串|无线网络接口状态资源的唯一标识。|
|Name|字符串|无线网络接口状态资源的名称。|
|default_gateway|布尔值|eth0或eth1是否配置默认网关。默认为false。<li>true：配置默认网关。</li><li>false：未配置默认网关。</li>|
|lte_enable|布尔值|无线网络是否使能，默认值为false。<li>true：使能。</li><li>false：不可用。</li>|
|sim_exist|布尔值|SIM卡是否在位。<li>true：在位。</li><li>false：不在位。</li>|
|state_data|布尔值|移动数据的开关状态。<li>true：打开。</li><li>false：关闭。</li>|
|state_lte|布尔值|无线网络的开关状态。<li>true：打开。</li><li>false：关闭。</li>|
|network_signal_level|数字|信号的强度。<br>取值范围：0~5级。默认值为None，取值为None时代表移动数据开关处于关闭状态。取值为0时，代表网络断开，这时network_type的取值为null。</br>|
|network_type|字符串|网络状态。<br>取值范围：5G、4G、3G、2G。</br>当网络断开，取值为null。|
|ip_addr|字符串|无线网络拨号成功后，要显示的IP地址。|

### 查询无线网络APN接口资源信息<a name="ZH-CN_TOPIC_0000001577810592"></a>

**命令功能<a name="zh-cn_topic_0000001397081586_zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_section54694405"></a>**

查询无线网络APN接口资源信息。

**命令格式<a name="zh-cn_topic_0000001397081586_zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_section22487602"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/LTE/ConfigInfo**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001397081586_zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_section9556486"></a>**

接口已默认开发，功能根据无线网卡适配。

**使用实例<a name="zh-cn_topic_0000001397081586_zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_section18899510"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/LTE/ConfigInfo
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.type": "#MindXEdgeLTE.v1_0_0.MindXEdgeLTE",
    "@odata.context": "/redfish/v1/$metadata#Systems/LTE/ConfigInfo",
    "@odata.id": "/redfish/v1/Systems/LTE/ConfigInfo",
    "Id" : "LTE ConfigInfo",
    "Name": "LTE ConfigInfo",
    "apn_name": null,
    "apn_user": null,
    "auth_type": null,
    "mode_type": 1
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397081586_zh-cn_topic_0000001082765802_zh-cn_topic_0200634534_section35877865"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.type|字符串|无线网络APN接口资源类型。|
|@odata.context|字符串|无线网络APN接口资源模型的OData描述信息。|
|@odata.id|字符串|无线网络APN接口资源模型节点的访问路径。|
|Id|字符串|无线网络APN接口资源的唯一标识。|
|Name|字符串|无线网络APN接口资源的名称。|
|apn_name|字符串|无线网络拨号时使用的APN名称，允许为空。|
|apn_user|字符串|无线网络拨号时使用的APN用户名，允许为空。|
|auth_type|字符串|身份验证类型。取值为0、1、2、3，其含义分别为：<li>0：NONE</li><li>1：PAP</li><li>2：CHAP</li><li>3：PAP or CHAP</li>|
|mode_type|数字|无线网络模块类型。取值为0、1、2、3，其含义分别为：<li>0：华为 ME909S（暂不支持）</li><li>1：移远 EC25（暂不支持）</li><li>2：移远 EC200T</li><li>3：移远 RM500U</li>|

### 配置无线网络APN接口资源信息<a name="ZH-CN_TOPIC_0000001628729977"></a>

**命令功能<a name="zh-cn_topic_0000001447281845_zh-cn_topic_0000001082606160_zh-cn_topic_0208776520_zh-cn_topic_0200634535_section1246702"></a>**

配置无线网络APN接口资源信息。

**命令格式<a name="zh-cn_topic_0000001447281845_zh-cn_topic_0000001082606160_zh-cn_topic_0208776520_zh-cn_topic_0200634535_section11220318"></a>**

操作类型：**PATCH**

**URL：https://**_device\_ip_**/redfish/v1/Systems/LTE/ConfigInfo**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "apn_name": apnname,
    "apn_user": apnuser,
    "apn_passwd": apnpasswd,
    "auth_type":authtype
}
```

**请求参数<a name="zh-cn_topic_0000001447281845_section482109104611"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|apn_name|必选|无线网络拨号时使用的APN名称，不允许为空。|字符串，可由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（_-.@）组成，最大长度为39字符。|
|apn_user|可选|无线网络拨号时使用的APN用户名，允许为空。apn_user和apn_passwd字段必须同时为空或者同时不为空。请根据运营商提供的实际信息进行配置。|字符串。最大长度为64字符，可由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（-_.@）组成。|
|apn_passwd|可选|无线网络拨号时使用的APN密码，允许为空。apn_user和apn_passwd字段必须同时为空或者同时不为空。请根据运营商提供的实际信息进行配置。|字符串。长度为1~64个字符，由数字、大小写字母及英文特殊符号~`!?, .:;-_'"(){}[]/<>@#^&$%*+\|\\=空格组成。|
|auth_type|必选|身份验证类型。|字符串。取值为0、1、2、3，其含义分别为：<li>0：NONE</li><li>1：PAP</li><li>2：CHAP</li><li>3：PAP or CHAP</li> [!NOTE] 说明<br>4G模块支持的auth_type取值为0、1、2；5G模块支持的auth_type的取值为0、1、2、3。</br>|

**使用指南<a name="zh-cn_topic_0000001447281845_zh-cn_topic_0000001082606160_zh-cn_topic_0208776520_zh-cn_topic_0200634535_section36430550"></a>**

接口已默认开发，功能根据无线网卡适配。

**使用实例<a name="zh-cn_topic_0000001447281845_zh-cn_topic_0000001082606160_zh-cn_topic_0208776520_zh-cn_topic_0200634535_section59439495"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/LTE/ConfigInfo
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "apn_name": "apnname",
    "apn_user": "apnuser",
    "apn_passwd": "apnpasswd",
    "auth_type": "1"
}
```

响应样例：

```json
{
    "error": {
    "code": "Base.1.0.Success",
    "message": "Operation success. See ExtendedInfo for more information.",
    "@Message.ExtendedInfo": [
        {
          "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
          "Description": "Indicates that no error has occurred.",
          "Message": "Config LTE APN successfully.",
          "Severity": "OK",
          "NumberOfArgs": null,
          "ParamTypes": null,
          "Resolution": "None"
        }
      ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001447281845_zh-cn_topic_0000001082606160_zh-cn_topic_0208776520_zh-cn_topic_0200634535_section65193414"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性。支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 查询以太网接口集合信息<a name="ZH-CN_TOPIC_0000001628729969"></a>

**命令功能<a name="zh-cn_topic_0000001446921685_zh-cn_topic_0000001082765790_zh-cn_topic_0178823243_section54694405"></a>**

查询以太网集合资源的信息。

**命令格式<a name="zh-cn_topic_0000001446921685_zh-cn_topic_0000001082765790_zh-cn_topic_0178823243_section22487602"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/EthernetInterfaces**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001446921685_zh-cn_topic_0000001082765790_zh-cn_topic_0178823243_section9556486"></a>**

无

**使用实例<a name="zh-cn_topic_0000001446921685_zh-cn_topic_0000001082765790_zh-cn_topic_0178823243_section18899510"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/EthernetInterfaces
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.type": "#EthernetInterfaceCollection.EthernetInterfaceCollection",
    "@odata.context": "/redfish/v1/$metadata#Systems/Members/EthernetInterfaces/$entity",
    "@odata.id": "/redfish/v1/Systems/EthernetInterfaces",
    "Name": "Ethernet Interface Collection",
    "Description": "System NICs on Servers",
    "Members@odata.count": 2,
    "Members": [
        {
            "@odata.id": "/redfish/v1/Systems/EthernetInterfaces/GMAC1"
        },
        {
            "@odata.id": "/redfish/v1/Systems/EthernetInterfaces/GMAC0"
        }
    ]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001446921685_zh-cn_topic_0000001082765790_zh-cn_topic_0178823243_section35877865"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|网口集合资源模型的OData描述信息。|
|@odata.id|字符串|网口集合资源的访问路径。|
|@odata.type|字符串|网口集合资源类型。|
|Name|字符串|网口集合资源的名称。|
|Description|字符串|网口描述信息。|
|Members@odata.count|数字|当前网口资源数量。|
|Members|数组|网口资源列表。|
|Members.@odata\.id|字符串|单个网口资源节点的访问路径。|

### 查询以太网接口资源信息<a name="ZH-CN_TOPIC_0000001628610537"></a>

**命令功能<a name="zh-cn_topic_0000001397081530_zh-cn_topic_0000001082765806_zh-cn_topic_0178823244_section65856196"></a>**

查询指定主机以太网接口资源的信息。

**命令格式<a name="zh-cn_topic_0000001397081530_zh-cn_topic_0000001082765806_zh-cn_topic_0178823244_section55834860"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/EthernetInterfaces/**_<eth\_Id_\>

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001397081530_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<eth_Id>|必选|网口资源的ID。|OS侧显示的网口名，取值为GMAC0或GMAC1。|

**使用指南<a name="zh-cn_topic_0000001397081530_zh-cn_topic_0000001082765806_zh-cn_topic_0178823244_section26329774"></a>**

为保障边缘管理系统的正常使用，避免出现卡顿或者上传、下载任务过慢的问题，网络带宽必须满足基本要求，推荐值如下：

- 边缘管理系统所在边缘设备与Web客户端之间的带宽 ≥ 20Mbit/s
- 其他网络要求：时延 < 30ms，丢包率 < 3%

**使用实例<a name="zh-cn_topic_0000001397081530_zh-cn_topic_0000001082765806_zh-cn_topic_0178823244_section35641380"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/EthernetInterfaces/GMAC0
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Members/EthernetInterfaces/Members/$entity",
    "@odata.id": "/redfish/v1/Systems/EthernetInterfaces/GMAC0",
    "@odata.type": "#EthernetInterface.v1_8_0.EthernetInterface",
    "Id": "GMAC0",
    "Name": "eth0",
    "Description": "Ethernet Interface over Wired Network Adapter",
    "PermanentMACAddress": "00:18:C0:xx:xx:03",
    "MACAddress": "00:18:C0:xx:xx:04",
    "InterfaceEnabled": true,
    "LinkStatus": "LinkUp",
    "IPv4Addresses": [{
        "Address": "xx.xx.xx.xx",
        "SubnetMask": "255.255.0.0",
        "Gateway": "xx.xx.xx.xx",
        "VlanId": "null",
        "AddressOrigin": "Static",
        "Tag": "Mgmt"
    }],
    "IPv6Addresses": [{
       "Address": "fe80::xxxx:xxxx:xxxx:6403",
       "PrefixLength": 64,
       "AddressOrigin": "LinkLocal",
       "AddressState": "Preferred"",
        "Gateway": null
    }],
    "IPv6DefaultGateway": "xxxx::xxxx",
    "NameServers": [
        "xxx.xxx.xxx"
    ],
    "Oem": {
        "WorkMode": "1000Mb/s"
        "AdapterType": "GMAC,
        "LteDataSwitchOn": true,
        "Connections": [],
        "Statistic": {
            "SendPackages": "123456",
            "RecvPackages": "123456",
            "ErrorPackages": "123456",
            "DropPackages": "123456"
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397081530_zh-cn_topic_0000001082765806_zh-cn_topic_0178823244_section52336965"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|指定网口资源模型的OData描述信息。|
|@odata.id|字符串|指定网口资源节点的访问路径。|
|@odata.type|字符串|指定网口资源类型。|
|Id|字符串|指定网口资源的唯一标识。<br>GMAC+数字，有线网口</br>|
|Name|字符串|指定网口资源的名称。|
|Description|字符串|指定网口的描述信息。|
|PermanentMACAddress|字符串|网口的永久MAC地址。|
|MACAddress|字符串|网口的当前MAC地址。|
|InterfaceEnabled|布尔值|网口是否使能。|
|LinkStatus|字符串|Link状态。<li>LinkUp</li><li>NoLink</li><li>LinkDown</li>|
|IPv4Addresses[]|数组|IPv4地址表。|
|IPv4Addresses[].Address|字符串|IPv4地址。|
|IPv4Addresses[].SubnetMask|字符串|子网掩码。|
|IPv4Addresses[].Gateway|字符串|网关。|
|IPv4Addresses[].VlanId|字符串|Vlan ID。|
|IPv4Addresses[].AddressOrigin|字符串|地址来源。<li>Static</li><li>DHCP</li>|
|IPv4Addresses[].Tag|字符串|IP地址用途标签。|
|IPv6Addresses[]|数组|IPv6地址表。|
|IPv6Addresses[].Address|字符串|IPv6地址。|
|IPv6Addresses[].PrefixLength|字符串|IPv6前缀长度。|
|IPv6Addresses[].AddressOrigin|字符串|地址来源。<li>Static</li><li>DHCPv6</li><li>LinkLocal</li><li>SLAAC</li>|
|IPv6Addresses[].AddressState|字符串|地址状态。<li>Preferred</li><li>Deprecated</li><li>Tentative</li><li>Failed</li>|
|IPv6Addresses[].Gateway|字符串|IPv6默认网关。|
|IPv6Addresses[].Tag|字符串|IP地址用途标签。|
|NameServers|数组|DNS字符串。|
|Oem|对象|自定义字段。|
|Oem.WorkMode|字符串|网口工作模式。<li>1000Mb/s</li><li>100Mb/s</li><li>10Mb/s</li>|
|Oem.AdapterType|字符串|指定网口的类型。取值为GMAC。|
|Oem.LteDataSwitchOn|布尔值|移动数据是否开关。|
|Oem.Connections[]|数组|可用连接列表。|
|Oem.Statistic[].SendPackages|字符串|发包。|
|Oem.Statistic[].RecvPackages|字符串|收包。|
|Oem.Statistic[].ErrorPackages|字符串|错包。|
|Oem.Statistic[].DropPackages|字符串|丢包。|

### 修改以太网接口资源信息<a name="ZH-CN_TOPIC_0000001628490573"></a>

**命令功能<a name="zh-cn_topic_0000001447121509_zh-cn_topic_0000001082477682_zh-cn_topic_0178823245_section65133568"></a>**

配置以太网接口，目前只支持IPv4的修改，同一网口的IP列表个数最多为4个。

**命令格式<a name="zh-cn_topic_0000001447121509_zh-cn_topic_0000001082477682_zh-cn_topic_0178823245_section49331203"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/EthernetInterfaces/**_<eth\_Id_\>

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "IPv4Addresses": [{
        "Address": Address,
        "SubnetMask": SubnetMask,
        "AddressOrigin": AddressOrigin,
        "VlanId": VlanId,
        "Gateway": Gateway,
        "Tag": Tag,
        "ConnectTest": ConnectTest,
        "RemoteTestIp": RemoteTestIp
    }]
}
```

**URL参数<a name="zh-cn_topic_0000001447121509_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<eth_Id>|必选|网口资源的ID。|OS侧显示的网口名，取值为GMAC0或GMAC1。|

**请求参数<a name="zh-cn_topic_0000001447121509_section6434183595615"></a>**

**表 2**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|IPv4Addresses|必选|IPv4地址信息。|列表类型，长度1~4，IPv4地址的其他字段信息。|
|Address|必选|IPv4地址|字符串，IPv4地址。|
|SubnetMask|必选|IPv4地址的子网掩码|字符串，IPv4地址的子网掩码。|
|AddressOrigin|必选|IPv4地址获取模式|字符串，可设置为“Static”。|
|VlanId|可选|虚拟局域网标识符|数字或者null，数字有效范围为1 ~ 4094。|
|Gateway|必选|IPv4网关地址|字符串或者null，IPv4网关地址。|
|Tag|必选|IPv4地址用途标签|字符串且非null，支持大小写字母（a~z、A~Z）、数字（0~9）、下划线（_），最大长度为32个字符。|
|ConnectTest|可选|是否对新增或修改的IP地址以及网关进行连通性测试|布尔值，取值为True或False。|
|RemoteTestIp|可选|对新增或修改的IP地址进行连通性测试的远端IP地址|字符串，IPv4地址。注意不能和Address参数的配置相同。<br> [!NOTE] 说明</br>如果要配置ConnectTest和RemoteTestIp，两个参数必须同时配置。只有当ConnectTest选择True时，此字段才有效，Web端新增IP时默认需要进行连通性测试。|

**使用指南<a name="zh-cn_topic_0000001447121509_zh-cn_topic_0000001082477682_zh-cn_topic_0178823245_section36404521"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447121509_zh-cn_topic_0000001082477682_zh-cn_topic_0178823245_section59205238"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/EthernetInterfaces/GMAC0
```

请求头：

```http
X-Auth-Token: auth_value 
Content-Type: application/json
```

请求消息体：

```json
{
    "IPv4Addresses":[{
        "Address":"xx.xx.xx.xx",
        "SubnetMask":"255.255.0.0",
        "AddressOrigin":"Static",
        "Gateway":"",
        "VlanId":null,
        "Tag":"net"
    },
    {
        "Address":"10.10.10.xx",
       "SubnetMask":"255.255.0.0",
       "Gateway":"10.10.10.xx",
       "VlanId":null,
       "Tag":"test",
       "ConnectTest":true,
       "RemoteTestIp":"xx.xx.xx.xx",
       "AddressOrigin":"Static"
    }]
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Members/1/EthernetInterfaces/Members/$entity",
    "@odata.type": "#EthernetInterface.v1_8_0.EthernetInterface",
    "@odata.id": "/redfish/v1/Systems/1/EthernetInterfaces/GMAC0",
    "Id": "GMAC0",
    "Name": "eth0",
    "IPv4Addresses": [{
            "Address": "xx.xx.xx.xx",
            "SubnetMask": "255.255.0.0",
            "AddressOrigin": "Static",
            "Gateway": "xx.xx.xx.xx",
            "VlanId": null,
            "Tag": "net"
        },
        {
            "Address": "xx.xx.xx.xx",
            "SubnetMask": "255.255.0.0",
            "AddressOrigin": "Static",
            "Gateway": "xx.xx.xx.xx",
            "VlanId": null,
            "Tag": "test"
        }],
    "Oem": {
        "StartTime": "2020-11-12T20:17:03+0000",
        "TaskState": "Running",
        "TaskPercentage": "ok"
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001447121509_zh-cn_topic_0000001082477682_zh-cn_topic_0178823245_section63085095"></a>**

**表 3**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|配置以太网任务资源模型的OData描述信息。|
|@odata.id|字符串|当前任务资源的访问路径。<br>[!NOTE] 说明</br>您可以访问该资源，获取该任务的详细信息。|
|@odata.type|字符串|配置以太网任务资源的类型。|
|Id|字符串|配置以太网任务资源的ID。|
|Name|字符串|配置以太网任务资源的名称。|
|IPv4Addresses|对象|配置以太网任务的IPv4地址表。|
|Address|字符串|配置以太网任务的IPv4地址。|
|SubnetMask|字符串|配置以太网任务的子网掩码。|
|AddressOrigin|字符串|配置以太网任务的地址来源。<li>Static</li><li>DHCP</li>|
|Gateway|字符串|配置以太网任务的网关。|
|VlanId|字符串|配置以太网任务的VLAN ID。|
|Tag|字符串|配置以太网任务的IP地址用途标签。|
|Oem|对象|自定义字段。|
|StartTime|字符串|配置以太网任务的起始时间。|
|TaskState|字符串|配置以太网任务资源的状态。<li>New</li><li>Starting</li><li>Running</li><li>Suspended</li><li>Interrupted</li><li>Pending</li><li>Stopping</li><li>Completed</li><li>Killed</li><li>Exception</li><li>Service</li>|
|TaskPercentage|字符串|配置以太网任务完成进度。|

### 获取网口与IP列表<a name="ZH-CN_TOPIC_0000001578489896"></a>

**命令功能<a name="zh-cn_topic_0000001447161473_zh-cn_topic_0000001082606168_section18331936111619"></a>**

获取用户配置的网口设备名称与IP对应关系列表。

**命令格式<a name="zh-cn_topic_0000001447161473_zh-cn_topic_0000001082606168_section1283523651620"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/EthIpList**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447161473_zh-cn_topic_0000001082606168_section384913671612"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447161473_zh-cn_topic_0000001082606168_section1784920369166"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/EthIpList
```

请求头：

```http
X-Auth-Token: auth_value
Content-Type: application/json
```

请求消息体：无

响应样例：

```json
{
  "@odata.context": "/redfish/v1/$metadata#Systems/EthIpList",
  "@odata.id": "/redfish/v1/Systems/EthIpList",
  "@odata.type": "#MindXEdgeEthIpList.v1_0_0.MindXEdgeEthIpList",
  "Id": "Ethernet Ip List",
  "Name": "Ethernet Ip List",
  "ip_addr_list": 
       {
            "eth0: 1": "xx.xx.xx.xx",
            "eth1": "xx.xx.xx.xx"
        },
  "noGatewayIP": ["xx.xx.xx.xx"]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447161473_zh-cn_topic_0000001082606168_section285373611162"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|查询网口与IP列表的OData描述信息。|
|@odata.id|字符串|查询网口与IP列表的访问路径。|
|@odata.type|字符串|查询网口与IP列表的资源类型。|
|Id|字符串|查询网口与IP列表的唯一标识。|
|Name|字符串|查询网口与IP列表的名称。|
|ip_addr_list|对象|用户配置的网络设备名称与IP对应关系列表。|
|noGatewayIP|列表|小站用于恢复出厂设置的IP地址与默认网关不匹配的集合。|

### 查询网管资源信息<a name="ZH-CN_TOPIC_0000001577530752"></a>

**命令功能<a name="zh-cn_topic_0000001425853180_section15992534585"></a>**

查询网管资源信息。

**命令格式<a name="zh-cn_topic_0000001425853180_section4841734165619"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1**/**NetManager**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001425853180_section7881163111714"></a>**

无

**使用实例<a name="zh-cn_topic_0000001425853180_section141635015617"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/NetManager
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

- 网管模式为Web模式

    ```json
    {
        "@odata.context": "/redfish/v1/$metadata#NetManager",
        "@odata.id": "/redfish/v1/NetManager",
        "@odata.type": "#MindXEdgeNetManager.v1_0_0.MindXEdgeNetManager",
        "Id": "NetManager",
        "Name": "NetManager",
        "NetManager": "Web",
        "NetIP": "",
        "Port": "",
        "NetAccount": "",
        "ServerName": "",
        "ConnectStatus": "not_configured",
        "NodeID": {
            "@odata.id": "/redfish/v1/NetManager/NodeID"
        },
        "QueryFdCert": {
            "@odata.id": "/redfish/v1/NetManager/QueryFdCert"
        },
        "Actions": {
            "#ImportFdCert": {
                "target": "/redfish/v1/NetManager/ImportFdCert"
            },
            "#ImportFdCrl": {
                "target": "/redfish/v1/NetManager/ImportFdCrl"
            }
        }
    }
    ```

- 网管模式为FusionDirector模式

    ```json
    {
        "@odata.context": "/redfish/v1/$metadata#NetManager",
        "@odata.id": "/redfish/v1/NetManager",
        "@odata.type": "#MindXEdgeNetManager.v1_0_0.MindXEdgeNetManager",
        "Id": "NetManager",
        "Name": "NetManager",
        "NetManager": "FusionDirector",
        "NetIP": "xx.xx.xx.xx",
        "Port": "443",
        "NetAccount": "2102312NNUN0L6000008",
        "ServerName": "fd.fusiondirector.huawei.com",
        "ConnectStatus": "connected",
        "NodeID": {
            "@odata.id": "/redfish/v1/NetManager/NodeID"
        },
        "QueryFdCert": {
            "@odata.id": "/redfish/v1/NetManager/QueryFdCert"
        },
        "Actions": {
            "#ImportFdCert": {
                "target": "/redfish/v1/NetManager/ImportFdCert"
            },
            "#ImportFdCrl": {
                "target": "/redfish/v1/NetManager/ImportFdCrl"
            }
        }
    }
    ```

**输出说明<a name="zh-cn_topic_0000001425853180_section047520175817"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|北向接口固定字段，当前资源模型的OData描述信息。|
|@odata.id|字符串|北向接口固定字段，当前资源节点的访问路径。|
|@odata.type|字符串|北向接口固定字段，当前资源类型。|
|Id|字符串|北向接口固定字段，当前资源的ID。|
|Name|字符串|北向接口固定字段，当前资源的名称。|
|NetManager|字符串|网管模式，分为以下几种：<li>Web：本地Web管理</li><li>FusionDirector：FusionDirector管理</li>|
|NetIP|字符串|对接IP地址，仅FusionDirector模式有效。|
|Port|字符串|对接端口号，仅FusionDirector模式有效。|
|NetAccount|字符串|对接账号，仅FusionDirector模式有效。|
|ServerName|字符串|服务器名称，仅FusionDirector模式有效。|
|ConnectStatus|字符串|对接状态，取值为：<li>not_configured：未配置</li><li>connecting：连接中</li><li>connected：已连接</li><li>error_configured：配置错误</li>|
|NodeID|对象|NodeID接口资源的访问路径。|
|QueryFdCert|对象|根证书接口资源的访问路径。|
|Actions|对象|可执行的操作。|
|Actions.#ImportFdCert|对象|导入根证书的访问路径。|
|Actions.#ImportFdCrl|对象|导入吊销列表的访问路径。|

### 查询网管节点ID<a name="ZH-CN_TOPIC_0000001628849953"></a>

**命令功能<a name="zh-cn_topic_0000001476129581_section15992534585"></a>**

查询网管节点ID。

**命令格式<a name="zh-cn_topic_0000001476129581_section4841734165619"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1**/**NetManager**/**NodeID**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001476129581_section7881163111714"></a>**

无

**使用实例<a name="zh-cn_topic_0000001476129581_section141635015617"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/NetManager/NodeID
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#NetManager/NodeID/$entity",
    "@odata.id": "/redfish/v1/NetManager/NodeID",
    "@odata.type": "#MindXEdgeNetManager.v1_0_0.MindXEdgeNetManager",
    "Id": "NodeID",
    "Name": "NodeID",
    "NodeConnectID": "c633ea21-48b1-4529-81d0-e5ccac0366ac"
}
```

**输出说明<a name="zh-cn_topic_0000001476129581_section047520175817"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|北向接口固定字段，当前资源模型的OData描述信息。|
|@odata.id|字符串|北向接口固定字段，当前资源节点的访问路径。|
|@odata.type|字符串|北向接口固定字段，当前资源类型。|
|Id|字符串|北向接口固定字段，当前资源的ID。|
|Name|字符串|北向接口固定字段，当前资源的名称。|
|NodeConnectID|字符串|节点ID。|

### 上传FusionDirector根证书<a name="ZH-CN_TOPIC_0000001577810620"></a>

**命令功能<a name="zh-cn_topic_0000001476209529_section15992534585"></a>**

上传FusionDirector根证书。

**命令格式<a name="zh-cn_topic_0000001476209529_section4841734165619"></a>**

操作类型：**POST**

**URL：https://**_device\_ip_**/redfish/v1**/**NetManager/ImportFdCert**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "imgfile": imgfile,
    "size": size
}
```

**请求参数<a name="zh-cn_topic_0000001476209529_section159421313404"></a>**

**表 1**  参数说明

|参数名|是否必选|参数说明|取值要求|
|--|--|--|--|
|imgfile|必选|传输文件名称。通过Form-Data传输。|上传文件的文件名需要满足长度为1~255个字符，由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（_.-）组成。允许的文件类型为crt。当KEY的值是imgfile时，对应VALUE的值选择对应文件。|
|size|可选|上传文件大小。通过Form-Data传输。|取值为数字，单位为字节。取值大小需要大于0，最大取值为1MB。|

**使用指南<a name="zh-cn_topic_0000001476209529_section7881163111714"></a>**

无

**使用实例<a name="zh-cn_topic_0000001476209529_section141635015617"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/NetManager/ImportFdCert
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "imgfile": rootCertChain.crt,
    "size": 10
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#NetManager/QueryFdCert",
    "@odata.id": "/redfish/v1/NetManager/QueryFdCert",
    "@odata.type": "#MindXEdgeNetManager.v1_0_0.MindXEdgeNetManager",
    "Id": "ImportFdCert",
    "Name": "ImportFdCert",
    "Issuer": "C=CN, ST=Guangdong, L=Shenzhen, O=Huawei, OU=Computing, CN=fusiondirectorCA",
    "Subject": "C=CN, ST=Guangdong, L=Shenzhen, O=Huawei, OU=Computing, CN=fusiondirectorCA",
    "Date": "2020-03-30 00:00:00--2030-12-31 00:00:00",
    "SerialNum": "65B64122AC4DE0B1",
    "Fingerprint": "XX:XX:XX:39:CD:A5:C4:8A:56:C4:1D:41:52:23:EC:97:CF:9E:88:78:F4:2D:4D:FB:9D:D1:1E:74:5F:XX:XX:XX"
}
```

**输出说明<a name="zh-cn_topic_0000001476209529_section047520175817"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|北向接口固定字段，当前资源模型的OData描述信息。|
|@odata.id|字符串|北向接口固定字段，当前资源节点的访问路径。|
|@odata.type|字符串|北向接口固定字段，当前资源类型。|
|Id|字符串|北向接口固定字段，当前资源的ID。|
|Name|字符串|北向接口固定字段，当前资源的名称。|
|Issuer|字符串|签发者。|
|Subject|字符串|拥有者。|
|Date|字符串|证书有效期。|
|SerialNum|字符串|序列号。|
|Fingerprint|字符串|证书指纹信息。|

### 上传FusionDirector吊销列表<a name="ZH-CN_TOPIC_0000001628730001"></a>

**命令功能<a name="zh-cn_topic_0000001461743524_section17183122111170"></a>**

上传FusionDirector吊销列表。

**命令格式<a name="zh-cn_topic_0000001461743524_section152210279507"></a>**

操作类型：**POST**

**URL：https://**_device\_ip_**/redfish/v1**/**NetManager/ImportFdCrl**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```http
{
    "imgfile": imgfile,
    "size": size
}
```

**请求参数<a name="zh-cn_topic_0000001461743524_section7247132415429"></a>**

**表 1**  参数说明

|参数名|是否必选|参数说明|取值要求|
|--|--|--|--|
|imgfile|必选|传输文件名称。通过Form-Data传输。|上传文件的文件名需要满足长度为1~255个字符，由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（_.-）组成。允许的文件类型为crl。当KEY的值是imgfile时，对应VALUE的值选择对应文件。|
|size|可选|上传文件大小。通过Form-Data传输。|取值为数字，单位为字节。取值大小需要大于0，最大取值为10KB。|

**使用指南<a name="zh-cn_topic_0000001461743524_section18366939145019"></a>**

无

**使用实例<a name="zh-cn_topic_0000001461743524_section1779216492508"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/NetManager/ImportFdCrl
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "imgfile": server.crl,
    "size": 1
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "import crl success.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

输出说明

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|消息详情。|
|@odata.type|字符串|会话资源类型。|
|Description|字符串|详细信息。|
|Message|字符串|返回消息。|
|Severity|字符串|严重性，Redfish支持的严重级别。<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|传递的参数数量。|
|ParamTypes|数组|传递的参数类型。|
|Resolution|字符串|解决方案描述。|

### 查询FusionDirector纳管根证书信息<a name="ZH-CN_TOPIC_0000001628729985"></a>

**命令功能<a name="zh-cn_topic_0000001475890097_section15992534585"></a>**

查询FusionDirector纳管根证书信息。

**命令格式<a name="zh-cn_topic_0000001475890097_section4841734165619"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1**/**NetManager/QueryFdCert**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001475890097_section7881163111714"></a>**

无

**使用实例<a name="zh-cn_topic_0000001475890097_section141635015617"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/NetManager/QueryFdCert
```

请求头：

```http
X-Auth-Token: auth_value
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#NetManager/QueryFdCert",
    "@odata.id": "/redfish/v1/NetManager/QueryFdCert",
    "@odata.type": "#MindXEdgeNetManager.v1_0_0.MindXEdgeNetManager",
    "Id": "QueryFdCert",
    "Name": "QueryFdCert",
    "Issuer": "C=CN, ST=Guangdong, L=Shenzhen, O=Huawei, OU=Computing, CN=fusiondirectorCA",
    "Subject": "C=CN, ST=Guangdong, L=Shenzhen, O=Huawei, OU=Computing, CN=fusiondirectorCA",
    "Date": "2020-03-30 00:00:00--2030-12-31 00:00:00",
    "SerialNum": "65B64122AC4DE0B1",
    "Fingerprint": "XX:XX:XX:39:CD:A5:C4:8A:56:C4:1D:41:52:23:EC:97:CF:9E:88:78:F4:2D:4D:FB:9D:D1:1E:74:5F:XX:XX:XX"
}
```

**输出说明<a name="zh-cn_topic_0000001475890097_section047520175817"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|北向接口固定字段，当前资源模型的OData描述信息。|
|@odata.id|字符串|北向接口固定字段，当前资源节点的访问路径。|
|@odata.type|字符串|北向接口固定字段，当前资源类型。|
|Id|字符串|北向接口固定字段，当前资源的ID。|
|Name|字符串|北向接口固定字段，当前资源的名称。|
|Issuer|字符串|签发者。|
|Subject|字符串|拥有者。|
|Date|字符串|证书有效期。|
|SerialNum|字符串|序列号。|
|Fingerprint|字符串|证书指纹信息。|

### 配置网管资源信息<a name="ZH-CN_TOPIC_0000001577810616"></a>

**命令功能<a name="zh-cn_topic_0000001426010812_section15992534585"></a>**

配置网管资源信息。

> [!NOTE] 说明 
> 该接口配置最长时间为4分钟左右，请耐心等待。

**命令格式<a name="zh-cn_topic_0000001426010812_section4841734165619"></a>**

操作类型：**POST**

**URL：https://**_device\_ip_**/redfish/v1**/**NetManager**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "ManagerType": manager_type,
    "NetIP": ip_address,
    "Port": port,
    "NetAccount": net_account,
    "NetPassword": net_password,
    "NodeId": node_id,
    "ServerName": server_name,
    "test": is_test
}
```

**请求参数<a name="zh-cn_topic_0000001426010812_section10867547333"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|ManagerType|必选|网管模式|字符串类型，取值为：<li>Web：本地Web管理</li><li>FusionDirector：FusionDirector管理</li>|
|NetIP|是否必选取决于ManagerType<li>ManagerType为Web时：可选</li><li>ManagerType为FusionDirector：必选</li>|对接IP地址|字符串类型，取值为IPv4地址。|
|Port|是否必选取决于ManagerType<li>ManagerType为Web时：可选</li><li>ManagerType为FusionDirector：必选</li>|对接端口号|数字类型，取值范围为1~65535，目前只支持443。|
|NetAccount|是否必选取决于ManagerType<li>ManagerType为Web时：可选</li><li>ManagerType为FusionDirector：必选</li>|对接账号|字符串类型，取值只能包含小写字母（a~z）、大写字母（A~Z）、数字（0~9）、中划线（-）、下划线（_），长度为1~256字节。|
|NetPassword|是否必选取决于ManagerType<li>ManagerType为Web时：可选</li><li>ManagerType为FusionDirector：必选</li>|对接密码|字符串类型，长度为8~32字符，遵循密码复杂度的规则。|
|NodeId|是否必选取决于ManagerType<li>ManagerType为Web时：可选</li><li>ManagerType为FusionDirector时：必选</li>|节点ID|字符串，当前仅支持一种格式：UUID，长度为36字节。如e6a47e30-3a09-11ea-9218-a8494df5f123。<br> [!NOTE] 说明</br>字符取值为数字（0\~9）、中划线（-）、小写字符（a\~f）有效。|
|ServerName|是否必选取决于ManagerType<li>ManagerType为Web时：可选</li><li>ManagerType为FusionDirector：可选</li>|服务器名称|FusionDirector模式：取值只能包含小写字母（a\~z）、大写字母（A\~Z）、数字（0\~9）、中划线（-）、英文点（.），长度范围为0~64字节。并且不能为/etc/hosts中127.0.0.1、::1对应的域名，包括但不仅限于localhost、localhost.localdomain、localhost4、localhost4.localdomain4、localhost6、localhost6.localdomain6。|
|test|是否必选取决于ManagerType<li>ManagerType为Web时：可选</li><li>ManagerType为FusionDirector：必选</li>|是否需要在网管生效前测试网管是否可用|布尔类型，取值为：<li>true：需要测试</li><li>false：不需要测试</li>|

**使用指南<a name="zh-cn_topic_0000001426010812_section7881163111714"></a>**

用户使用端口号时，如果曾多次配置端口号，以最近一次配置的端口号为准。

**使用实例<a name="zh-cn_topic_0000001426010812_section141635015617"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/NetManager
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

- 配置成Web模式：

    ```json
    {
        "ManagerType":"Web"
    }
    ```

- 配置成FusionDirector模式：

    ```json
    {
        "ManagerType": "FusionDirector",
        "NetIP": "xx.xx.xx.xx",
        "Port": 443,
        "NetAccount": "EdgeAccount",
        "NetPassword": "****",
        "NodeId": "e6a47e30-3a09-11ea-9218-a8494df5f123",
        "ServerName": "",
        "test": true
    }
    ```

响应样例：

- 网管模式为Web模式

    ```json
    {
        "@odata.context": "/redfish/v1/$metadata#NetManager",
        "@odata.id": "/redfish/v1/NetManager",
        "@odata.type": "#MindXEdgeNetManager.v1_0_0.MindXEdgeNetManager",
        "Id": "NetManager",
        "Name": "NetManager",
        "NetManager": "Web",
        "NetIP": "",
        "Port": "",
        "NetAccount": "",
        "ServerName": "",
        "ConnectStatus": "not_configured",
        "NodeID": {
            "@odata.id": "/redfish/v1/NetManager/NodeID"
        },
        "QueryFdCert": {
            "@odata.id": "/redfish/v1/NetManager/QueryFdCert"
        },
        "Actions": {
            "#ImportFdCert": {
                "target": "/redfish/v1/NetManager/ImportFdCert"
            },
            "#ImportFdCrl": {
                "target": "/redfish/v1/NetManager/ImportFdCrl"
            }
        }
    }
    ```

- 网管模式为FusionDirector模式

    ```json
    {
        "@odata.context": "/redfish/v1/$metadata#NetManager",
        "@odata.id": "/redfish/v1/NetManager",
        "@odata.type": "#MindXEdgeNetManager.v1_0_0.MindXEdgeNetManager",
        "Id": "NetManager",
        "Name": "NetManager",
        "NetManager": "FusionDirector",
        "NetIP": "xx.xx.xx.xx",
        "Port": "443",
        "NetAccount": "2102312NNUN0L6000008",
        "ServerName": "fd.fusiondirector.huawei.com",
        "ConnectStatus": "connected",
        "NodeID": {
            "@odata.id": "/redfish/v1/NetManager/NodeID"
        },
        "QueryFdCert": {
            "@odata.id": "/redfish/v1/NetManager/QueryFdCert"
        },
        "Actions": {
            "#ImportFdCert": {
                "target": "/redfish/v1/NetManager/ImportFdCert"
            },
            "#ImportFdCrl": {
                "target": "/redfish/v1/NetManager/ImportFdCrl"
            }
        }
    }
    ```

响应码：200

**输出说明<a name="zh-cn_topic_0000001426010812_section047520175817"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|北向接口固定字段，当前资源模型的OData描述信息。|
|@odata.id|字符串|北向接口固定字段，当前资源节点的访问路径。|
|@odata.type|字符串|北向接口固定字段，当前资源类型。|
|Id|字符串|北向接口固定字段，当前资源的ID。|
|Name|字符串|北向接口固定字段，当前资源的名称。|
|NetManager|字符串|网管模式，分为以下几种：<li>Web：本地Web管理</li><li>FusionDirector：FusionDirector管理</li>|
|NetIP|字符串|对接IP地址，仅FusionDirector模式有效。|
|Port|字符串|对接端口号，仅FusionDirector模式有效。|
|NetAccount|字符串|对接账号，仅FusionDirector模式有效。|
|ServerName|字符串|服务器名称，仅FusionDirector模式有效。|
|ConnectStatus|字符串|对接状态，取值为：<li>not_configured：未配置</li><li>connecting：连接中</li><li>connected：已连接</li><li>error_configured：配置错误</li>|
|NodeID|对象|NodeID接口资源的访问路径。|
|QueryFdCert|对象|根证书接口资源的访问路径。|
|Actions|对象|可执行的操作。|
|Actions.#ImportFdCert|对象|导入根证书的访问路径。|
|Actions.#ImportFdCrl|对象|导入吊销列表的访问路径。|

## 存储管理<a name="ZH-CN_TOPIC_0000001628610569"></a>

### 查询简单存储集合信息<a name="ZH-CN_TOPIC_0000001578489864"></a>

**命令功能<a name="zh-cn_topic_0000001447161521_zh-cn_topic_0000001082925744_zh-cn_topic_0178823246_section28334053"></a>**

查询当前存储集合资源信息。

**命令格式<a name="zh-cn_topic_0000001447161521_zh-cn_topic_0000001082925744_zh-cn_topic_0178823246_section53679891"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SimpleStorages**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447161521_zh-cn_topic_0000001082925744_zh-cn_topic_0178823246_section53103925"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447161521_zh-cn_topic_0000001082925744_zh-cn_topic_0178823246_section8173284"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/SimpleStorages
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Members/SimpleStorages/$entity",
    "@odata.id": "/redfish/v1/Systems/SimpleStorages",
    "@odata.type": "#SimpleStorageCollection.SimpleStorageCollection",
    "Name": "Simple Storage Collection",
    "Members@odata.count": 1,
    "Members": [{
        "@odata.id": "/redfish/v1/Systems/SimpleStorages/1"
    }]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447161521_zh-cn_topic_0000001082925744_zh-cn_topic_0178823246_section6450692"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|简单存储集合资源模型的OData描述信息。|
|@odata.id|字符串|简单存储集合资源的访问路径。|
|@odata.type|字符串|简单存储集合资源的类型。|
|Name|字符串|简单存储集合资源的名称。|
|Members@odata.count|数字|当前简单存储资源数量。|
|Members[]|数组|存储控制器资源列表。|
|Members[]@odata.id|字符串|单个存储控制器资源节点的访问路径。|

### 查询简单存储资源信息<a name="ZH-CN_TOPIC_0000001628849917"></a>

**命令功能<a name="zh-cn_topic_0000001447281853_zh-cn_topic_0000001082606156_zh-cn_topic_0178823247_section37034591"></a>**

查询指定存储资源信息。

**命令格式<a name="zh-cn_topic_0000001447281853_zh-cn_topic_0000001082606156_zh-cn_topic_0178823247_section64875869"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SimpleStorages/**_<storage\_id_\>

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001447281853_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|*<storage_id*>|必选|指定存储资源的ID。|数字，通过存储集合资源获得。<li>1：disk</li><li>2：u-disk</li><li>3：eMMC，M.2通用系统不支持查询eMMC</li>|

**使用指南<a name="zh-cn_topic_0000001447281853_zh-cn_topic_0000001082606156_zh-cn_topic_0178823247_section20454002"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447281853_zh-cn_topic_0000001082606156_zh-cn_topic_0178823247_section49868291"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/SimpleStorages/3
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    @odata.context: "/redfish/v1/$metadata#Systems/Members/SimpleStorages/Members/$entity",
    @odata.id: "/redfish/v1/Systems/SimpleStorages/3",
    @odata.type: "#SimpleStorage.v1_3_1.SimpleStorage",
    Id: "3",
    Name: "eMMC",
    Description: "System eMMC Flash",
    Status: {
        State: "Enabled",
        Health: "OK"
    },
    "Devices": [{
        "Name": "/dev/mmcblk0",
        "Manufacturer": "Hynix",
        "Model": "HBG4a2",
        "CapacityBytes": "31268536320",
        "LeftBytes": "588251136",
        "PartitionStyle": "GPT",
        "Location": "eMMC1",
        "DeviceLifeTimeUsed": 2
        "Status": {
            "State": "Enabled",
            "Health": "OK"
        }
    }]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447281853_zh-cn_topic_0000001082606156_zh-cn_topic_0178823247_section46161443"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|指定简单存储资源模型的OData描述信息。|
|@odata.id|字符串|指定简单存储资源的访问路径。|
|@odata.type|字符串|指定简单存储资源的类型。|
|Id|字符串|指定简单存储资源的ID。|
|Name|字符串|指定简单存储资源的名称。|
|Description|字符串|指定简单存储资源的描述。|
|Status|对象|指定简单存储控制器的状态，包括：<li>State：存储控制器使能状态</li><li>Health：存储控制器健康状态</li>|
|Devices[]|数组|下挂设备列表。|
|Devices [].Name|字符串|下挂设备的名称。|
|Devices [].Manufacturer|字符串|下挂设备的厂商。|
|Devices [].Model|字符串|下挂设备的型号。|
|Devices [].CapacityBytes|字符串|下挂设备的容量。|
|Devices [].LeftBytes|字符串|下挂设备的剩余容量。|
|Devices[].PartitionStyle|字符串|分区格式：<li>GPT</li><li>MBR</li>|
|Devices[].Location|字符串|设备的位置信息。|
|Devices[].DeviceLifeTimeUsed|数字|设备的使用寿命。取值如下：<li>0：未定义值</li><li>1：设备使用时间为0％-10％</li><li>2：设备使用时间为10％-20％</li><li>3：设备使用时间为20％-30％</li><li>4：设备使用时间为30％-40％</li><li>5：设备使用时间为40％-50％</li><li>6：设备使用时间为50％-60％</li><li>7：设备使用时间为60％-70％</li><li>8：设备使用时间为70％-80％</li><li>9：设备使用时间为80％-90％</li><li>10：设备使用时间为90％-100％</li><li>11：超过了其最大的预计设备使用寿命</li><li>others：预留</li>|
|Devices [].Status|对象|下挂设备的状态，包括：<li>State：存储控制器使能状态</li><li>Health：存储控制器健康状态</li>|

### 查询磁盘分区集合信息<a name="ZH-CN_TOPIC_0000001628490561"></a>

**命令功能<a name="zh-cn_topic_0000001447161529_zh-cn_topic_0000001082765792_zh-cn_topic_0178823248_section50659272"></a>**

查询当前磁盘分区集合资源信息。

**命令格式<a name="zh-cn_topic_0000001447161529_zh-cn_topic_0000001082765792_zh-cn_topic_0178823248_section53280267"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Partitions**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447161529_zh-cn_topic_0000001082765792_zh-cn_topic_0178823248_section20734340"></a>**

接口已默认开发，功能根据外置存储配置适配。

**使用实例<a name="zh-cn_topic_0000001447161529_zh-cn_topic_0000001082765792_zh-cn_topic_0178823248_section52391332"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Partitions
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{

    "@odata.context": "/redfish/v1/$metadata#Systems/Partitions/$entity",
    "@odata.id": "/redfish/v1/Systems/Partitions",
    "@odata.type": "#MindXEdgePartitionCollection.MindXEdgePartitionCollection",
    "Name": "Partition Collection",
    "Members@odata.count": 3,
    "Members": [{
        "@odata.id": "/redfish/v1/Systems/Partitions/hdisk0p1"
    },
    {
        "@odata.id": "/redfish/v1/Systems/Partitions/hdisk0p2"
    },
    {
        "@odata.id": "/redfish/v1/Systems/Partitions/hdisk0p3"
    }],
    "Mount":{
        "@odata.id": "/redfish/v1/Systems/Partitions/Mount"
    },
    "Unmount":{
        "@odata.id": "/redfish/v1/Systems/Partitions/Unmount"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447161529_zh-cn_topic_0000001082765792_zh-cn_topic_0178823248_section1759941"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|磁盘分区集合资源模型的OData描述信息。|
|@odata.id|字符串|磁盘分区集合资源的访问路径。|
|@odata.type|字符串|磁盘分区集合资源的类型。|
|Name|字符串|磁盘分区集合资源的名称。|
|Members@odata.count|数字|当前磁盘分区资源数量。|
|Members|对象|磁盘分区资源列表。|
|Members@odata\.id|字符串|单个磁盘分区资源节点的访问路径。|
|Mount|对象|挂载磁盘分区对象。|
|Mount@odata\.id|字符串|挂载磁盘分区资源节点的访问路径。|
|Unmount|对象|解挂磁盘分区对象。|
|Unmount@odata\.id|字符串|解挂磁盘分区资源节点的访问路径。|

### 查询磁盘分区资源信息<a name="ZH-CN_TOPIC_0000001577530760"></a>

**命令功能<a name="zh-cn_topic_0000001397241534_zh-cn_topic_0000001082765794_zh-cn_topic_0178823249_section43101492"></a>**

查询指定磁盘分区资源信息。

**命令格式<a name="zh-cn_topic_0000001397241534_zh-cn_topic_0000001082765794_zh-cn_topic_0178823249_section52369113"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Partitions/**_<partition\_id_\>

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001397241534_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<partition_id>|必选|磁盘分区资源的ID。|字符串，可通过查询分区集合信息获取。长度为1~128字符，可由大小写字母（a~z、A~Z）、数字（0~9）和下划线（_）组成且不含“..”。|

**使用指南<a name="zh-cn_topic_0000001397241534_zh-cn_topic_0000001082765794_zh-cn_topic_0178823249_section14039750"></a>**

接口已默认开发，功能根据外置存储配置适配

**使用实例<a name="zh-cn_topic_0000001397241534_zh-cn_topic_0000001082765794_zh-cn_topic_0178823249_section59248888"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Partitions/mdisk0p8
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Partitions/Members/$entity",
    "@odata.id": "/redfish/v1/Systems/Partitions/mdisk0p8",
    "@odata.type": "#MindXEdgePartition.v1_0_0.MindXEdgePartition",
    "Id": "mdisk0p8",
    "Name": "mdisk0p8",
    "CapacityBytes": "7516192768",
    "FreeBytes": "6905237504",
    "Links": [
        {
            "Device": "/redfish/v1/Systems/SimpleStorages/SATA",
            "DeviceName": "/dev/mdisk0",
            "Location": "PCIE3-0"
        }
    ],
    "MountPath": "/var/lib/docker",
    "Primary": false,
    "FileSystem": "ext4",
    "Status": {
        "State": "Enabled",
        "Health": "OK"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397241534_zh-cn_topic_0000001082765794_zh-cn_topic_0178823249_section63477947"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|分区资源模型的OData描述信息。|
|@odata.id|字符串|分区资源的访问路径。|
|@odata.type|字符串|分区资源的类型。|
|Id|字符串|分区资源的ID。|
|Name|字符串|分区的名称。|
|CapacityBytes|字符串|分区的总容量。|
|FreeBytes|字符串|分区的剩余空闲容量。|
|Links|对象|分区的链接信息。|
|Links.Device|字符串|分区对应的设备信息。|
|Links.DeviceName|字符串|分区对应的设备名称。|
|Links.Location|字符串|设备的位置信息。|
|Primary|布尔值|是否主分区（系统分区）。|
|FileSystem|字符串|文件系统格式。|
|MountPath|字符串|分区挂载路径。|
|Status|对象|逻辑盘的状态，包括：<li>Health：逻辑盘的健康状态</li><li>State：逻辑盘的使能状态</li>|

### 创建磁盘分区<a name="ZH-CN_TOPIC_0000001628490545"></a>

**命令功能<a name="zh-cn_topic_0000001447121473_zh-cn_topic_0000001082925748_zh-cn_topic_0178823250_section23909343"></a>**

创建磁盘分区。

**命令格式<a name="zh-cn_topic_0000001447121473_zh-cn_topic_0000001082925748_zh-cn_topic_0178823250_section13857498"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Partitions**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Number": number,
    "CapacityBytes": capacity,
    "Links": [{
        "Device": {
            "@odata.id": device
        }
    }],
    "FileSystem": filesystem
}
```

**请求参数<a name="zh-cn_topic_0000001447121473_section10330217734"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|Number|必选|分区个数，必选参数。|数字类型，单磁盘最大分区个数为16个（包含系统分区）。|
|CapacityBytes|必选|容量大小 <br>[!NOTE] 说明</br>磁盘分区创建中，空间大小采用2M向上对齐，当分区的容量不是2M的倍数时向上对齐。|字符串类型，取值为数字，单位GB，最小值0.5，且最多一位小数，必须是0.5的整数倍。|
|Links|必选|对象|分区的链接信息，只能包含1条元素。|
|Links.Device|必选|对象|分区对应的设备信息，只能包含@odata.id。|
|Links.Device.@odata\.id|必选|设备路径|设备资源路径，字符串，长度最大为256字符，可由大小写字母（a~z、A~Z）、数字（0~9）和其他字符（_-）组成且不含“..”，以/dev/开头。|
|FileSystem|必选|文件系统|取值为ext4。|

**使用指南<a name="zh-cn_topic_0000001447121473_zh-cn_topic_0000001082925748_zh-cn_topic_0178823250_section48715557"></a>**

接口已默认开发，功能根据外置存储配置适配。

**使用实例<a name="zh-cn_topic_0000001447121473_zh-cn_topic_0000001082925748_zh-cn_topic_0178823250_section35786830"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/Partitions
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Number": 1,
    "CapacityBytes": "0.5",
    "Links": [{
        "Device": {
            "@odata.id": "/dev/mdisk0"
        }
    }],
    "FileSystem": "ext4"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Create system partition successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001447121473_zh-cn_topic_0000001082925748_zh-cn_topic_0178823250_section53646020"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性。支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 删除磁盘分区<a name="ZH-CN_TOPIC_0000001628490525"></a>

**命令功能<a name="zh-cn_topic_0000001447121481_zh-cn_topic_0000001129563535_zh-cn_topic_0178823251_section36263752"></a>**

删除指定的逻辑盘。

**命令格式<a name="zh-cn_topic_0000001447121481_zh-cn_topic_0000001129563535_zh-cn_topic_0178823251_section57938315"></a>**

操作类型：**DELETE**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Partitions/**_<partition\_id_\>

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001447121481_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<partition_id>|必选|磁盘分区资源的ID。|字符串，可通过查询磁盘分区集合信息获取。长度为1~128字符，可由大小写字母（a~z、A~Z）、数字（0~9）和下划线（_）组成且不含“..”。|

**使用指南<a name="zh-cn_topic_0000001447121481_zh-cn_topic_0000001129563535_zh-cn_topic_0178823251_section62491942"></a>**

接口已默认开发，功能根据外置存储配置适配。

**使用实例<a name="zh-cn_topic_0000001447121481_zh-cn_topic_0000001129563535_zh-cn_topic_0178823251_section25556567"></a>**

请求样例：

```http
DELETE https://10.10.10.10/redfish/v1/Systems/Partitions/mdisk0p10
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Delete partition [mdisk0p10] successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001447121481_zh-cn_topic_0000001129563535_zh-cn_topic_0178823251_section28682517"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性，支持的严重级别包括：<li>OK<li>Warning<li>Critical|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 挂载磁盘分区<a name="ZH-CN_TOPIC_0000001577810568"></a>

**命令功能<a name="zh-cn_topic_0000001396921722_zh-cn_topic_0000001129385267_zh-cn_topic_0178823252_section47066149"></a>**

挂载磁盘分区到指定目录。

**命令格式<a name="zh-cn_topic_0000001396921722_zh-cn_topic_0000001129385267_zh-cn_topic_0178823252_section20942157"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Partitions/Mount**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "MountPath": path
    "PartitionID": partition id
}
```

**请求参数<a name="zh-cn_topic_0000001396921722_section6679175616535"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|PartitionID|必选|分区资源的ID。|字符串，可通过查询磁盘分区集合信息获取。长度为1\~128字符，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和下划线（_）组成，且不含“..”。|
|MountPath|必选|挂载目录，必选参数。|字符串，表示分区挂载目录。长度最大为256，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（/_-）组成，且以/开头，且不含“..”。|

**使用指南<a name="zh-cn_topic_0000001396921722_zh-cn_topic_0000001129385267_zh-cn_topic_0178823252_section18593118"></a>**

接口已默认开发，功能根据外置存储配置适配。

**使用实例<a name="zh-cn_topic_0000001396921722_zh-cn_topic_0000001129385267_zh-cn_topic_0178823252_section33120337"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/Partitions/Mount
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "MountPath": "/opt/mount"
    "PartitionID": "mdisk0p11"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Mount partition [mdisk0p11] successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001396921722_zh-cn_topic_0000001129385267_zh-cn_topic_0178823252_section29647584"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性，支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 解挂磁盘分区<a name="ZH-CN_TOPIC_0000001578449936"></a>

**命令功能<a name="zh-cn_topic_0000001515509149_section196514549274"></a>**

解挂磁盘分区。

**命令格式<a name="section4671541529"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Partitions/Unmount**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "PartitionID": "partition id"
}
```

**请求参数<a name="zh-cn_topic_0000001515509149_section926682651518"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|PartitionID|必选|分区资源的ID|字符串，可通过查询磁盘分区集合信息获取。长度为1\~128字符，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和下划线（_）组成，且不含“..”。|

**使用指南<a name="zh-cn_topic_0000001515509149_section7979155802820"></a>**

接口已默认开发，功能根据外置存储配置适配。

**使用实例<a name="zh-cn_topic_0000001515509149_section1835341115292"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/Partitions/Unmount
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "PartitionID": "mdisk0p11"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Unmount partition [mdisk0p11] successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性，支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 查询NFS分区信息<a name="ZH-CN_TOPIC_0000001628490565"></a>

**命令功能<a name="zh-cn_topic_0000001446921657_zh-cn_topic_0000001129668057_zh-cn_topic_0200980029_section93401824121416"></a>**

查询当前已配置的NFS分区信息。

**命令格式<a name="zh-cn_topic_0000001446921657_zh-cn_topic_0000001129668057_zh-cn_topic_0200980029_section1734517243143"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/NfsManage**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001446921657_zh-cn_topic_0000001129668057_zh-cn_topic_0200980029_section13380162414147"></a>**

无

**使用实例<a name="zh-cn_topic_0000001446921657_zh-cn_topic_0000001129668057_zh-cn_topic_0200980029_section5381924121413"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/NfsManage
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Members/NfsManage",
    "@odata.type": "#MindXEdgeNfsManage.v1_0_0.MindXEdgeNfsManage",
    "@odata.id": "/redfish/v1/Systems/NfsManage",
    "Id": "1",
    "Name": "Nfs Manage",
    "nfsList": [{
        "ServerIP": "xx.xx.xx.xx",
        "ServerDir": "/home/huawei/nfstest",
        "MountPath": "/home/test2",
        "FileSystem": "nfs4",
        "Status": "ok",
        "CapacityBytes": 585995649024,
        "FreeBytes": 497078501376
    },
    {
        "ServerIP": "xx.xx.xx.xx",
        "ServerDir": "/home",
        "MountPath": "/home/test3",
        "FileSystem": "nfs4",
        "Status": "error",
        "CapacityBytes": "NA",
        "FreeBytes": "NA"
    }]
    "Actions": {
        "#NfsManage.Mount": {
            "target": "/redfish/v1/Systems/NfsManage/Actions/NfsManage.Mount"
        },
        "#NfsManage.Unmount": {
            "target": "/redfish/v1/Systems/NfsManage/Actions/NfsManage.Unmount"
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001446921657_zh-cn_topic_0000001129668057_zh-cn_topic_0200980029_section4402724121415"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|NFS分区信息的OData描述信息。|
|@odata.type|字符串|NFS分区信息的类型。|
|@odata.id|字符串|NFS分区信息的访问路径。|
|Id|字符串|NFS分区信息的ID。|
|Name|字符串|NFS分区信息的名称。|
|nfsList|数组|NFS分区信息列表。|
|ServerIP|字符串|NFS服务器IP地址。|
|ServerDir|字符串|NFS服务器共享路径。|
|MountPath|字符串|NFS分区本地挂载路径。|
|FileSystem|字符串|NFS版本信息。|
|Status|字符串|NFS分区健康状态。|
|CapacityBytes|数字|NFS分区总容量。|
|FreeBytes|数字|NFS分区剩余空闲容量。|
|Actions|对象|NFS资源可执行的操作。|
|#NfsManage.Mount|对象|挂载NFS资源。|
|target|字符串|挂载NFS资源的访问路径。|
|#NfsManage.Unmount|对象|解挂NFS资源。|
|target|字符串|解挂NFS资源的访问路径。|

### 挂载NFS分区<a name="ZH-CN_TOPIC_0000001578449960"></a>

**命令功能<a name="zh-cn_topic_0000001447121549_zh-cn_topic_0000001129822437_zh-cn_topic_0200980210_section1074482018158"></a>**

挂载NFS分区到指定目录，且NFS挂载最大目录个数为32。

**命令格式<a name="zh-cn_topic_0000001447121549_zh-cn_topic_0000001129822437_zh-cn_topic_0200980210_section374618200154"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/NfsManage**/**Actions/NfsManage.Mount**

请求头：

```json
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "ServerIP": serverIp,
    "ServerDir": serverPath,
    "FileSystem": version,
    "MountPath": mountPath
}
```

**请求参数<a name="zh-cn_topic_0000001447121549_section17944142592416"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|ServerIP|必选|NFS服务器IP地址|字符串，IPv4地址。|
|ServerDir|必选|NFS服务器共享的目录|字符串，长度为最大为256，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（/_-）组成，且以/开头，且不含“..”。|
|FileSystem|必选|NFS协议版本信息|字符串，取值为nfs4。|
|MountPath|必选|NFS分区本地挂载点路径|字符串，长度为最大为256，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（/_-）组成，且需要以/开头，且不含“..”。|

**使用指南<a name="zh-cn_topic_0000001447121549_zh-cn_topic_0000001129822437_zh-cn_topic_0200980210_section1282112017157"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447121549_zh-cn_topic_0000001129822437_zh-cn_topic_0200980210_section682232016153"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/NfsManage/Actions/NfsManage.Mount
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{

    "ServerIP": "192.168.2.108",
    "ServerDir": "/home",
    "FileSystem": "nfs4",
    "MountPath": "/opt/mount"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Mount NFS successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447121549_zh-cn_topic_0000001129822437_zh-cn_topic_0200980210_section31051180"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性，支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 解挂NFS分区<a name="ZH-CN_TOPIC_0000001577530744"></a>

**命令功能<a name="zh-cn_topic_0000001515515385_section25921002459"></a>**

解除挂载NFS分区。

**命令格式<a name="zh-cn_topic_0000001515515385_section461014168459"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/NfsManage**/**Actions/NfsManage.Unmount**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "ServerIP": serverIp,
    "ServerDir": serverPath,
    "FileSystem": version,
    "MountPath": mountPath
}
```

**请求参数<a name="zh-cn_topic_0000001515515385_section638424372816"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|ServerIP|必选|NFS服务器IP地址|字符串，IPv4地址。|
|ServerDir|必选|NFS服务器共享的目录|字符串，长度最大为256，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（/_-）组成，且以/开头，且不含“..”。|
|FileSystem|必选|NFS协议版本信息|字符串，取值为nfs4。|
|MountPath|必选|NFS分区本地挂载点路径|字符串，长度最大为256，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（/_-）组成，且需要以/开头，且不含“..”。|

**使用指南<a name="zh-cn_topic_0000001515515385_section109126114611"></a>**

无

**使用实例<a name="zh-cn_topic_0000001515515385_section55617151466"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/NfsManage/Actions/NfsManage.Unmount
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "ServerIP": "192.168.2.108",
    "ServerDir": "/home",
    "FileSystem": "nfs4",
    "MountPath": "/opt/mount"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Unmount NFS successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001515515385_section1356045244618"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性，支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

## 用户管理<a name="ZH-CN_TOPIC_0000001628490581"></a>

### 查询用户资源集合信息<a name="ZH-CN_TOPIC_0000001628849937"></a>

**命令功能<a name="zh-cn_topic_0000001454999733_zh-cn_topic_0000001129668043_section4248051"></a>**

查询用户资源的集合信息。

**命令格式<a name="zh-cn_topic_0000001454999733_zh-cn_topic_0000001129668043_section38232461"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/AccountService/Accounts**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001454999733_zh-cn_topic_0000001129668043_section9821607"></a>**

无

**使用实例<a name="zh-cn_topic_0000001454999733_zh-cn_topic_0000001129668043_section21285601"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/AccountService/Accounts
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
  "@odata.context": "/redfish/v1/$metadata#AccountService/Accounts/$entity",
  "@odata.id": "/redfish/v1/AccountService/Accounts",
  "@odata.type": "#ManagerAccountCollection.ManagerAccountCollection",
  "Name": "Accounts Collection",
  "Members@odata.count": 1,
  "Members": [
    {
      "@odata.id": "/redfish/v1/AccountService/Accounts/1"
    }
  ]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001454999733_zh-cn_topic_0000001129668043_section57352685"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|用户资源集合模型的OData描述信息。|
|@odata.type|字符串|用户资源集合类型。|
|@odata.id|字符串|用户资源集合的访问路径。|
|Name|字符串|用户资源集合的名称。|
|Members@odata.count|数字|当前用户数量。|
|Members|数组|用户资源列表。|
|@odata.id|字符串|单个用户资源节点的访问路径。|

### 查询用户服务信息<a name="ZH-CN_TOPIC_0000001577810604"></a>

**命令功能<a name="zh-cn_topic_0000001396761798_zh-cn_topic_0000001129668043_section4248051"></a>**

查询用户服务的信息。

**命令格式<a name="zh-cn_topic_0000001396761798_zh-cn_topic_0000001129668043_section38232461"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/AccountService**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001396761798_zh-cn_topic_0000001129668043_section9821607"></a>**

无

**使用实例<a name="zh-cn_topic_0000001396761798_zh-cn_topic_0000001129668043_section21285601"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/AccountService 
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#AccountService",
    "@odata.id": "/redfish/v1/AccountService",
    "@odata.type": "#AccountService.v1_11_0.AccountService",
    "Id": "AccountService",
    "Name": "Account Service",
    "PasswordExpirationDays": 90,
    "Accounts": {
        "@odata.id": "/redfish/v1/AccountService/Accounts"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001396761798_zh-cn_topic_0000001129668043_section57352685"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|AccountService资源模型的OData描述信息。|
|@odata.id|字符串|AccountService资源的访问路径。|
|@odata.type|字符串|AccountService资源类型。|
|Id|字符串|AccountService资源的ID。|
|Name|字符串|AccountService资源的名称。|
|PasswordExpirationDays|数字|密码有效期。<br>默认90天，取值范围为0~365，0表示永不过期。</br>|
|Accounts|对象|Accounts接口资源的访问路径。|

### 修改用户服务信息<a name="ZH-CN_TOPIC_0000001628490549"></a>

**命令功能<a name="zh-cn_topic_0000001447121525_zh-cn_topic_0000001082606164_section4248051"></a>**

修改用户服务信息。

**命令格式<a name="zh-cn_topic_0000001447121525_zh-cn_topic_0000001082606164_section38232461"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/AccountService**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "PasswordExpirationDays": PasswordExpirationDays,
    "Password": password
}
```

**请求参数<a name="zh-cn_topic_0000001447121525_zh-cn_topic_0000001082606164_section9821607"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|PasswordExpirationDays|必选|密码有效期|数字类型，默认为90天，取值范围为0~365，0表示永不过期。|
|Password|必选|当前用户密码|字符串，长度为8~20个字符。|

**使用指南<a name="zh-cn_topic_0000001447121525_section071813591483"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447121525_zh-cn_topic_0000001082606164_section21285601"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/AccountService
```

请求头：

```http
X-Auth-Token: auth_value
Content-Type: application/json 
```

请求消息体：

```json
{
    "PasswordExpirationDays":100,
    "Password": "password"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#AccountService",
    "@odata.id": "/redfish/v1/AccountService",
    "@odata.type": "#AccountService.v1_11_0.AccountService",
    "Id": "AccountService",
    "Name": "Account Service",
    "PasswordExpirationDays": 100,
    "Accounts": {
        "@odata.id": "/redfish/v1/AccountService/Accounts"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447121525_zh-cn_topic_0000001082606164_section57352685"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|AccountService资源模型的OData描述信息。|
|@odata.id|字符串|AccountService资源的访问路径。|
|@odata.type|字符串|AccountService资源类型。|
|Id|字符串|AccountService资源的Id。|
|Name|字符串|AccountService资源的名称。|
|PasswordExpirationDays|数字|密码有效期。<br>默认为90天，取值范围为0~365，0表示永不过期。</br>|
|Accounts|对象|Accounts接口资源的访问路径。|

### 查询指定用户资源信息<a name="ZH-CN_TOPIC_0000001577810600"></a>

**命令功能<a name="zh-cn_topic_0000001397241542_zh-cn_topic_0000001082477674_zh-cn_topic_0178823273_section20465753"></a>**

查询指定用户资源信息。

**命令格式<a name="zh-cn_topic_0000001397241542_zh-cn_topic_0000001082477674_zh-cn_topic_0178823273_section49974054"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/AccountService/Accounts/<**_member\_id_\>

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001397241542_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<member_id>|必选|待查询的用户ID。|用户ID，数字，长度为1~16字符。|

**使用指南<a name="zh-cn_topic_0000001397241542_zh-cn_topic_0000001082477674_zh-cn_topic_0178823273_section21366574"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397241542_zh-cn_topic_0000001082477674_zh-cn_topic_0178823273_section58081443"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/AccountService/Accounts/1
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#AccountService/Accounts/Members/$entity",
    "@odata.id": "/redfish/v1/AccountService/Accounts/1",
    "@odata.type": "#ManagerAccount.v1_3_4.ManagerAccount",
    "Id": "1",
    "Name": "User Account",
    "Oem": {
        "LastLoginSuccessTime": "2022-11-16 20:54:03",
        "LastLoginFailureTime": "2022-11-16 06:15:29",
        "AccountInsecurePrompt": false,
        "ConfigNavigatorPrompt": true,
        "PasswordValidDays": "--",
        "PwordWrongTimes": 0,
        "LastLoginIP": "127.0.xx.xx"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397241542_zh-cn_topic_0000001082477674_zh-cn_topic_0178823273_section52970947"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|指定用户资源模型的OData描述信息。|
|@odata.id|字符串|指定用户资源的访问路径。|
|@odata.type|字符串|指定用户资源类型。|
|Id|字符串|指定用户资源的ID。|
|Name|字符串|指定用户资源的名称。|
|Oem|对象|自定义属性。|
|LastLoginSuccessTime|字符串|上次成功登录的时间。为空时表示首次登录。|
|LastLoginFailureTime|字符串|上次登录失败的时间。为空时表示没有失败过。|
|AccountInsecurePrompt|布尔值|账号不安全提示。<li>true：密码为默认密码。</li><li>false：密码不是默认密码。</li> [!NOTE] 说明<br>默认密码时仅允许登录并修改密码，其余接口均不可访问。</br>|
|ConfigNavigatorPrompt|布尔值|是否需要提示进入快速配置向导，默认为true。浏览器完成配置向导时，将该字段修改为false。|
|PasswordValidDays|字符串|值为数字，表示密码有效期剩余天数。“--”表示有效期无限。|
|PwordWrongTimes|数字|登录成功前的失败次数。取值为0、1、2、3、4、5。|
|LastLoginIP|字符串|上次登录的IP地址。|

### 修改指定用户信息<a name="ZH-CN_TOPIC_0000001578489884"></a>

**命令功能<a name="zh-cn_topic_0000001397081538_zh-cn_topic_0000001082477668_zh-cn_topic_0178823274_section4248051"></a>**

当前仅支持修改指定用户的密码信息。

**命令格式<a name="zh-cn_topic_0000001397081538_zh-cn_topic_0000001082477668_zh-cn_topic_0178823274_section38232461"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/AccountService/Accounts/**_<member\_id_\>

请求头：

```http
X-Auth-Token: auth_value
Content-Type: application/json
```

请求消息体：

```json
{
    "UserName": "admin",
    "old_password": "old_password",
    "Password":"new_password",
    "new_password_second": "new_password_second"
}
```

**URL参数<a name="zh-cn_topic_0000001397081538_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|<member_id>|必选|待查询的用户ID。|用户ID，数字，长度为1~16字符。|

**请求参数<a name="zh-cn_topic_0000001397081538_section1866192715343"></a>**

**表 2**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|UserName|必选|需要修改密码的用户名。|字符串，长度为1~16个字符，支持数字和英文字母，不能全为数字。|
|old_password|必选|用户需要修改的密码。|长度为8~20的字符串。默认开启密码复杂度检查功能，设置和修改的密码必须遵循密码复杂度的规则。|
|Password|必选|修改后的用户密码。|长度为8~20的字符串。默认开启密码复杂度检查功能，设置和修改的密码必须遵循密码复杂度的规则。|
|new_password_second|必选|再次输入修改后的密码。|长度为8~20的字符串。默认开启密码复杂度检查功能，设置和修改的密码必须遵循密码复杂度的规则。|

**使用指南<a name="zh-cn_topic_0000001397081538_zh-cn_topic_0000001082477668_zh-cn_topic_0178823274_section9821607"></a>**

针对指定用户，仅支持修改请求消息体中的密码属性。

**使用实例<a name="zh-cn_topic_0000001397081538_zh-cn_topic_0000001082477668_zh-cn_topic_0178823274_section21285601"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/AccountService/Accounts/1
```

请求头：

```http
X-Auth-Token: auth_value
Content-Type: application/json
```

请求消息体：

```json
{
    "UserName": "test",
    "old_password": "old_password",
    "Password":"password",
    "new_password_second":"password"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#AccountService/Accounts/Members/$entity",
    "@odata.id": "/redfish/v1/AccountService/Accounts/1",
    "@odata.type": "#ManagerAccount.v1_3_4.ManagerAccount",
    "Id": "1",
    "Name": "User Account",
    "Oem": {
        "LastLoginSuccessTime": "2022-11-16 20:54:03",
        "LastLoginFailureTime": "2022-11-16 06:15:29",
        "AccountInsecurePrompt": false,
        "ConfigNavigatorPrompt": true,
        "PasswordValidDays": "--",
        "PwordWrongTimes": 0,
        "LastLoginIP": "127.0.xx.xx"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397081538_zh-cn_topic_0000001082477668_zh-cn_topic_0178823274_section57352685"></a>**

**表 3**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|指定用户资源模型的OData描述信息。|
|@odata.id|字符串|指定用户资源的访问路径。|
|@odata.type|字符串|指定用户资源类型。|
|Id|字符串|指定用户资源的ID。|
|Name|字符串|指定用户资源的名称。|
|Oem|对象|自定义属性。|
|LastLoginSuccessTime|字符串|上次成功登录的时间。为空时表示首次登录。|
|LastLoginFailureTime|字符串|上次登录失败的时间。为空时表示没有失败过。|
|AccountInsecurePrompt|布尔值|账号不安全提示。<li>true密码为默认密码。</li><li>false：密码不是默认密码。</li>|
|ConfigNavigatorPrompt|布尔值|是否需要提示进入快速配置向导，默认为"true"。浏览器完成配置向导时，将该字段修改为"false"。|
|PasswordValidDays|字符串|取值为数字，表示密码有效期剩余天数。“--”表示有效期无限。|
|PwordWrongTimes|数字|登录成功前的失败次数。取值为0、1、2、3、4、5。|
|LastLoginIP|字符串|上次登录的IP地址。|

### 查询会话服务信息<a name="ZH-CN_TOPIC_0000001577530748"></a>

**命令功能<a name="zh-cn_topic_0000001397081578_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section2794612"></a>**

查询当前会话服务的信息。

**命令格式<a name="zh-cn_topic_0000001397081578_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section25151514"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/SessionService**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001397081578_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section24006753"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397081578_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section14734188"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/SessionService
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#SessionService",
    "@odata.id": "/redfish/v1/SessionService",
    "@odata.type": "#SessionService.v1_1_8.SessionService",
    "Id": "SessionService",
    "Name": "Session Service",
    "SessionTimeout": 100,
    "Sessions": {
        "@odata.id": "/redfish/v1/SessionService/Sessions"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397081578_zh-cn_topic_0000001129563537_zh-cn_topic_0178823223_section65498832"></a>**

**表 1**  SessionService资源信息

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|当前服务资源模型的OData描述信息。|
|@odata.id|字符串|当前服务资源节点的访问路径。|
|@odata.type|字符串|当前服务资源类型。|
|Id|字符串|当前服务资源的ID。|
|Name|字符串|当前服务资源的名称。|
|SessionTimeout|数字|Redfish会话超时时长，取值范围：5~120，单位为分钟。|
|Sessions|对象|会话列表。|
|@odata.id|字符串|会话列表的访问路径。|

### 修改会话服务信息<a name="ZH-CN_TOPIC_0000001578449932"></a>

**命令功能<a name="zh-cn_topic_0000001447161537_zh-cn_topic_0000001082477670_section2794612"></a>**

修改当前会话服务的信息。

**命令格式<a name="zh-cn_topic_0000001447161537_zh-cn_topic_0000001082477670_section25151514"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/SessionService**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "SessionTimeout": sessiontimeout,
    "Password": password
}
```

**请求参数<a name="zh-cn_topic_0000001447161537_section527481765616"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|SessionTimeout|必选|Redfish会话超时时长|数字，取值范围为5~120，单位为分钟。|
|Password|必选|当前用户密码|字符串，长度为8~20个字符。默认开启密码复杂度检查功能，设置和修改的密码必须遵循密码复杂度的规则。|

**使用指南<a name="zh-cn_topic_0000001447161537_zh-cn_topic_0000001082477670_section24006753"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447161537_zh-cn_topic_0000001082477670_section14734188"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/SessionService
```

请求头：

```http
X-Auth-Token: auth_value 
Content-Type: application/json
```

请求消息体：

```json
{
    "SessionTimeout":16,
    "Password":"password"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#SessionService",
    "@odata.id": "/redfish/v1/SessionService",
    "@odata.type": "#SessionService.v1_1_8.SessionService",
    "Id": "SessionService",
    "Name": "Session Service",
    "SessionTimeout": 16,
    "Sessions": {
        "@odata.id": "/redfish/v1/SessionService/Sessions"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447161537_zh-cn_topic_0000001082477670_section65498832"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|当前服务资源模型的OData描述信息。|
|@odata.id|字符串|当前服务资源节点的访问路径。|
|@odata.type|字符串|当前服务资源类型。|
|Id|字符串|当前服务资源的ID。|
|Name|字符串|当前服务资源的名称。|
|SessionTimeout|数字|Redfish会话超时时长。|
|Sessions|对象|会话列表。|
|@odata.id|字符串|会话列表的访问路径。|

### 创建会话<a name="ZH-CN_TOPIC_0000001578489868"></a>

**命令功能<a name="zh-cn_topic_0000001446921641_zh-cn_topic_0000001129385271_zh-cn_topic_0178823224_section32820427"></a>**

创建新会话。

**命令格式<a name="zh-cn_topic_0000001446921641_zh-cn_topic_0000001129385271_zh-cn_topic_0178823224_section26948387"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/SessionService/Sessions**

请求头：

```http
Content-Type: header_type
```

请求消息体：

```jsdn
{
    "UserName":name,
    "Password":password
}
```

**请求参数<a name="zh-cn_topic_0000001446921641_section143771844135817"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|UserName|必选|新建会话对应的用户。|字符串，长度为1~16个字符，支持数字和英文字母，不能全为数字。|
|Password|必选|新建会话对应的用户的密码。|边缘管理系统的用户对应的密码。字符串，长度为8~20个字符。|

**使用指南<a name="zh-cn_topic_0000001446921641_zh-cn_topic_0000001129385271_zh-cn_topic_0178823224_section35335711"></a>**

在Redfish操作过程中，该**POST**操作是首先要执行的。因为后续大部分操作，都需要在“Headers”中携带“X-Auth-Token”值用于鉴权，而“X-Auth-Token”可通过本操作获得。

**使用实例<a name="zh-cn_topic_0000001446921641_zh-cn_topic_0000001129385271_zh-cn_topic_0178823224_section49585950"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/SessionService/Sessions
```

请求头：

```http
Content-Type: application/json
```

请求消息体：

```json
{
    "UserName": "username",
    "Password": "password"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Session.Session",
    "@odata.id": "/redfish/v1/SessionService/Sessions/49fec170a501116ec8e10c8dd279fe51218a3a805ee37173",
    "@odata.type": "#Session.v1_4_0.Session",
    "Id": "49fec170a501116ec8e10c8dd279fe51218a3a805ee37173",
    "Name": "User Session",
    "UserName": "admin",
    "Oem": {
        "UserId": 1,
        "AccountInsecurePrompt": false,
        "message": "[1:admin]"
    }
}
```

响应码：201

**输出说明<a name="zh-cn_topic_0000001446921641_zh-cn_topic_0000001129385271_zh-cn_topic_0178823224_section43620368"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|新创建会话资源模型的OData描述信息。|
|@odata.id|字符串|新创建会话资源节点的访问路径。|
|@odata.type|字符串|新创建会话资源类型。|
|Id|字符串|新创建会话资源的唯一标识。|
|Name|字符串|新创建会话的名称。|
|UserName|字符串|该会话对应的用户。|
|Oem|对象|自定义属性。|
|UserId|数字|当前会话用户ID。|
|AccountInsecurePrompt|布尔值|账号不安全提示。<li>true：密码为默认密码。</li><li>false：密码不是默认密码。</li>|
|message|字符串|该会话用户及ID消息内容。|

### 删除指定会话<a name="ZH-CN_TOPIC_0000001628729973"></a>

**命令功能<a name="zh-cn_topic_0000001447121517_zh-cn_topic_0000001129668053_zh-cn_topic_0178823225_section62110049"></a>**

删除指定会话。

**命令格式<a name="zh-cn_topic_0000001447121517_zh-cn_topic_0000001129668053_zh-cn_topic_0178823225_section22119530"></a>**

操作类型：**DELETE**

**URL**：**https://**_device\_ip_**/redfish/v1/SessionService/Sessions**/<i>session\_id</i>

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**URL参数<a name="zh-cn_topic_0000001447121517_section472012341135"></a>**

**表 1**  URL参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|session_id|必选|待删除的会话ID。|创建会话时的ID。字符串，由小写字母（a\~f）与数字（0~9）组成，长度为48个字符。|

**使用指南<a name="zh-cn_topic_0000001447121517_zh-cn_topic_0000001129668053_zh-cn_topic_0178823225_section46851500"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447121517_zh-cn_topic_0000001129668053_zh-cn_topic_0178823225_section19010318"></a>**

请求样例：

```http
DELETE https://10.10.10.10/redfish/v1/SessionService/Sessions/44010860c8d7dfe60a8728dc33dfd354efabe3eefc68535e
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Logout Success",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447121517_zh-cn_topic_0000001129668053_zh-cn_topic_0178823225_section36875142"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|消息详情。|
|@odata.type|字符串|会话资源类型。|
|Description|字符串|详细信息。|
|Message|字符串|返回消息。|
|Severity|字符串|严重性，Redfish支持的严重级别。<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|传递的参数数量。|
|ParamTypes|数组|传递的参数类型。|
|Resolution|字符串|解决方案描述。|

## 系统监测<a name="ZH-CN_TOPIC_0000001628849957"></a>

### 查询系统资源信息<a name="ZH-CN_TOPIC_0000001628610517"></a>

**命令功能<a name="zh-cn_topic_0000001447281881_zh-cn_topic_0000001082925754_zh-cn_topic_0178823230_section65976137"></a>**

查询当前系统集合资源的信息。

**命令格式<a name="zh-cn_topic_0000001447281881_zh-cn_topic_0000001082925754_zh-cn_topic_0178823230_section56914325"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447281881_zh-cn_topic_0000001082925754_zh-cn_topic_0178823230_section46657603"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447281881_zh-cn_topic_0000001082925754_zh-cn_topic_0178823230_section17265247"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems",
    "@odata.id": "/redfish/v1/Systems",
    "@odata.type": "#ComputerSystem.v1_18_0.ComputerSystem",
    "Id": "1",
    "Name": "Computer System",
    "HostName": "Euler",
    "UUID": "025VLB10K6000019",
    "Model": "Atlas 200 DK A2",
    "SupportModel": "Atlas 200 DK A2",
    "SerialNumber": "2102312NNU10K7000017",
    "AssetTag": "perl",
    "Status": {
        "Health": "OK"
    },
    "Processors": {
        "@odata.id": "/redfish/v1/Systems/Processors"
    },
    "Memory": {
        "@odata.id": "/redfish/v1/Systems/Memory"
    },
    "EthernetInterfaces": {
        "@odata.id": "/redfish/v1/Systems/EthernetInterfaces"
    },
    "LogServices": {
        "@odata.id": "/redfish/v1/Systems/LogServices"
    },
    "SimpleStorages": {
        "@odata.id": "/redfish/v1/Systems/SimpleStorages"
    },
    "Oem": {
        "PCBVersion": 1,
        "Temperature": 50,
        "Power": 23,
        "Voltage": 0.02,
        "CpuHeating": "Stop",
        "DiskHeating": "Stop",
        "UsbHubHeating": "Heating",
        "AiTemperature": 55,
        "ProcessorArchitecture": "ARM",
        "OSVersion": "EulerOS 2.0 (SP10)",
        "KernelVersion": "4.19.90",
        "Uptime": "06:56:58 up 6 min",
        "Datetime": "Wed Nov 16 06:56:58 UTC 2022",
        "DateTimeLocalOffset": "UTC (UTC, +0000)",
        "CpuUsage": 9.09,
        "MemoryUsage": 16.31,
        "Firmware": [
            {
                "Version": "3.0",
                "InactiveVersion": "3.0",
                "Module": "MindXOM",
                "BoardId": "",
                "UpgradeResult": false,
                "UpgradeProcess": 0
            },
         ]
        "InactiveConfiguration": null,
        "NTPService": {
            "@odata.id": "/redfish/v1/Systems/NTPService"
        },
        "ExtendedDevices": {
            "@odata.id": "/redfish/v1/Systems/ExtendedDevices"
        },
        "LTE": {
            "@odata.id": "/redfish/v1/Systems/LTE"
        },
        "Partitions": {
            "@odata.id": "/redfish/v1/Systems/Partitions"
        },
        "NfsManage": {
            "@odata.id": "/redfish/v1/Systems/NfsManage"
        },
        "SecurityService": {
            "@odata.id": "/redfish/v1/Systems/SecurityService"
        },
        "Alarm": {
            "@odata.id": "/redfish/v1/Systems/Alarm"
        },
        "SystemTime": {
            "@odata.id": "/redfish/v1/Systems/SystemTime"
        },
        "EthIpList": {
            "@odata.id": "/redfish/v1/Systems/EthIpList"
        },
        "Modules": {
            "@odata.id": "/redfish/v1/Systems/Modules"
        }
    },
    "Actions": {
        "#ComputerSystem.Reset": {
            "target": "/redfish/v1/Systems/Actions/ComputerSystem.Reset"
        },
        "Oem": {
            "#RestoreDefaults.Reset": {
                "target": "/redfish/v1/Systems/Actions/RestoreDefaults.Reset"
            }
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447281881_zh-cn_topic_0000001082925754_zh-cn_topic_0178823230_section21169502"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|指定系统资源模型的OData描述信息。|
|@odata.id|字符串|指定系统资源的访问路径。|
|@odata.type|字符串|指定系统资源的类型。|
|Id|字符串|指定系统资源的ID。|
|Name|字符串|指定系统资源的名称。|
|HostName|字符串|系统主机名称。|
|UUID|字符串|系统唯一标识。|
|Model|字符串|产品名称。|
|SupportModel|字符串|支持升级的产品名称。|
|SerialNumber|字符串|产品序列号。|
|AssetTag|字符串|用户定义电子标签。|
|Status|对象|指定系统资源的状态。Health：系统资源健康状态。|
|Processors|对象|处理器接口资源的访问路径。|
|Memory|对象|内存接口资源的访问路径。|
|EthernetInterfaces|对象|以太网接口资源的访问路径。|
|LogServices|对象|日志接口资源的访问路径。|
|SimpleStorages|对象|简单存储接口资源的访问路径。|
|Oem|对象|自定义属性。|
|PCBVersion|数字|PCB版本号。|
|Temperature|数字|系统温度。|
|Power|数字|系统功率。|
|Voltage|数字|系统电压。|
|CpuHeating|字符串|CPU加热状态。|
|DiskHeating|字符串|硬盘加热状态。|
|UsbHubHeating|字符串|USB Hub加热状态。|
|AiTemperature|数字|MiniD温度。|
|ProcessorArchitecture|字符串|处理器架构。|
|OSVersion|字符串|操作系统版本。|
|KernelVersion|字符串|内核版本号。|
|Uptime|字符串|系统连续运行时间。|
|Datetime|字符串|系统当前时间。<br> [!NOTE] 说明</br>由于系统数据缓存，返回的时间与系统时间存在20秒左右的偏差。|
|DateTimeLocalOffset|字符串|系统时间时区。|
|CpuUsage|数字|CPU占用率百分比，0~100。|
|MemoryUsage|数字|内存占用率百分比，0~100。|
|Firmware|列表|固件列表。<li>Ascend-firmware</li><li>NPU Driver</li>|
|Version|字符串|当前运行的版本号。|
|InactiveVersion|字符串|待生效版本号。|
|Module|字符串|固件名称。<li>Ascend-firmware</li><li>NPU Driver</li>|
|BoardId|字符串|板号。|
|UpgradeResult|布尔值|固件升级结果。<li>true</li><li>false</li><li>null（表示固件未进行升级）</li>|
|UpgradeProcess|数字|升级进度|
|InactiveConfiguration|字符串|未生效已导入的配置文件名称。|
|NTPService|对象|NTPService接口资源的访问路径。|
|ExtendedDevices|对象|外部设备接口资源的访问路径。|
|LTE|对象|无线网络接口资源的访问路径。|
|Partitions|对象|磁盘分区接口资源的访问路径。|
|NfsManage|对象|NfsManage接口资源的访问路径。|
|SecurityService|对象|安全服务接口资源的访问路径。|
|Alarm|对象|告警接口资源的访问路径。|
|SystemTime|对象|系统时间接口资源的访问路径。|
|EthIpList|对象|获取网口与IP列表接口资源的访问路径。|
|Modules|对象|外部设备模组接口资源的访问路径。|
|Actions|对象|可执行操作。|
|#ComputerSystem.Reset|对象|OS重启操作。|
|#RestoreDefaults.Reset|对象|恢复出厂访问操作。|

### 修改系统资源属性<a name="ZH-CN_TOPIC_0000001628849945"></a>

**命令功能<a name="zh-cn_topic_0000001396761862_zh-cn_topic_0000001129563531_zh-cn_topic_0178823231_section22682682"></a>**

修改当前系统资源属性。

**命令格式<a name="zh-cn_topic_0000001396761862_zh-cn_topic_0000001129563531_zh-cn_topic_0178823231_section2817551"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "AssetTag": tag,
    "DateTime": time,
    "DateTimeLocalOffset": offset,
    "HostName": hostname
}
```

**请求参数<a name="zh-cn_topic_0000001396761862_section12975146182812"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|AssetTag|可选|自定义的资产标签。|取值范围为1～255位的字符串。可由数字、英文字母和英文标点符号等组成（即ASCII码值从0x20～0x7E的字符）。|
|DateTime|可选|系统当前时间。|字符串。长度为10~64字符，时间格式：%Y-%m-%d %H:%M:%S或者是%Y-%m-%d。<br> [!NOTE] 说明</br>系统支持设置的时间范围为Linux系统支持的时间范围。|
|DateTimeLocalOffset|可选|系统时间时区。|字符串。长度为0~100可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（+-/:_）组成且不含“..”。|
|HostName|可选|系统主机名称。|取值范围为1～63位的字符串。可由数字（0\~9）、英文字母（a\~z、A\~Z）和连字符（-）组成，且连字符不能出现在开头和结尾。|

**使用指南<a name="zh-cn_topic_0000001396761862_zh-cn_topic_0000001129563531_zh-cn_topic_0178823231_section26895113"></a>**

无

**使用实例<a name="zh-cn_topic_0000001396761862_zh-cn_topic_0000001129563531_zh-cn_topic_0178823231_section40729432"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems
```

请求头：

```http
X-Auth-Token: auth_value 
Content-Type: application/json 
```

请求消息体：

```json
{
    "AssetTag": "test00",
    "DateTime": "2022-05-13 15:36:10",
    "DateTimeLocalOffset": "UTC",
    "HostName": "Atlas 200I A2"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems",
    "@odata.id": "/redfish/v1/Systems",
    "@odata.type": "#ComputerSystem.v1_18_0.ComputerSystem",
    "Id": "1",
    "Name": "Computer System",
    "HostName": "Atlas 200 A2",
    "UUID": "025VLB10K6000019",
    "Model": "Atlas 200 A2",
    "SupportModel": "Atlas 200 DK A2",
    "SerialNumber": "2102312NNU10K7000017",
    "AssetTag": "test00",
    "Status": {
        "Health": "OK"
    },
    "Processors": {
        "@odata.id": "/redfish/v1/Systems/Processors"
    },
    "Memory": {
        "@odata.id": "/redfish/v1/Systems/Memory"
    },
    "EthernetInterfaces": {
        "@odata.id": "/redfish/v1/Systems/EthernetInterfaces"
    },
    "LogServices": {
        "@odata.id": "/redfish/v1/Systems/LogServices"
    },
    "SimpleStorages": {
        "@odata.id": "/redfish/v1/Systems/SimpleStorages"
    },
    "Oem": {
        "PCBVersion": 1,
        "Temperature": 50,
        "Power": 23,
        "Voltage": 0.02,
        "CpuHeating": "Stop",
        "DiskHeating": "Stop",
        "UsbHubHeating": "Heating",
        "AiTemperature": 55,
        "SoftwareVersion": "22.0.3",
        "ProcessorArchitecture": "ARM",
        "OSVersion": "EulerOS 2.0 (SP10)",
        "KernelVersion": "4.19.90",
        "Uptime": "06:56:58 up 6 min",
        "Datetime": "Fri May 13 15:36:17 UTC 2022",
        "DateTimeLocalOffset": "UTC (UTC, +0000)",
        "CpuUsage": 9.09,
        "MemoryUsage": 16.31,
        "Firmware": [
            {
                "Version": "3.0",
                "InactiveVersion": "3.0",
                "Module": "Ascend-firmware",
                "BoardId": "",
                "UpgradeResult": false,
                "UpgradeProcess": 0
            }
        ],
        "InactiveFirmware": [],
        "InactiveConfiguration": null,
        "NTPService": {
            "@odata.id": "/redfish/v1/Systems/NTPService"
        },
        "ExtendedDevices": {
            "@odata.id": "/redfish/v1/Systems/ExtendedDevices"
        },
        "LTE": {
            "@odata.id": "/redfish/v1/Systems/LTE"
        },
        "NfsManage": {
            "@odata.id": "/redfish/v1/Systems/NfsManage"
        },
        "SecurityService": {
            "@odata.id": "/redfish/v1/Systems/SecurityService"
        },
        "Alarm": {
            "@odata.id": "/redfish/v1/Systems/Alarm"
        },
        "SystemTime": {
            "@odata.id": "/redfish/v1/Systems/SystemTime"
        },
        "EthIpList": {
            "@odata.id": "/redfish/v1/Systems/EthIpList"
        },
        "Modules": {
            "@odata.id": "/redfish/v1/Systems/Module"
        }
    },
    "Actions": {
        "#ComputerSystem.Reset": {
            "target": "/redfish/v1/Systems/Actions/ComputerSystem.Reset"
        },
        "Oem": {
            "#ComputerSystem.RestoreDefaults": {
                "target": "/redfish/v1/Systems/Actions/ComputerSystem.RestoreDefaults"
            }
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001396761862_zh-cn_topic_0000001129563531_zh-cn_topic_0178823231_section31020572"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|指定系统资源模型的OData描述信息。|
|@odata.id|字符串|指定系统资源的访问路径。|
|@odata.type|字符串|指定系统资源的类型。|
|Id|字符串|指定系统资源的ID。|
|Name|字符串|指定系统资源的名称。|
|HostName|字符串|系统主机名称。|
|UUID|字符串|系统唯一标识。|
|Model|字符串|产品名称。|
|SupportModel|字符串|支持升级的产品名称。|
|SerialNumber|字符串|产品序列号。|
|AssetTag|字符串|用户定义电子标签。|
|Status|对象|指定系统资源的状态。Health：系统资源健康状态。|
|Processors|对象|处理器接口资源的访问路径。|
|Memory|对象|内存接口资源的访问路径。|
|EthernetInterfaces|对象|以太网接口资源的访问路径。|
|LogServices|对象|日志接口资源的访问路径。|
|SimpleStorage|对象|简单存储接口资源的访问路径。|
|Oem|对象|自定义属性。|
|PCBVersion|数字|PCB版本号。|
|Temperature|数字|系统温度。|
|Power|数字|系统功率。|
|Voltage|数字|系统电压。|
|CpuHeating|字符串|CPU加热状态。|
|DiskHeating|字符串|硬盘加热状态。|
|UsbHubHeating|字符串|USB Hub加热状态。|
|AiTemperature|数字|MiniD温度。|
|SoftwareVersion|字符串|系统软件版本号。|
|ProcessorArchitecture|字符串|处理器架构。|
|OSVersion|字符串|操作系统版本。|
|KernelVersion|字符串|内核版本号。|
|Uptime|字符串|系统连续运行时间。|
|Datetime|字符串|系统当前时间。<br> [!NOTE] 说明</br>由于系统数据缓存，返回的时间与系统时间存在20秒左右的偏差。|
|DateTimeLocalOffset|字符串|系统时间时区。|
|CpuUsage|数字|CPU占用率百分比，取值范围为0~100。|
|MemoryUsage|数字|内存占用率百分比，取值范围为0~100。|
|Firmware|列表|固件列表。取值为Ascend-firmware|
|InactiveFirmware|列表|未生效固件的名称。如有多个未生效固件，则以逗号隔开。|
|InactiveConfiguration|字符串|未生效的已导入的配置文件名称。|
|NTPService|对象|NTPService接口资源的访问路径。|
|ExtendedDevices|对象|外部设备接口资源的访问路径。|
|LTE|对象|无线网络接口资源的访问路径。|
|Partitions|对象|磁盘接口资源的访问路径。|
|NfsManage|对象|NFS接口资源的访问路径。|
|SecurityService|对象|安全服务接口资源的访问路径。|
|Alarm|对象|告警接口资源的访问路径。|
|SystemTime|对象|系统时间接口资源的访问路径。|
|EthIpList|对象|获取网口与IP列表接口资源的访问路径。|
|Modules|对象|外部设备模组接口资源的访问路径。|
|Actions|对象|可执行操作。|
|#ComputerSystem.Reset|对象|OS重启操作。|
|#ComputerSystem.RestoreDefaults|对象|远程恢复出厂设置操作。|

### 复位系统操作<a name="ZH-CN_TOPIC_0000001578449972"></a>

**命令功能<a name="zh-cn_topic_0000001129563525_zh-cn_topic_0178823232_section5012580"></a>**

复位系统。

**命令格式<a name="zh-cn_topic_0000001129563525_zh-cn_topic_0178823232_section45113228"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Actions/ComputerSystem.Reset**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "ResetType": value
}
```

**请求参数<a name="zh-cn_topic_0000001129563525_zh-cn_topic_0178823232_section3365875"></a>**

**表 1**  参数说明表

|参数|是否必选|参数说明|取值|
|--|--|--|--|
|ResetType|必选|复位类型。|字符串。<br>GracefulRestart：平滑复位。</br> [!NOTE] 说明<br>在执行升级、升级生效、系统复位和恢复出厂操作时，再执行系统复位操作，系统复位操作会执行失败。</br>|

**使用指南<a name="zh-cn_topic_0000001129563525_zh-cn_topic_0178823232_section30292883"></a>**

无

**使用实例<a name="zh-cn_topic_0000001129563525_zh-cn_topic_0178823232_section4200491"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/Actions/ComputerSystem.Reset
```

请求头：

```http
X-Auth-Token: auth_value 
Content-Type: application/json
```

请求消息体：

```json
{
    "ResetType":"GracefulRestart"
}
```

**输出说明<a name="zh-cn_topic_0000001129563525_zh-cn_topic_0178823232_section37804421"></a>**

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Restart system (GracefulRestart) successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：200

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|消息详情。|
|@odata.type|字符串|会话资源类型。|
|Description|字符串|详细信息。|
|Message|字符串|返回消息。|
|Severity|字符串|严重性，Redfish支持的严重级别。<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|传递的参数数量。|
|ParamTypes|数组|传递的参数类型。|
|Resolution|字符串|解决方案描述。|

### 查询处理器资源集合信息<a name="ZH-CN_TOPIC_0000001628849905"></a>

**命令功能<a name="zh-cn_topic_0000001416124898_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section34679494"></a>**

查询当前处理器资源集合信息。

**命令格式<a name="zh-cn_topic_0000001416124898_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section43679990"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Processors**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001416124898_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section48418320"></a>**

无

**使用实例<a name="zh-cn_topic_0000001416124898_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section33111699"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Processors
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Processors/#entity",
    "@odata.id": "/redfish/v1/Systems/Processors",
    "@odata.type": "#ProcessorCollection.ProcessorCollection",
    "Name": "Processors Collection",
    "Members@odata.count": 2,
    "Members": [{
        "@odata.id": "/redfish/v1/Systems/Processors/CPU"
        },
        {
         "@odata.id": "/redfish/v1/Systems/Processors/AiProcessor"
        }
    ]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001416124898_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section29569838"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|处理器资源集合模型的OData描述信息。|
|@odata.id|字符串|处理器资源集合的访问路径。|
|@odata.type|字符串|处理器资源集合的类型。|
|Name|字符串|处理器资源集合的名称。|
|Members@odata.count|数字|当前处理器资源数量。|
|Members|列表|处理器资源列表。|
|@odata.id|字符串|单个处理器资源节点的访问路径。|

### 查询CPU概要信息<a name="ZH-CN_TOPIC_0000001577810560"></a>

**命令功能<a name="zh-cn_topic_0000001447121501_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section34679494"></a>**

查询当前CPU概要信息。

**命令格式<a name="zh-cn_topic_0000001447121501_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section43679990"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Processors/CPU**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447121501_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section48418320"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447121501_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section33111699"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Processors/CPU
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Processors/Members/$entity",
    "@odata.id": "/redfish/v1/Systems/Processors/CPU",
    "@odata.type": "#Processor.v1_15_0.Processor",
    "Name": "CPU",
    "Id": "CPU",
    "Manufacturer": "Huawei",
    "Oem": {
        "CPUModel": "Hi3559AV100",
        "Count": "ARM Cortex A73 * 2 + ARM Cortex A53 * 2"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447121501_zh-cn_topic_0000001129385265_zh-cn_topic_0178823234_section29569838"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|CPU资源模型的OData描述信息。|
|@odata.id|字符串|CPU资源的访问路径。|
|@odata.type|字符串|CPU资源的类型。|
|Name|字符串|CPU资源的名称。|
|Id|字符串|CPU资源的ID。|
|Manufacturer|字符串|生产厂商。|
|Oem|对象|自定义属性。|
|CPUModel|字符串|CPU型号。|
|Count|字符串|CPU资源类型和数量。|

### 查询AI处理器资源信息<a name="ZH-CN_TOPIC_0000001628729997"></a>

**命令功能<a name="zh-cn_topic_0000001397081570_zh-cn_topic_0000001129385275_zh-cn_topic_0178823240_section7589703"></a>**

查询AI处理器资源信息。

**命令格式<a name="zh-cn_topic_0000001397081570_zh-cn_topic_0000001129385275_zh-cn_topic_0178823240_section1198467"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/Systems/Processors/AiProcessor**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001397081570_zh-cn_topic_0000001129385275_zh-cn_topic_0178823240_section29967027"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397081570_zh-cn_topic_0000001129385275_zh-cn_topic_0178823240_section1267793"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Processors/AiProcessor
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Processors/Members/$entity",
    "@odata.id": "/redfish/v1/Systems/Processors/AiProcessor",
    "@odata.type": "#Processor.v1_15_0.Processor",
    "Name": "AiProcessor",
    "Id": "AiProcessor",
    "Manufacturer": "Huawei Technologies Co., Ltd",
    "Model": "Atlas 200I A2",
    "Status": {
        "Health": "OK",
        "State": true
    },
    "Oem": {
        "Count": 1,
        "Capability": {
            "Calc": "20TOPS",
            "Ddr": 8
        },
        "OccupancyRate": {
            "AiCore": 0,
            "AiCpu": 0,
            "CtrlCpu": 0,
            "DdrUsage": 7,
            "DdrBandWidth": 0
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397081570_zh-cn_topic_0000001129385275_zh-cn_topic_0178823240_section11410140"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|AI处理器资源模型的OData描述信息。|
|@odata.id|字符串|AI处理器资源的访问路径。|
|@odata.type|字符串|AI处理器资源的类型。|
|Name|字符串|AI处理器资源的名称。|
|Id|字符串|AI处理器资源的Id。|
|Manufacturer|字符串|AI处理器厂商。|
|Model|字符串|AI处理器型号。|
|Status|对象|AI处理器的状态。<li>Health：健康状态</li><li>State：使能状态</li>|
|Oem|对象|自定义属性。|
|Count|数字|AI处理器个数。|
|Capability|对象|AI处理器能力。|
|Capability.Calc|字符串|AI处理器算力，例如16TOPS。|
|Capability.Ddr|数字|AI处理器内存，例如4GB。|
|OccupancyRate|对象|AI处理器资源占用率。|
|OccupancyRate.AiCore|数字|AI Core占用率。|
|OccupancyRate.AiCpu|数字|AI CPU占用率。|
|OccupancyRate.CtrlCpu|数字|控制CPU占用率。|
|OccupancyRate.DdrUsage|数字|DDR内存占用率。|
|OccupancyRate.DdrBandWidth|数字|内存带宽占用率。|

### 查询内存概要信息<a name="ZH-CN_TOPIC_0000001577530776"></a>

**命令功能<a name="zh-cn_topic_0000001446921649_zh-cn_topic_0000001129822443_zh-cn_topic_0178823235_section65777515"></a>**

查询内存概要信息。

**命令格式<a name="zh-cn_topic_0000001446921649_zh-cn_topic_0000001129822443_zh-cn_topic_0178823235_section55126729"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Memory**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001446921649_zh-cn_topic_0000001129822443_zh-cn_topic_0178823235_section36080041"></a>**

无

**使用实例<a name="zh-cn_topic_0000001446921649_zh-cn_topic_0000001129822443_zh-cn_topic_0178823235_section56284918"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Memory
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.type": "#Memory.v1_15_0.Memory",
    "@odata.context": "/redfish/v1/$metadata#Systems/Memory",
    "@odata.id": "/redfish/v1/Systems/Memory",
    "Id": "Memory",
    "Name": "Memory",
    "Oem": {
        "TotalSystemMemoryGiB": 4
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001446921649_zh-cn_topic_0000001129822443_zh-cn_topic_0178823235_section36802218"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.type|字符串|内存资源的类型。|
|@odata.context|字符串|内存资源模型的OData描述信息。|
|@odata.id|字符串|内存资源的访问路径。|
|Id|字符串|内存资源的ID。|
|Name|字符串|内存资源的名称。|
|Oem|对象|自定义属性。|
|TotalSystemMemoryGiB|数字|OS可访问内存总大小。|

### 查询NTP服务信息<a name="ZH-CN_TOPIC_0000001628490569"></a>

**命令功能<a name="zh-cn_topic_0000001397241518_zh-cn_topic_0000001082606162_zh-cn_topic_0178823236_section11376373"></a>**

查询NTP配置信息。

**命令格式<a name="zh-cn_topic_0000001397241518_zh-cn_topic_0000001082606162_zh-cn_topic_0178823236_section35278494"></a>**

操作类型：**GET**

**URL：https://**_device\_ip_**/redfish/v1/Systems/NTPService**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001397241518_zh-cn_topic_0000001082606162_zh-cn_topic_0178823236_section38985771"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397241518_zh-cn_topic_0000001082606162_zh-cn_topic_0178823236_section15327625"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/NTPService
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.type": "#MindXEdgeNTPService.v1_0_0.MindXEdgeNTPService",
    "@odata.context": "/redfish/v1/$metadata#Systems/NTPService",
    "@odata.id": "/redfish/v1/Systems/NTPService",
    "Id": "NTPService",
    "Name": "NTPService",
    "ClientEnabled": true,
    "ServerEnabled": false,
    "Port": null,
    "NTPRemoteServers": "127.127.xx.xx",
    "NTPRemoteServersbak": null,
    "NTPLocalServers": ""
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397241518_zh-cn_topic_0000001082606162_zh-cn_topic_0178823236_section3730901"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.type|字符串|NTP服务资源的类型。|
|@odata.context|字符串|NTP服务资源模型的OData描述信息。|
|@odata.id|字符串|NTP服务资源的访问路径。|
|Id|字符串|NTP服务资源的ID。|
|Name|字符串|NTP服务资源的名称。|
|ClientEnabled|布尔值|NTP客户端是否使能。|
|ServerEnabled|布尔值|NTP服务器是否使能。|
|Port|字符串|NTP服务端口号。|
|NTPRemoteServers|字符串|首选NTP远端服务器IP地址。|
|NTPRemoteServersbak|字符串|备选NTP远端服务器IP地址。|
|NTPLocalServers|字符串|用于NTP服务的本机IP地址。|

### 配置NTP服务信息<a name="ZH-CN_TOPIC_0000001628610541"></a>

**命令功能<a name="zh-cn_topic_0000001397081522_zh-cn_topic_0000001129668045_zh-cn_topic_0178823237_section1246702"></a>**

配置NTP服务信息。

**命令格式<a name="zh-cn_topic_0000001397081522_zh-cn_topic_0000001129668045_zh-cn_topic_0178823237_section11220318"></a>**

操作类型：**PATCH**

**URL：https://**_device\_ip_**/redfish/v1/Systems/NTPService**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "ClientEnabled":ClientEnabled,
    "NTPLocalServers":NTPLocalServers,
    "NTPRemoteServers":NTPRemoteServers,
    "NTPRemoteServersbak":NTPRemoteServersbak,
    "ServerEnabled":ServerEnabled,
    "Target":Target
}
```

**请求参数<a name="zh-cn_topic_0000001397081522_section760410326354"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|ClientEnabled|必选|NTP客户端是否使能。|布尔值，取值为true或false。|
|NTPLocalServers|可选|用于NTP服务的本机IP地址。|字符串，IPv4地址。仅当ServerEnabled为true时参数有效<br> [!NOTE] 说明</br>该字段在此版本上无效。|
|NTPRemoteServers|必选|首选NTP远端服务器IP地址。|字符串，IPv4地址。仅当ClientEnabled为true时参数有效，且有效时取值必须与NTPRemoteServersbak不同|
|NTPRemoteServersbak|必选|备选NTP远端服务器IP地址。|字符串，IPv4地址。仅当ClientEnabled为true时参数有效，且有效时取值必须与NTPRemoteServers不同|
|ServerEnabled|必选|NTP服务器是否使能。|布尔值，取值为false。|
|Target|可选|NTP服务的配置类型，此参数可选，默认值为Client。|字符串，取值为Client。|

**使用指南<a name="zh-cn_topic_0000001397081522_zh-cn_topic_0000001129668045_zh-cn_topic_0178823237_section36430550"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397081522_zh-cn_topic_0000001129668045_zh-cn_topic_0178823237_section59439495"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/NTPService 
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "ClientEnabled":true,
    "NTPLocalServers":"",
    "NTPRemoteServers":"192.168.xx.xx",
    "NTPRemoteServersbak":"",
    "ServerEnabled":false,
    "Target":"Client"
}
```

响应样例：

```json
{
    "@odata.type": "#MindXEdgeNTPService.v1_0_0.MindXEdgeNTPService",
    "@odata.context": "/redfish/v1/$metadata#Systems/NTPService",
    "@odata.id": "/redfish/v1/Systems/NTPService",
    "Id": "NTPService",
    "Name": "NTPService",
    "ClientEnabled": true,
    "ServerEnabled": false, 
    "Port": null,
    "NTPRemoteServers": "192.168.xx.xx", 
    "NTPRemoteServersbak": null, 
    "NTPLocalServers": ""
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397081522_zh-cn_topic_0000001129668045_zh-cn_topic_0178823237_section65193414"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.type|字符串|NTP服务资源的类型。|
|@odata.context|字符串|NTP服务资源模型的OData描述信息。|
|@odata.id|字符串|NTP服务资源的访问路径。|
|Id|字符串|NTP服务资源的ID。|
|Name|字符串|NTP服务资源的名称。|
|ClientEnabled|布尔值|NTP客户端是否使能。|
|ServerEnabled|布尔值|NTP服务器是否使能。|
|Port|字符串|NTP服务端口号。|
|NTPRemoteServers|字符串|首选NTP远端服务器IP地址。|
|NTPRemoteServersbak|字符串|备选NTP远端服务器IP地址。|
|NTPLocalServers|字符串|用于NTP服务的本机IP地址。|

### 查询告警资源信息<a name="ZH-CN_TOPIC_0000001578449952"></a>

**命令功能<a name="zh-cn_topic_0000001397241510_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section44345243"></a>**

查询告警资源信息。

**命令格式<a name="zh-cn_topic_0000001397241510_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section63562868"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Alarm**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001397241510_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section48318689"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397241510_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section32215023"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Alarm
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Alarm",
    "@odata.id": "/redfish/v1/Systems/Alarm",
    "@odata.type": "MindXEdgeAlarm.v1_0_0.MindXEdgeAlarm",
    "Id": "Alarm",
    "Name": "Alarm",
    "AlarmInfo": {
        "@odata.id": "/redfish/v1/Systems/Alarm/AlarmInfo"
    },
    "AlarmShield": {
        "@odata.id": "/redfish/v1/Systems/Alarm/AlarmShield"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397241510_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section21499755"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|告警资源模型的OData描述信息。|
|@odata.id|字符串|告警资源的访问路径。|
|@odata.type|字符串|告警资源的类型。|
|Id|字符串|告警资源的ID。|
|Name|字符串|告警资源的名称。|
|AlarmInfo|对象|告警资源服务信息。|
|AlarmShield|对象|告警资源的屏蔽规则。|

### 查询告警资源服务<a name="ZH-CN_TOPIC_0000001628849913"></a>

**命令功能<a name="zh-cn_topic_0000001472475913_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section44345243"></a>**

查询告警资源服务。

**命令格式<a name="zh-cn_topic_0000001472475913_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section63562868"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Alarm/AlarmInfo**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001472475913_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section48318689"></a>**

无

**使用实例<a name="zh-cn_topic_0000001472475913_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section32215023"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Alarm/AlarmInfo
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Alarm/AlarmInfo",
    "@odata.id": "/redfish/v1/Systems/Alarm/AlarmInfo",
    "@odata.type": "MindXEdgeAlarm.v1_0_0.MindXEdgeAlarm",
    "Id" : "Alarm Info",
    "Name": "Alarm Info",
    "AlarMessages": [
        {
            "AlarmId": "00160000",
            "AlarmName": "directory space full",
            "AlarmInstance": "MEM OR STORAGE",
            "Timestamp": "2022-12-25 00:07:31",
            "PerceivedSeverity": "2"
        }
    ]
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001472475913_zh-cn_topic_0000001129822439_zh-cn_topic_0178823259_section21499755"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|告警资源服务模型的OData描述信息。|
|@odata.id|字符串|告警资源服务的访问路径。|
|@odata.type|字符串|告警资源服务的类型。|
|Name|字符串|告警资源服务的名称。|
|Id|字符串|告警资源服务的ID。|
|AlarMessages|数组|告警信息集合。|
|AlarmId|字符串|告警码。|
|AlarmName|字符串|告警名称。|
|AlarmInstance|字符串|告警对象，故障点器件。|
|Timestamp|字符串|告警产生时间。|
|PerceivedSeverity|字符串|严重程度。<li>0：紧急</li><li>1：严重</li><li>2：一般</li>|

### 查询告警屏蔽规则<a name="ZH-CN_TOPIC_0000001628849925"></a>

**命令功能<a name="zh-cn_topic_0000001447161465_zh-cn_topic_0000001129563539_section44345243"></a>**

查询告警屏蔽规则。

**命令格式<a name="zh-cn_topic_0000001447161465_zh-cn_topic_0000001129563539_section63562868"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Alarm/AlarmShield**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001447161465_zh-cn_topic_0000001129563539_section48318689"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447161465_zh-cn_topic_0000001129563539_section32215023"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/Alarm/AlarmShield
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/Alarm/AlarmShield",
    "@odata.id": "/redfish/v1/Systems/Alarm/AlarmShield",
    "@odata.type": "MindXEdgeAlarm.v1_0_0.MindXEdgeAlarm",
    "Id": "Alarm Shield",
    "Name": "Alarm Shield",
    "AlarmShieldMessages": [],
    "Increase": {
        "@odata.id": "/redfish/v1/Systems/Alarm/AlarmShield/Increase"
    },
    "Decrease": {
        "@odata.id": "/redfish/v1/Systems/Alarm/AlarmShield/Decrease"
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001447161465_zh-cn_topic_0000001129563539_section21499755"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|告警资源屏蔽规则模型的OData描述信息。|
|@odata.id|字符串|告警资源屏蔽规则的访问路径。|
|@odata.type|字符串|告警资源屏蔽规则的类型。|
|Id|字符串|告警资源屏蔽规则的ID|
|Name|字符串|告警资源服务的名称。|
|AlarmShieldMessages|数组|告警屏蔽信息集合。|
|Increase|对象|创建告警屏蔽的访问路径。|
|Decrease|对象|取消告警屏蔽的访问路径。|

### 创建告警屏蔽规则<a name="ZH-CN_TOPIC_0000001577810584"></a>

**命令功能<a name="zh-cn_topic_0000001397081554_zh-cn_topic_0000001082606170_section44345243"></a>**

创建告警屏蔽规则。

**命令格式<a name="zh-cn_topic_0000001397081554_zh-cn_topic_0000001082606170_section63562868"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Alarm/AlarmShield/Increase**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "AlarmShieldMessages": [
        {
            "UniquelyIdentifies": Identifies,
            "AlarmId": Id,
            "PerceivedSeverity": PerceivedSeverity,
            "AlarmInstance": AlarmInstance
        }
    ]
}
```

**请求参数<a name="zh-cn_topic_0000001397081554_section9147104015377"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|AlarmShieldMessages|必选|告警屏蔽信息集合。|数组，长度为1~256。|
|UniquelyIdentifies|必选|唯一标识符|字符串，长度为1\~32，可由大小写字母（a\~z、A\~Z）与数字（0\~9）组成。|
|AlarmId|必选|告警ID|字符串，长度为1~32，可由大小写字母（a\~z、A\~Z）与数字（0\~9）组成。|
|PerceivedSeverity|必选|严重程度|字符串，取值为0\~9。|
|AlarmInstance|必选|告警对象|字符串，长度为1\~32，可由大小写字母（a\~z、A\~Z）、数字（0\~9）、其他字符（_.-）和空白字符组成。|

**使用指南<a name="zh-cn_topic_0000001397081554_zh-cn_topic_0000001082606170_section48318689"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397081554_zh-cn_topic_0000001129563539_section32215023"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/Alarm/AlarmShield/Increase
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "AlarmShieldMessages": [
        {
            "UniquelyIdentifies": "a000000001",
            "AlarmId": "00000001",
            "PerceivedSeverity": "2",
            "AlarmInstance": "M.2"
        }
    ]
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Increase alarm shield successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001397081554_zh-cn_topic_0000001082606170_section21499755"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性，支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 取消告警屏蔽规则<a name="ZH-CN_TOPIC_0000001628730013"></a>

**命令功能<a name="zh-cn_topic_0000001473472064_section7890325205012"></a>**

取消告警屏蔽规则。

**命令格式<a name="zh-cn_topic_0000001473472064_section163220359506"></a>**

操作类型：**PATCH**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/Alarm/AlarmShield**/**Decrease**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "AlarmShieldMessages": [
        {
            "UniquelyIdentifies": Identifies,
            "AlarmId": Id,
            "PerceivedSeverity": PerceivedSeverity,
            "AlarmInstance": AlarmInstance
        }
    ]
}
```

**参数说明<a name="zh-cn_topic_0000001473472064_section896614520422"></a>**

**表 1**  参数说明

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|AlarmShieldMessages|必选|告警屏蔽信息集合。|数组，长度为1~256。|
|UniquelyIdentifies|必选|唯一标识符|字符串，长度为1\~32，可由大小写字母（a\~z、A\~Z）与数字（0\~9）组成。|
|AlarmId|必选|告警ID|字符串，长度为1~32，可由大小写字母（a\~z、A\~Z）与数字（0\~9）组成。|
|PerceivedSeverity|必选|严重程度|字符串，长度为0~9。|
|AlarmInstance|必选|告警对象|字符串，长度为1~32，可由大小写字母（a\~z、A\~Z）、数字（0\~9）、其他字符（_.\-）和空白字符组成。|

**使用实例<a name="zh-cn_topic_0000001473472064_section14654191355117"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/AlarmShield/Decrease
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "AlarmShieldMessages": [
        {
            "UniquelyIdentifies": "a000000001",
            "AlarmId": "00000001",
            "PerceivedSeverity": "2",
            "AlarmInstance": "M.2"
        }
    ]
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Decrease alarm shield successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001473472064_section34781649105110"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性，支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

## 安全管理<a name="ZH-CN_TOPIC_0000001577810624"></a>

### 查询安全服务资源信息<a name="ZH-CN_TOPIC_0000001578449944"></a>

**命令功能<a name="zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section42149482"></a>**

查询安全服务资源信息。

**命令格式<a name="zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section43801020"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section58221747"></a>**

无

**使用实例<a name="zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section54233682"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/SecurityService
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/SecurityService",
    "@odata.id": "/redfish/v1/Systems/SecurityService",
    "@odata.type": "#MindXEdgeSecurityService.v1_0_0.MindXEdgeSecurityService",
    "Id": "Security Service",
    "Name": "Security Service",
    "HttpsCert": {
        "@odata.id": "/redfish/v1/Systems/SecurityService/HttpsCert"
    },
    "DownloadCSRFile": {
        "@odata.id": "/redfish/v1/Systems/SecurityService/downloadCSRFile"
    },
    "HttpsCertAlarmTime": {
        "@odata.id": "/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime"
    },
    "SecurityLoad": {
         "@odata.id": "/redfish/v1/Systems/SecurityService/SecurityLoad"
    },
    "Actions": {
        "#SecurityService.PunyDictImport": {
            "target": "/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictImport"
        },
        "#SecurityService.PunyDictExport": {
            "target": "/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictExport"
        },
        "#SecurityService.PunyDictDelete": {
            "target": "/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictDelete"
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section18341095"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|安全服务资源模型的OData描述信息。|
|@odata.id|字符串|安全服务资源节点的访问路径。|
|@odata.type|字符串|安全服务资源类型。|
|Id|字符串|安全服务资源的ID。|
|Name|字符串|安全服务资源的名称。|
|HttpsCert|对象|服务器证书资源的访问路径。|
|DownloadCSRFile|对象|下载证书签名请求的访问路径。|
|HttpsCertAlarmTime|对象|证书有效期提醒时间资源的访问路径。|
|SecurityLoad|对象|登录规则资源的访问路径。|
|Actions|对象|可执行的操作。|
|#SecurityService.PunyDictImport|对象|导入弱字典资源。|
|#SecurityService.PunyDictImport.target|字符串|导入弱字典资源的访问路径。|
|#SecurityService.PunyDictExport|对象|导出弱字典资源。|
|#SecurityService.PunyDictExport.target|字符串|导出弱字典资源的访问路径。|
|#SecurityService.PunyDictDelete|对象|删除弱字典资源。|
|#SecurityService.PunyDictDelete.target|字符串|删除弱字典资源的访问路径。|

### 查询SSL证书资源信息<a name="ZH-CN_TOPIC_0000001577530796"></a>

**命令功能<a name="zh-cn_topic_0000001396921698_zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section42149482"></a>**

查询当前支持的SSL证书资源的信息。

**命令格式<a name="zh-cn_topic_0000001396921698_zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section43801020"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/HttpsCert**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001396921698_zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section58221747"></a>**

无

**使用实例<a name="zh-cn_topic_0000001396921698_zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section54233682"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/SecurityService/HttpsCert
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#EdgeSystem/SecurityService/HttpsCert/$entity",
    "@odata.id": "/redfish/v1/EdgeSystem/SecurityService/HttpsCert",
    "@odata.type": "#MindXEdgeHttpsCert.v1_0_0.MindXEdgeHttpsCert",
    "Id": "HttpsCert",
    "Name": "Https cert info",
    "X509CertificateInformation": {
        "ServerCert": {
            "Subject": "CN=Server, OU=IT, O=Huawei, L=ShenZhen, S=GuangDong, C=CN",
            "Issuer": "CN=Server, OU=IT, O=Huawei, L=ShenZhen, S=GuangDong, C=CN",
            "ValidNotBefore": "Jul 25 2014 GMT",
            "ValidNotAfter": "Jul 22 2024 GMT",
            "SerialNumber": "07"
            "FingerPrint": "XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:",
            "HttpsCertEable": "",
            "ExpiredDayRemaining": 6933
        }
    },
    "CertificateSigningRequest": null,
    "Actions": {
        "#HttpsCert.ImportServerCertificate": {
            "target": "/redfish/v1/Systems/SecurityService/HttpsCert/Actions/HttpsCert.ImportServerCertificate"
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001396921698_zh-cn_topic_0000001082765798_zh-cn_topic_0178823253_section18341095"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|SSL证书资源模型的OData描述信息。|
|@odata.id|字符串|SSL证书资源节点的访问路径。|
|@odata.type|字符串|SSL证书资源类型。|
|Id|字符串|SSL证书资源的ID。|
|Name|字符串|SSL证书资源的名称。|
|X509CertificateInformation|对象|SSL证书信息。|
|ServerCert.Subject|字符串|证书使用者。|
|ServerCert.Issuer|字符串|证书签发者。|
|ServerCert.ValidNotBefore|字符串|生效起始日期。|
|ServerCert.ValidNotAfter|字符串|生效结束日期。|
|ServerCert.SerialNumber|字符串|证书序列号。|
|ServerCert.FingerPrint|字符串|证书指纹信息。|
|ServerCert.HttpsCertEable|字符串|使能。|
|ServerCert.ExpiredDayRemaining|数字|证书有效期。|
|ServerCert.CertificateSigningRequest|字符串|CSR信息。<br> [!NOTE] 说明</br>导入服务器证书后，之前生成的CSR信息清除，此处显示为“null”。|
|Actions|对象|可执行的操作。|
|Actions.#HttpsCert.ImportServerCertificate|对象|导入服务器证书的资源路径。|

### 导入服务器证书<a name="ZH-CN_TOPIC_0000001628490537"></a>

**命令功能<a name="zh-cn_topic_0000001447161533_zh-cn_topic_0000001129563529_zh-cn_topic_0178823255_section56876138"></a>**

导入服务器证书。

**命令格式<a name="zh-cn_topic_0000001447161533_zh-cn_topic_0000001129563529_zh-cn_topic_0178823255_section42123196"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/HttpsCert/Actions/HttpsCert.ImportServerCertificate**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "FileName":filename,
    "Password":password
}
```

**请求参数<a name="zh-cn_topic_0000001447161533_section191720279460"></a>**

**表 1**  请求参数

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|FileName|必选|服务器证书文件名|字符串，仅支持.cer或者.crt格式的证书。长度为1\~255，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（_.-）组成，且不含“..”，后缀只能为"crt"和"cer"。|
|Password|必选|当前用户的密码|字符串，用户自定义。长度为8~20字符，可由大小写字母（a\~z、A\~Z）、数字（0\~9）、其他字符（`\~!@#$%^&*()-_=+\|[{}];:'",<.>/?）和空格组成，至少包含其中3类字符。|

**使用指南<a name="zh-cn_topic_0000001447161533_zh-cn_topic_0000001129563529_zh-cn_topic_0178823255_section56535697"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447161533_zh-cn_topic_0000001129563529_zh-cn_topic_0178823255_section39059231"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/SecurityService/HttpsCert/Actions/HttpsCert.ImportServerCertificate
```

请求头：

```http
X-Auth-Token: auth_value
Content-Type: application/json
```

请求消息体：

```json
{
    "FileName":"server.crt",
    "Password":"password"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Import custom certificate successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001447161533_zh-cn_topic_0000001129563529_zh-cn_topic_0178823255_section15988759"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性。支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 下载证书签名请求<a name="ZH-CN_TOPIC_0000001628610565"></a>

**命令功能<a name="zh-cn_topic_0000001562241373_zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section52444064"></a>**

下载证书签名请求。

**命令格式<a name="zh-cn_topic_0000001562241373_zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section2234534"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/downloadCSRFile**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

无

**使用指南<a name="zh-cn_topic_0000001562241373_zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section46779599"></a>**

无

**使用实例<a name="zh-cn_topic_0000001562241373_zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section18363212"></a>**

请求样例：

```http
POST https://10.10.10.10//redfish/v1/Systems/SecurityService/downloadCSRFile
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：无（直接导出文件）

响应码：200

**输出说明<a name="zh-cn_topic_0000001562241373_zh-cn_topic_0000001129822431_zh-cn_topic_0178823257_section31051180"></a>**

直接下载文件

### 导入弱字典<a name="ZH-CN_TOPIC_0000001578449968"></a>

**命令功能<a name="zh-cn_topic_0000001447121489_zh-cn_topic_0000001129822427_section44345243"></a>**

导入弱字典。

> [!NOTE] 说明 
> 
> - 导入及导出弱字典时系统对大小写不敏感，只会保存对应小写字符串，设置密码时也将无视大小写进行弱字典匹配。
> - 若要保留系统弱口令配置，请先导出系统弱口令后，在导出的弱口令文件上新增弱口令。
> - 每条弱口令规则建议不超过30个字符。

**命令格式<a name="zh-cn_topic_0000001447121489_zh-cn_topic_0000001129822427_section63562868"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictImport**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "FileName":FileName,
    "Password":password
}
```

**请求参数<a name="zh-cn_topic_0000001447121489_zh-cn_topic_0000001129822427_section35194905"></a>**

**表 1**  参数说明

|参数|是否必选|参数说明|取值|
|--|--|--|--|
|FileName|必选|导入弱字典的文件名。|字符串，长度为1~255，可由大小写字母（a\~z、A\~Z）、数字（0\~9）、其他字符（_.-）组成且不含“..”，后缀必须为“.conf”。|
|Password|必选|用户密码。|字符串，边缘管理系统用户对应的密码。长度为8~20字符。|

**使用指南<a name="zh-cn_topic_0000001447121489_zh-cn_topic_0000001129822427_section48318689"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447121489_zh-cn_topic_0000001129822427_section32215023"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictImport
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "FileName":"import.conf",
    "Password":"password"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Import puny dict successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001447121489_zh-cn_topic_0000001129822427_section21499755"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性。支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 导出弱字典<a name="ZH-CN_TOPIC_0000001628849933"></a>

**命令功能<a name="zh-cn_topic_0000001515758689_section15454135611582"></a>**

导出弱字典。

>  [!NOTE] 说明 
> 导入及导出弱字典时系统对大小写不敏感，只会保存对应小写字符串，设置密码时也将无视大小写进行弱字典匹配。

**命令格式<a name="zh-cn_topic_0000001515758689_section3891161718597"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictExport**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001515758689_section94881404014"></a>**

无

**使用实例<a name="zh-cn_topic_0000001515758689_section15503695014"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictExport
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：无（直接导出文件）

响应码：200

### 删除弱字典<a name="ZH-CN_TOPIC_0000001628610557"></a>

**命令功能<a name="zh-cn_topic_0000001464798952_section1489016194213"></a>**

删除弱字典。

**命令格式<a name="zh-cn_topic_0000001464798952_section62286343212"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictDelete**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Password":password
}
```

**请求参数<a name="zh-cn_topic_0000001464798952_section145089571828"></a>**

**表 1**  参数说明

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|Password|必选|用户密码|字符串，边缘管理系统的系统用户对应的密码。长度为8~20字符。|

**使用指南<a name="zh-cn_topic_0000001464798952_section201453615317"></a>**

无

**使用实例<a name="zh-cn_topic_0000001464798952_section155201143318"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictDelete
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Password":"password"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Delete puny dict successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001464798952_section167845293311"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性。支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 查询证书有效期提醒时间<a name="ZH-CN_TOPIC_0000001578449964"></a>

**命令功能<a name="zh-cn_topic_0000001446921665_zh-cn_topic_0000001129668049_zh-cn_topic_0215269327_section1098281717238"></a>**

查询证书有效期提醒时间。

**命令格式<a name="zh-cn_topic_0000001446921665_zh-cn_topic_0000001129668049_zh-cn_topic_0215269327_section4982171762320"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001446921665_zh-cn_topic_0000001129668049_zh-cn_topic_0215269327_section1799141742313"></a>**

无

**使用实例<a name="zh-cn_topic_0000001446921665_zh-cn_topic_0000001129668049_zh-cn_topic_0215269327_section179919176237"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#EdgeSystem/SecurityService/$entity",
    "@odata.id": "/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime",
    "@odata.type": "#MindXEdgeSecurityService.v1_0_0.MindXEdgeSecurityService",
    "CertAlarmTime": 100
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001446921665_zh-cn_topic_0000001129668049_zh-cn_topic_0215269327_section399613177238"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|SSL证书资源模型的OData描述信息。|
|@odata.id|字符串|SSL证书资源节点的访问路径。|
|@odata.type|字符串|SSL证书资源类型。|
|CertAlarmTime|数字|证书有效期提醒时间（单位/天）。|

### 修改证书有效期提醒时间<a name="ZH-CN_TOPIC_0000001628610573"></a>

**命令功能<a name="zh-cn_topic_0000001396761850_zh-cn_topic_0000001129563527_zh-cn_topic_0215269328_section174001434132316"></a>**

修改证书有效期提醒时间。

**命令格式<a name="zh-cn_topic_0000001396761850_zh-cn_topic_0000001129563527_zh-cn_topic_0215269328_section940083419231"></a>**

操作类型：**PATCH**

**URL：https://**_device\_ip_**/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime**

请求头：

```http
X-Auth-Token: auth_value
Content-Type: header_type
```

请求消息体：

```json
{
    "CertAlarmTime":days,
    "Password": password
}
```

**请求参数<a name="zh-cn_topic_0000001396761850_zh-cn_topic_0000001129563527_zh-cn_topic_0215269328_section74104344239"></a>**

**表 1**  参数说明

|参数名|是否必选|参数说明|取值|
|--|--|--|--|
|CertAlarmTime|必选|证书有效期提醒时间|数字，取值范围7~180。默认值为10天。|
|Password|必选|当前用户密码|字符串，长度为8~20字符。|

**使用指南<a name="zh-cn_topic_0000001396761850_zh-cn_topic_0000001129563527_zh-cn_topic_0215269328_section342663416236"></a>**

无

**使用实例<a name="zh-cn_topic_0000001396761850_zh-cn_topic_0000001129563527_zh-cn_topic_0215269328_section1842716341239"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime
```

请求头：

```http
X-Auth-Token: auth_value
Content-Type: application/json
```

请求消息体：

```json
{
    "CertAlarmTime":100,
    "Password":"password"
}
```

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#EdgeSystem/SecurityService/$entity",
    "@odata.id": "/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime",
    "@odata.type": "#MindXEdgeSecurityService.v1_0_0.MindXEdgeSecurityService",
    "CertAlarmTime": 100
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001396761850_zh-cn_topic_0000001129563527_zh-cn_topic_0215269328_section15436143419232"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|SSL证书资源模型的OData描述信息。|
|@odata.id|字符串|SSL证书资源节点的访问路径。|
|@odata.type|字符串|SSL证书资源类型。|
|CertAlarmTime|数字|证书有效期提醒时间（单位/天）。|

### 查询登录规则信息<a name="ZH-CN_TOPIC_0000001578449920"></a>

**命令功能<a name="zh-cn_topic_0000001446921617_zh-cn_topic_0000001082477672_section38411947202213"></a>**

查询登录规则信息。

**命令格式<a name="zh-cn_topic_0000001446921617_zh-cn_topic_0000001082477672_section7843447182213"></a>**

操作类型：**GET**

**URL**：**https://**_device\_ip_**/redfish/v1/Systems/SecurityService/SecurityLoad**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001446921617_zh-cn_topic_0000001082477672_section5885124712214"></a>**

无

**使用实例<a name="zh-cn_topic_0000001446921617_zh-cn_topic_0000001082477672_section188734722210"></a>**

请求样例：

```http
GET https://10.10.10.10/redfish/v1/Systems/SecurityService/SecurityLoad
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：

```json
{
    "@odata.context": "/redfish/v1/$metadata#Systems/SecurityService/SecurityLoad",
    "@odata.id": "/redfish/v1/Systems/SecurityService/SecurityLoad",
    "@odata.type": "#MindXEdgeSecurityService.v1_0_0.MindXEdgeSecurityService",
    "Id": "SecurityLoad",
    "Name": "Security Load",
    "load_cfg":[{
        "enable": "true",
        "start_time": "06:00",
        "end_time": "12:00",
        "ip_addr": "xx.xx.xx.xx",
        "mac_addr": "00:89:01:xx:xx:04"
    }],
    "Actions": {
        "#SecurityLoad.Import": {
            "target": "/redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Import"
  },
       "#SecurityLoad.Export": {
            "target": "/redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Export"
        }
    }
}
```

响应码：200

**输出说明<a name="zh-cn_topic_0000001446921617_zh-cn_topic_0000001082477672_section10906747142211"></a>**

**表 1**  操作输出说明

|字段|类型|说明|
|--|--|--|
|@odata.context|字符串|登录规则资源模型的OData描述信息。|
|@odata.id|字符串|登录规则资源的访问路径。|
|@odata.type|字符串|登录规则资源的类型。|
|Id|字符串|登录规则资源的ID。|
|Name|字符串|登录规则资源的名称。|
|load_cfg|列表|配置项列表。|
|enable|字符串|配置项是否使能。<li>true</li><li>false</li>|
|start_time|字符串|允许登录的时间起点。<br>24小时制，样例：“小时:分钟”</br>|
|end_time|字符串|允许登录的时间终点。<br>24小时制，样例：“小时:分钟”</br>|
|ip_addr|字符串|允许登录的IPv4地址或IPv4地址及掩码位数。|
|mac_addr|字符串|允许登录的MAC地址。|
|Actions|对象|可执行的操作。|
|Actions.#SecurityLoad.Import|对象|导入登录规则信息的资源路径。|
|Actions.#SecurityLoad.Export|对象|导出登录规则信息的资源路径。|

### 配置登录规则信息<a name="ZH-CN_TOPIC_0000001578489856"></a>

**命令功能<a name="zh-cn_topic_0000001396761866_zh-cn_topic_0000001082765796_section3762193715151"></a>**

配置登录规则信息。

**命令格式<a name="zh-cn_topic_0000001396761866_zh-cn_topic_0000001082765796_section7962142411160"></a>**

操作类型：**PATCH**

**URL：https://**_device\_ip_**/redfish/v1/Systems/SecurityService/SecurityLoad**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Password":XXX,
    "load_cfg":[{
        "enable":enable,
        "start_time": start_time,
        "end_time": end_time,
        "ip_addr": ip_addr,
        "mac_addr": mac_addr
    }]
}
```

**请求参数<a name="zh-cn_topic_0000001396761866_zh-cn_topic_0000001082765796_section121235531719"></a>**

**表 1**  参数说明

|参数|是否必选|参数说明|取值要求|
|--|--|--|--|
|Password|必选|用户密码。|字符串，长度为8~20字符。|
|load_cfg|必选|配置项列表。|列表，长度最大30。|
|enable|必选|配置项是否使能。|字符串，取值为true或false。|
|start_time|可选|允许登录的时间起点。|24小时制，样例：“小时:分钟”|
|end_time|可选|允许登录的时间终点。|24小时制，样例：“小时:分钟”|
|ip_addr|可选|允许登录的IP地址或IP地址及掩码位数。|字符串，IPv4地址，点分十进制。<br>掩码位数1~32。|
|mac_addr|可选|允许登录的MAC地址。|字符串，只支持单播地址。允许只配置单播地址高24位。|

**使用指南<a name="zh-cn_topic_0000001396761866_zh-cn_topic_0000001082765796_section1012415091720"></a>**

无

**使用实例<a name="zh-cn_topic_0000001396761866_zh-cn_topic_0000001082765796_section1750213481811"></a>**

请求样例：

```http
PATCH https://10.10.10.10/redfish/v1/Systems/SecurityLoad
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Password": "password",
    "load_cfg":[{
        "enable": "true",
        "start_time": "06:00",
        "end_time": "12:00",
        "ip_addr": "xx.xx.xx.xx",
        "mac_addr": "00:89:01:xx:xx:04"
    }]
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.Success",
        "message": "Operation success. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Config security load successfully.",
                "Severity": "OK",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001396761866_zh-cn_topic_0000001082765796_section97486268267"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性。支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 导入登录规则信息<a name="ZH-CN_TOPIC_0000001628490577"></a>

**命令功能<a name="zh-cn_topic_0000001447121533_zh-cn_topic_0000001082925750_section1030202217304"></a>**

导入登录规则信息。

**命令格式<a name="zh-cn_topic_0000001447121533_zh-cn_topic_0000001082925750_section156141741133017"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_/**redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Import**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Password": password,
    "file_name": file_name
}
```

**请求参数<a name="zh-cn_topic_0000001447121533_zh-cn_topic_0000001082925750_section174943505333"></a>**

**表 1**  参数说明

|参数|是否必选|参数说明|取值|
|--|--|--|--|
|Password|必选|用户密码|字符串，长度为8~20字符。|
|file_name|必选|上传文件的文件名|字符串，长度为1\~255，可由大小写字母（a\~z、A\~Z）、数字（0\~9）和其他字符（_.-）组成，且不能包含“..”且后缀为“ini”。|

**使用指南<a name="zh-cn_topic_0000001447121533_zh-cn_topic_0000001082925750_section81561537153412"></a>**

无

**使用实例<a name="zh-cn_topic_0000001447121533_zh-cn_topic_0000001082925750_section151485433517"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Import
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：

```json
{
    "Password": "password",
    "file_name": "session_cfg.ini"
}
```

响应样例：

```json
{
    "error": {
        "code": "Base.1.0.GeneralError",
        "message": "A GeneralError has occurred. See ExtendedInfo for more information.",
        "@Message.ExtendedInfo": [
            {
                "@odata.type": "#MessageRegistry.v1_0_0.MessageRegistry",
                "Description": "Indicates that no error has occurred.",
                "Message": "Import configuration of security load successfully.",
                "Severity": "Critical",
                "NumberOfArgs": null,
                "ParamTypes": null,
                "Resolution": "None"
            }
        ]
    }
}
```

响应码：202

**输出说明<a name="zh-cn_topic_0000001447121533_zh-cn_topic_0000001082925750_section176060323812"></a>**

**表 2**  操作输出说明

|字段|类型|说明|
|--|--|--|
|code|字符串|指示消息注册表中特定消息ID的字符串。|
|message|字符串|与消息注册表中的消息对应的易读的消息。|
|@odata.type|字符串|消息资源的OData描述信息。|
|Description|字符串|消息资源的具体描述。|
|Message|字符串|消息资源的详细信息。|
|Severity|字符串|严重性。支持的严重级别包括：<li>OK</li><li>Warning</li><li>Critical</li>|
|NumberOfArgs|数字|消息描述的参数个数。|
|ParamTypes|数组|参数类型列表。|
|Resolution|字符串|事件处理建议。|

### 导出登录规则信息<a name="ZH-CN_TOPIC_0000001578489892"></a>

**命令功能<a name="zh-cn_topic_0000001397081546_zh-cn_topic_0000001129385273_section994114214393"></a>**

导出登录规则信息。

**命令格式<a name="zh-cn_topic_0000001397081546_zh-cn_topic_0000001129385273_section857313188395"></a>**

操作类型：**POST**

**URL**：**https://**_device\_ip_/**redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Export**

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

**使用指南<a name="zh-cn_topic_0000001397081546_zh-cn_topic_0000001129385273_section1237136174013"></a>**

无

**使用实例<a name="zh-cn_topic_0000001397081546_zh-cn_topic_0000001129385273_section8134155311401"></a>**

请求样例：

```http
POST https://10.10.10.10/redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Export
```

请求头：

```http
X-Auth-Token: auth_value
```

请求消息体：无

响应样例：无（直接导出文件）

响应码：200

## Redfish资源树<a name="ZH-CN_TOPIC_0000001578489872"></a>

Redfish各资源允许的操作如[表1 Redfish资源树](#Redfish资源树table)所示。

**表 1**  Redfish资源树<a id="Redfish资源树table"></a>

|编号|URL|允许操作|
|--|--|--|
|1|/redfish|GET|
|2|/redfish/v1|GET|
|3|/redfish/v1/$metadata|GET|
|4|/redfish/v1/JSONSchemas|GET|
|5|/redfish/v1/JSONSchemas/*<member_id*>|GET|
|6|/redfish/v1/odata|GET|
|7|/redfish/v1/AccountService/Accounts/*<member_id*>|GET/PATCH|
|8|/redfish/v1/AccountService/Accounts|GET|
|9|/redfish/v1/AccountService|GET/PATCH|
|10|/redfish/v1/SessionService|GET/PATCH|
|11|/redfish/v1/SessionService/Sessions|POST|
|12|/redfish/v1/SessionService/Sessions/<<i>session_id</i>>|DELETE|
|13|/redfish/v1/UpdateService|GET|
|14|/redfish/v1/UpdateService/Actions/UpdateService.SimpleUpdate|GET/POST|
|15|/redfish/v1/UpdateService/Actions/UpdateService.Reset|POST|
|16|/redfish/v1/UpdateService/FirmwareInventory|POST|
|17|/redfish/v1/Systems|GET/PATCH|
|18|/redfish/v1/Systems/SystemTime|GET|
|19|/redfish/v1/Systems/Actions/ComputerSystem.Reset|POST|
|20|/redfish/v1/Systems/Processors|GET|
|21|/redfish/v1/Systems/Processors/CPU|GET|
|22|/redfish/v1/Systems/NTPService|GET/PATCH|
|23|/redfish/v1/Systems/ExtendedDevices|GET|
|24|/redfish/v1/Systems/ExtendedDevices/*<extend_id>*|GET|
|25|/redfish/v1/Systems/Processors/AiProcessor|GET|
|26|/redfish/v1/Systems/LTE|GET|
|27|/redfish/v1/Systems/Memory|GET|
|28|/redfish/v1/Systems/LTE/StatusInfo|GET/PATCH|
|29|/redfish/v1/Systems/LTE/ConfigInfo|GET/PATCH|
|30|/redfish/v1/Systems/EthernetInterfaces|GET|
|31|/redfish/v1/Systems/EthernetInterfaces/<i><eth_id></i>|GET/PATCH|
|32|/redfish/v1/Systems/SimpleStorages|GET|
|33|/redfish/v1/Systems/SimpleStorages/*<storage_id*>|GET|
|34|/redfish/v1/Systems/Partitions|GET/POST|
|35|/redfish/v1/Systems/Partitions/*<partition_id*>|GET/DELETE|
|36|/redfish/v1/Systems/Partitions/Mount|PATCH|
|37|/redfish/v1/Systems/Partitions/Unmount|PATCH|
|38|/redfish/v1/Systems/NfsManage|GET|
|39|/redfish/v1/Systems/NfsManage/Actions/NfsManage.Mount|POST|
|40|/redfish/v1/Systems/NfsManage/Actions/NfsManage.Unmount|POST|
|41|/redfish/v1/Systems/SecurityService/HttpsCert|GET|
|42|/redfish/v1/Systems/SecurityService/HttpsCert/Actions/HttpsCert.ImportServerCertificate|POST|
|43|/redfish/v1/Systems/SecurityService/downloadCSRFile|POST|
|44|/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictImport|POST|
|45|/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictExport|POST|
|46|/redfish/v1/Systems/SecurityService/Actions/SecurityService.PunyDictDelete|POST|
|47|/redfish/v1/Systems/LogServices|GET|
|48|/redfish/v1/Systems/LogServices/Actions/download|POST|
|49|/redfish/v1/Systems/LogServices/progress|GET|
|50|/redfish/v1/Systems/Alarm/AlarmInfo|GET|
|51|/redfish/v1/Systems/Actions/UpdateService.Reset|POST|
|52|/redfish/v1/Systems/Alarm|GET|
|53|/redfish/v1/Systems/Alarm/AlarmShield|GET/PATCH|
|54|/redfish/v1/Systems/SecurityService|GET|
|55|/redfish/v1/Systems/SecurityService/HttpsCert|GET|
|56|/redfish/v1/Systems/SecurityService/HttpsCertAlarmTime|GET/PATCH|
|57|/redfish/v1/Systems/EthIpList|GET|
|58|/redfish/v1/Systems/Modules|GET|
|59|/redfish/v1/Systems/Modules/*<module_id>*|GET|
|60|/redfish/v1/Systems/Modules/*<module_id>/<device_id>*|GET/PATCH|
|61|/redfish/v1/Systems/Actions/RestoreDefaults.Reset|POST|
|62|/redfish/v1/NetManager|GET/POST|
|63|/redfish/v1/NetManager/NodeID|GET|
|64|/redfish/v1/NetManager/ImportFdCert|POST|
|65|/redfish/v1/NetManager/ImportFdCrl|POST|
|66|/redfish/v1/NetManager/QueryFdCert|GET|
|67|/redfish/v1/Systems/SecurityService/SecurityLoad|GET|
|68|/redfish/v1/Systems/SecurityService/SecurityLoad|PATCH|
|69|/redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Import|POST|
|70|/redfish/v1/Systems/SecurityService/SecurityLoad/Actions/SecurityLoad.Export|POST|
