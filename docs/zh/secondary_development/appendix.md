# 附录<a name="ZH-CN_TOPIC_0000001577530716"></a>

## 产品规格配置文件样例<a id="产品规格配置文件样例"></a>

产品规格配置文件是om-sdk.tar.gz中software/ibma/config/devm\_configs下的product\_specification.json文件。

```json
{
    "name": "Atlas 500",
    "modules": {
        "npu": {
            "devices": ["davinci0"]
        },
        "cpu": {
            "devices": ["cpu0"]
        },
        "mainboard": {
            "devices": ["mainboard0"]
        },
        "system": {
            "devices": ["system0"]
        },
        "A200": {
            "devices": ["Atlas 200"]
        },
        "ethnet": {
            "devices": ["eth0", "eth1"]
        },
        "disk": {
            "devices": ["disk0"]
        },
        "lte": {
            "devices": ["LTE-module"]
        },
        "wifi": {
            "devices": ["WIFI-module"]
        },
        "emmc": {
            "devices": []
        },
        "u-disk": {
            "devices": []
        },
        "demo": {
            "devices": ["extend_device01", "extend_device02"]
        }
    }
}
```

## 模组规格配置文件样例<a id="模组规格配置文件样例"></a>

模组规格配置文件需要开发者在_\{project\_dir\}_/config/module\_def目录下新建module\_demo.json文件。

```json
{
    "id": 1,
    "name": "demo",
    "category": "addition",
    "driver": "/usr/local/mindx/MindXOM/lib/libdemo_adapter.so",
    "dynamic": true,
    "attributes": {
        "name": {
            "description": "device name",
            "type": "string",
            "id": 1,
            "accessMode": "Read"
        },
        "class": {
            "description": "device class",
            "type": "string",
            "id": 2,
            "accessMode": "Read"
        },
        "present": {
            "description": "device present",
            "type": "int",
            "id": 65537,
            "accessMode": "ReadWrite"
        },
        "temperature": {
            "description": "device temperature",
            "type": "float",
            "id": 65538,
            "accessMode": "ReadWrite"
        },
        "voltage": {
            "description": "device voltage",
            "type": "float",
            "id": 65539,
            "accessMode": "ReadWrite"
        },
        "switch": {
            "description": "device switch",
            "type": "bool",
            "id": 65540,
            "accessMode": "ReadWrite"
        },
        "memory": {
            "description": "device memory",
            "type": "long long",
            "id": 65541,
            "accessMode": "ReadWrite"
        },
        "version": {
            "description": "device version",
            "type": "string",
            "id": 65542,
            "accessMode": "ReadWrite"
        },
        "signal": {
            "description": "device account info",
            "type": "json",
            "id": 65543,
            "accessMode": "ReadWrite",
            "subAttributes": {
                "signal_type": {
                    "accessMode": "ReadWrite",
                    "description": "signal_type",
                    "type": "string",
                    "id": 1
                },
                "signal_strength": {
                    "accessMode": "ReadWrite",
                    "description": "signal_strength",
                    "type": "string",
                    "id": 2
                }
            }
        }
    }
}
```

## OM SDK预留的告警配置<a id="ZH-CN_TOPIC_0000001635122545"></a>

下表中的告警信息包括OM SDK已经实现的告警和暂未实现的预留告警，开发者可以自行实现预留的告警。

**表 1**  OM SDK预留的告警信息<a id="om-sdk预留的告警配置table"></a>

<a name="table196690915513"></a>
<table><thead align="left"><tr id="row1767039185514"><th class="cellrowborder" valign="top" width="7.5359202424005485%" id="mcps1.2.8.1.1"><p id="p116705935519"><a name="p116705935519"></a><a name="p116705935519"></a>告警类型</p>
</th>
<th class="cellrowborder" valign="top" width="9.725344541100577%" id="mcps1.2.8.1.2"><p id="p156701494556"><a name="p156701494556"></a><a name="p156701494556"></a>告警ID</p>
</th>
<th class="cellrowborder" valign="top" width="13.94780568859349%" id="mcps1.2.8.1.3"><p id="p8670129165518"><a name="p8670129165518"></a><a name="p8670129165518"></a>告警名称</p>
</th>
<th class="cellrowborder" valign="top" width="7.682533476688497%" id="mcps1.2.8.1.4"><p id="p175741544102620"><a name="p175741544102620"></a><a name="p175741544102620"></a>是否实现</p>
</th>
<th class="cellrowborder" valign="top" width="10.741862965497019%" id="mcps1.2.8.1.5"><p id="p1867018995515"><a name="p1867018995515"></a><a name="p1867018995515"></a>告警对象</p>
</th>
<th class="cellrowborder" valign="top" width="7.027661030202327%" id="mcps1.2.8.1.6"><p id="p18670199135513"><a name="p18670199135513"></a><a name="p18670199135513"></a>告警等级</p>
</th>
<th class="cellrowborder" valign="top" width="43.338872055517555%" id="mcps1.2.8.1.7"><p id="p1767099195514"><a name="p1767099195514"></a><a name="p1767099195514"></a>告警描述</p>
</th>
</tr>
</thead>
<tbody><tr id="row46701491557"><td class="cellrowborder" rowspan="2" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p1273591514216"><a name="p1273591514216"></a><a name="p1273591514216"></a>温度告警</p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p16704915550"><a name="p16704915550"></a><a name="p16704915550"></a>0x00000000</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p16670393553"><a name="p16670393553"></a><a name="p16670393553"></a>硬盘温度过高</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p957410444267"><a name="p957410444267"></a><a name="p957410444267"></a>是</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p96701191556"><a name="p96701191556"></a><a name="p96701191556"></a>硬盘</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p1567015995510"><a name="p1567015995510"></a><a name="p1567015995510"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="p176701698550"><a name="p176701698550"></a><a name="p176701698550"></a>当硬盘温度高于一般告警阈值时，产生此告警；当温度恢复到正常范围内时，此告警消失。</p>
</td>
</tr>
<tr id="row16701905516"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p16670169175514"><a name="p16670169175514"></a><a name="p16670169175514"></a>0x000E003B</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p067015955516"><a name="p067015955516"></a><a name="p067015955516"></a>NPU温度检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p2574104411264"><a name="p2574104411264"></a><a name="p2574104411264"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p967159105514"><a name="p967159105514"></a><a name="p967159105514"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1671119185510"><a name="p1671119185510"></a><a name="p1671119185510"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0176114055_p9510154473511"><a name="zh-cn_topic_0176114055_p9510154473511"></a><a name="zh-cn_topic_0176114055_p9510154473511"></a>当NPU温度高于告警阈值时，产生此告警，当温度恢复到正常范围内时，此告警恢复。</p>
</td>
</tr>
<tr id="row66711291554"><td class="cellrowborder" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p66710995512"><a name="p66710995512"></a><a name="p66710995512"></a>电源告警</p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p1567113985513"><a name="p1567113985513"></a><a name="p1567113985513"></a>0x00110000</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p15671179175518"><a name="p15671179175518"></a><a name="p15671179175518"></a>RTC时钟电池电压过低</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p0574144482614"><a name="p0574144482614"></a><a name="p0574144482614"></a>否</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p66711495551"><a name="p66711495551"></a><a name="p66711495551"></a>RTC时钟</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p106711796550"><a name="p106711796550"></a><a name="p106711796550"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="zh-cn_topic_0176149423_p1175917962"><a name="zh-cn_topic_0176149423_p1175917962"></a><a name="zh-cn_topic_0176149423_p1175917962"></a>当RTC时钟电池电压低于1.66V时，产生此告警；当RTC电池电压恢复到3.0V时，此告警消失</p>
</td>
</tr>
<tr id="row1335019306131"><td class="cellrowborder" rowspan="17" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p75891129228"><a name="p75891129228"></a><a name="p75891129228"></a>存储告警</p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p173501530151317"><a name="p173501530151317"></a><a name="p173501530151317"></a>0x00000001</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p193504303135"><a name="p193504303135"></a><a name="p193504303135"></a>硬盘寿命到期预警</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p165741044182618"><a name="p165741044182618"></a><a name="p165741044182618"></a>是</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p163501630161320"><a name="p163501630161320"></a><a name="p163501630161320"></a>硬盘</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p135010300136"><a name="p135010300136"></a><a name="p135010300136"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="zh-cn_topic_0176113985_p1175917962"><a name="zh-cn_topic_0176113985_p1175917962"></a><a name="zh-cn_topic_0176113985_p1175917962"></a>当硬盘寿命即将到期时，产生此告警；当更换硬盘后，此告警消失。</p>
</td>
</tr>
<tr id="row657643381312"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p857617338136"><a name="p857617338136"></a><a name="p857617338136"></a>0x00000002</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p757693301313"><a name="p757693301313"></a><a name="p757693301313"></a>硬盘不在位</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p115741544152618"><a name="p115741544152618"></a><a name="p115741544152618"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p17576933131315"><a name="p17576933131315"></a><a name="p17576933131315"></a>硬盘</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p25761033131311"><a name="p25761033131311"></a><a name="p25761033131311"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0187057013_p9510154473511"><a name="zh-cn_topic_0187057013_p9510154473511"></a><a name="zh-cn_topic_0187057013_p9510154473511"></a>当检测不到硬盘在位信号时，产生此告警；当检测到硬盘在位时，此告警消失。</p>
</td>
</tr>
<tr id="row1640616372131"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p2407137181312"><a name="p2407137181312"></a><a name="p2407137181312"></a>0x00000003</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p740713713138"><a name="p740713713138"></a><a name="p740713713138"></a>硬盘访问阻塞</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p457413443269"><a name="p457413443269"></a><a name="p457413443269"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1040720375135"><a name="p1040720375135"></a><a name="p1040720375135"></a>硬盘</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p6407037131314"><a name="p6407037131314"></a><a name="p6407037131314"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0187057014_p9510154473511"><a name="zh-cn_topic_0187057014_p9510154473511"></a><a name="zh-cn_topic_0187057014_p9510154473511"></a>当访问硬盘无响应时，产生此告警；当更换硬盘后，此告警消失。</p>
</td>
</tr>
<tr id="row20661441161312"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p46715417130"><a name="p46715417130"></a><a name="p46715417130"></a>0x00000004</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p146794181313"><a name="p146794181313"></a><a name="p146794181313"></a>不稳定扇区数临界预警</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1357416448264"><a name="p1357416448264"></a><a name="p1357416448264"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p2671441161317"><a name="p2671441161317"></a><a name="p2671441161317"></a>硬盘</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1967194101315"><a name="p1967194101315"></a><a name="p1967194101315"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0207467396_p78512113911"><a name="zh-cn_topic_0207467396_p78512113911"></a><a name="zh-cn_topic_0207467396_p78512113911"></a>当硬盘扇区读取出现错误时，不稳定扇区计数会增加，增加到超过临界状态时产生告警；当更换硬盘后，此告警消失。</p>
</td>
</tr>
<tr id="row27361244131310"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p67361544141314"><a name="p67361544141314"></a><a name="p67361544141314"></a>0x00000005</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p20736444181316"><a name="p20736444181316"></a><a name="p20736444181316"></a>剩余备用扇区不足</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p05741044142619"><a name="p05741044142619"></a><a name="p05741044142619"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p373654417137"><a name="p373654417137"></a><a name="p373654417137"></a>硬盘</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1273618448132"><a name="p1273618448132"></a><a name="p1273618448132"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0207467397_p19546124324416"><a name="zh-cn_topic_0207467397_p19546124324416"></a><a name="zh-cn_topic_0207467397_p19546124324416"></a>当剩余备用扇区数量接近或已达到临界值时，将产生该告警；当更换硬盘后，此告警消失。</p>
</td>
</tr>
<tr id="row221004818133"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p12101848131317"><a name="p12101848131317"></a><a name="p12101848131317"></a>0x00030000</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p2210948141310"><a name="p2210948141310"></a><a name="p2210948141310"></a>eMMC寿命到期预警</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p65742447266"><a name="p65742447266"></a><a name="p65742447266"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1821014810137"><a name="p1821014810137"></a><a name="p1821014810137"></a>eMMC</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p17210184871315"><a name="p17210184871315"></a><a name="p17210184871315"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p62101848161311"><a name="p62101848161311"></a><a name="p62101848161311"></a>eMMC寿命即将到期。</p>
</td>
</tr>
<tr id="row199855119137"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p169981851191317"><a name="p169981851191317"></a><a name="p169981851191317"></a>0x00030001</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p0998115131314"><a name="p0998115131314"></a><a name="p0998115131314"></a>eMMC平均写入量超标</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p2057418440261"><a name="p2057418440261"></a><a name="p2057418440261"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p899895131313"><a name="p899895131313"></a><a name="p899895131313"></a>eMMC</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p0998351131313"><a name="p0998351131313"></a><a name="p0998351131313"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p13998115112138"><a name="p13998115112138"></a><a name="p13998115112138"></a>当eMMC连续三天的平均写入量超标时（52GB），会产生此告警；当平均写入量不超过告警门限时，此告警消失。</p>
</td>
</tr>
<tr id="row16602955171317"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p060212556137"><a name="p060212556137"></a><a name="p060212556137"></a>0x00030002</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p19602125511313"><a name="p19602125511313"></a><a name="p19602125511313"></a>eMMC每日写入量超标</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p057514422611"><a name="p057514422611"></a><a name="p057514422611"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1260220558130"><a name="p1260220558130"></a><a name="p1260220558130"></a>eMMC</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p86025557138"><a name="p86025557138"></a><a name="p86025557138"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0197270247_p17534151155418"><a name="zh-cn_topic_0197270247_p17534151155418"></a><a name="zh-cn_topic_0197270247_p17534151155418"></a>当eMMC每日写入量超标时（52GB），会产生此告警；当每日写入量不超过告警门限时，此告警消失。</p>
</td>
</tr>
<tr id="row21053593131"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p11105175914132"><a name="p11105175914132"></a><a name="p11105175914132"></a>0x00030003</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1810511593131"><a name="p1810511593131"></a><a name="p1810511593131"></a>eMMC可用预留块不足</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1857544412262"><a name="p1857544412262"></a><a name="p1857544412262"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1210535921310"><a name="p1210535921310"></a><a name="p1210535921310"></a>eMMC</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p810619598133"><a name="p810619598133"></a><a name="p810619598133"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p191370204259"><a name="p191370204259"></a><a name="p191370204259"></a>当eMMC可用预留块不足时，会产生此告警；当可用预留块充足时，此告警消失。</p>
</td>
</tr>
<tr id="row78121533142"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p178127321419"><a name="p178127321419"></a><a name="p178127321419"></a>0x00040000</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p781218301420"><a name="p781218301420"></a><a name="p781218301420"></a>SD卡挂载失败</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1657574412619"><a name="p1657574412619"></a><a name="p1657574412619"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p98122311146"><a name="p98122311146"></a><a name="p98122311146"></a>SD</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p3812173191414"><a name="p3812173191414"></a><a name="p3812173191414"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1581213317147"><a name="p1581213317147"></a><a name="p1581213317147"></a>当SD卡挂载失败时，产生此告警；当SD卡挂载成功时，此告警消失。</p>
</td>
</tr>
<tr id="row956912286258"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p12570142817251"><a name="p12570142817251"></a><a name="p12570142817251"></a>0x00040001</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p35701228172512"><a name="p35701228172512"></a><a name="p35701228172512"></a>SD卡寿命到期预警</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p12575194416260"><a name="p12575194416260"></a><a name="p12575194416260"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p6570428142511"><a name="p6570428142511"></a><a name="p6570428142511"></a>SD</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p125704287252"><a name="p125704287252"></a><a name="p125704287252"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p11570162815255"><a name="p11570162815255"></a><a name="p11570162815255"></a>当SD寿命即将到期时，产生此告警；当更换SD卡后，此告警消失。</p>
</td>
</tr>
<tr id="row195391731112516"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p11540123115254"><a name="p11540123115254"></a><a name="p11540123115254"></a>0x00120000</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p354073152518"><a name="p354073152518"></a><a name="p354073152518"></a>存储设备挂载配置异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p205751444172617"><a name="p205751444172617"></a><a name="p205751444172617"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p054015312257"><a name="p054015312257"></a><a name="p054015312257"></a>eMMC/SD/USB/HDD</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p05405317256"><a name="p05405317256"></a><a name="p05405317256"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p10540031162517"><a name="p10540031162517"></a><a name="p10540031162517"></a>当MNT设备的挂载配置文件错误时，产生此告警；当MNT设备挂载配置文件恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1193334102511"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p31931934182512"><a name="p31931934182512"></a><a name="p31931934182512"></a>0x00120001</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p6194193410253"><a name="p6194193410253"></a><a name="p6194193410253"></a>存储设备分区丢失</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p65757445266"><a name="p65757445266"></a><a name="p65757445266"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1919418344257"><a name="p1919418344257"></a><a name="p1919418344257"></a>eMMC/SD/USB/HDD</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p3194334192516"><a name="p3194334192516"></a><a name="p3194334192516"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p12194203492518"><a name="p12194203492518"></a><a name="p12194203492518"></a>当MNT外接存储设备的分区丢失时，产生此告警；当外接存储设备的分区恢复到正常情况时，此告警消失。</p>
</td>
</tr>
<tr id="row14963123614256"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1696413361250"><a name="p1696413361250"></a><a name="p1696413361250"></a>0x00120002</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p119641336162518"><a name="p119641336162518"></a><a name="p119641336162518"></a>存储设备丢失</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p45751443267"><a name="p45751443267"></a><a name="p45751443267"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1964236152517"><a name="p1964236152517"></a><a name="p1964236152517"></a>eMMC/SD/USB/HDD</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p6964736172518"><a name="p6964736172518"></a><a name="p6964736172518"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p12964113652520"><a name="p12964113652520"></a><a name="p12964113652520"></a>当MNT外接存储设备丢失时，产生此告警；当外接存储设备恢复到正常情况时，此告警消失。</p>
</td>
</tr>
<tr id="row127134042515"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p142721540112517"><a name="p142721540112517"></a><a name="p142721540112517"></a>0x00120003</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p12272174082514"><a name="p12272174082514"></a><a name="p12272174082514"></a>存储设备挂载失败</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p7575644182610"><a name="p7575644182610"></a><a name="p7575644182610"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p8272440182517"><a name="p8272440182517"></a><a name="p8272440182517"></a>eMMC/SD/USB/HDD</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p19272154082510"><a name="p19272154082510"></a><a name="p19272154082510"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p927274012256"><a name="p927274012256"></a><a name="p927274012256"></a>当MNT外接存储设备挂载失败时，产生此告警；当外接存储设备挂载正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1749813504257"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1649965082515"><a name="p1649965082515"></a><a name="p1649965082515"></a>0x00120004</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p19499185010251"><a name="p19499185010251"></a><a name="p19499185010251"></a>存储设备挂载点异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p195758445264"><a name="p195758445264"></a><a name="p195758445264"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1449925082513"><a name="p1449925082513"></a><a name="p1449925082513"></a>MNT</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p9499145015252"><a name="p9499145015252"></a><a name="p9499145015252"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1749925010255"><a name="p1749925010255"></a><a name="p1749925010255"></a>当MNT外接设备的挂接点错误时，产生此告警；当外接设备的挂接点正常时，此告警消失。</p>
</td>
</tr>
<tr id="row77851353132515"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p678515533250"><a name="p678515533250"></a><a name="p678515533250"></a>0x00160000</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1378665310256"><a name="p1378665310256"></a><a name="p1378665310256"></a>目录空间满</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1857517449260"><a name="p1857517449260"></a><a name="p1857517449260"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p2786155362519"><a name="p2786155362519"></a><a name="p2786155362519"></a>eMMC/DDR</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1078645316254"><a name="p1078645316254"></a><a name="p1078645316254"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p19786753132517"><a name="p19786753132517"></a><a name="p19786753132517"></a>当该目录占用率超过85%时，会产生此告警；当占用率小于80%时，此告警消失。</p>
</td>
</tr>
<tr id="row18125113153017"><td class="cellrowborder" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p17125193173014"><a name="p17125193173014"></a><a name="p17125193173014"></a>NFS告警</p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p31251537303"><a name="p31251537303"></a><a name="p31251537303"></a>0x00140000</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p1012519343019"><a name="p1012519343019"></a><a name="p1012519343019"></a>NFS异常</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p55755443267"><a name="p55755443267"></a><a name="p55755443267"></a>是</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p41258314308"><a name="p41258314308"></a><a name="p41258314308"></a>NFS模块</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p1412511393017"><a name="p1412511393017"></a><a name="p1412511393017"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="p1112518373017"><a name="p1112518373017"></a><a name="p1112518373017"></a>当NFS挂载状态异常时，产生此告警；当NFS挂载状态正常时，此告警消失。</p>
</td>
</tr>
<tr id="row4629769308"><td class="cellrowborder" rowspan="4" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p197901241224"><a name="p197901241224"></a><a name="p197901241224"></a>端口告警</p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p762915616305"><a name="p762915616305"></a><a name="p762915616305"></a>0x00090000</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p1262915613019"><a name="p1262915613019"></a><a name="p1262915613019"></a>网口接收错包</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p85751444102620"><a name="p85751444102620"></a><a name="p85751444102620"></a>否</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p462915616303"><a name="p462915616303"></a><a name="p462915616303"></a>网口</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p136291660307"><a name="p136291660307"></a><a name="p136291660307"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="p1062919614304"><a name="p1062919614304"></a><a name="p1062919614304"></a>当网口收到CRC错包时，产生此告警；当网口24小时未收到CRC错包时，此告警消失。</p>
</td>
</tr>
<tr id="row3675596307"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p4675179183018"><a name="p4675179183018"></a><a name="p4675179183018"></a>0x00090001</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p367516917300"><a name="p367516917300"></a><a name="p367516917300"></a>网口发送错包</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p957584412268"><a name="p957584412268"></a><a name="p957584412268"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p10675169123012"><a name="p10675169123012"></a><a name="p10675169123012"></a>网口</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p15675493309"><a name="p15675493309"></a><a name="p15675493309"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1767518911308"><a name="p1767518911308"></a><a name="p1767518911308"></a>当网口发送CRC错包时，产生此告警；当网口24小时未检测到发送CRC错包时，此告警消失。</p>
</td>
</tr>
<tr id="row2903141273011"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p11903121253012"><a name="p11903121253012"></a><a name="p11903121253012"></a>0x00090002</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p89031412163015"><a name="p89031412163015"></a><a name="p89031412163015"></a>网口链路断开</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1157534422612"><a name="p1157534422612"></a><a name="p1157534422612"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1290391217302"><a name="p1290391217302"></a><a name="p1290391217302"></a>网口</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p13903412163015"><a name="p13903412163015"></a><a name="p13903412163015"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p11903812203011"><a name="p11903812203011"></a><a name="p11903812203011"></a>当网口link状态由up变成down时，产生此告警；当网口link状态显示up时，此告警消失。</p>
</td>
</tr>
<tr id="row4713121533018"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p971311151304"><a name="p971311151304"></a><a name="p971311151304"></a>0x00090003</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p16713181553013"><a name="p16713181553013"></a><a name="p16713181553013"></a>网口设备故障</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p18575144182610"><a name="p18575144182610"></a><a name="p18575144182610"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p19713141523020"><a name="p19713141523020"></a><a name="p19713141523020"></a>网口</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p167131415183017"><a name="p167131415183017"></a><a name="p167131415183017"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p12713715183016"><a name="p12713715183016"></a><a name="p12713715183016"></a>当网口无法正常访问时，产生此告警；当网口可正常访问时，此告警消失。</p>
</td>
</tr>
<tr id="row1395212211302"><td class="cellrowborder" rowspan="58" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p17181330143716"><a name="p17181330143716"></a><a name="p17181330143716"></a>NPU告警</p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p14952172153011"><a name="p14952172153011"></a><a name="p14952172153011"></a>0x000E0000</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p63005300118"><a name="p63005300118"></a><a name="p63005300118"></a>TEEDrv硬件模块错误</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p7575114414261"><a name="p7575114414261"></a><a name="p7575114414261"></a>是</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p095212173015"><a name="p095212173015"></a><a name="p095212173015"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p495217219306"><a name="p495217219306"></a><a name="p495217219306"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="zh-cn_topic_0208481120_zh-cn_topic_0176114036_p1175917962"><a name="zh-cn_topic_0208481120_zh-cn_topic_0176114036_p1175917962"></a><a name="zh-cn_topic_0208481120_zh-cn_topic_0176114036_p1175917962"></a>当TEEDrv硬件模块错误时，产生此告警；当TEEDrv硬件模块恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row497442419307"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p17974112411302"><a name="p17974112411302"></a><a name="p17974112411302"></a>0x000E0001</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p159741248307"><a name="p159741248307"></a><a name="p159741248307"></a>TEEDrv侧硬件多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1957574412267"><a name="p1957574412267"></a><a name="p1957574412267"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p2097482411301"><a name="p2097482411301"></a><a name="p2097482411301"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p199749243302"><a name="p199749243302"></a><a name="p199749243302"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p697482483015"><a name="p697482483015"></a><a name="p697482483015"></a>当TEEDrv侧硬件多bit ECC错误时，产生此告警；当TEEDrv侧硬件恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row52741028113018"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p9274122833012"><a name="p9274122833012"></a><a name="p9274122833012"></a>0x000E0002</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p182741128183012"><a name="p182741128183012"></a><a name="p182741128183012"></a>TS心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p165759448261"><a name="p165759448261"></a><a name="p165759448261"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p102741028173014"><a name="p102741028173014"></a><a name="p102741028173014"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p17274182819304"><a name="p17274182819304"></a><a name="p17274182819304"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p5274202810301"><a name="p5274202810301"></a><a name="p5274202810301"></a>当TS心跳检测异常时，产生此告警；当TS心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row178926319306"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p12925272132"><a name="p12925272132"></a><a name="p12925272132"></a>0x000E0003</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p5892173118303"><a name="p5892173118303"></a><a name="p5892173118303"></a>TS多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p13575114418269"><a name="p13575114418269"></a><a name="p13575114418269"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p9893143115308"><a name="p9893143115308"></a><a name="p9893143115308"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p989318318304"><a name="p989318318304"></a><a name="p989318318304"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p17893123113304"><a name="p17893123113304"></a><a name="p17893123113304"></a>当TS多bit ECC错误时，产生此告警；当TS多bit ECC恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row16261352164910"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p13261852194919"><a name="p13261852194919"></a><a name="p13261852194919"></a>0x000E0004</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p13262205284914"><a name="p13262205284914"></a><a name="p13262205284914"></a>slogd心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1757534414266"><a name="p1757534414266"></a><a name="p1757534414266"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1726214525496"><a name="p1726214525496"></a><a name="p1726214525496"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p326275218499"><a name="p326275218499"></a><a name="p326275218499"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p02111498142"><a name="p02111498142"></a><a name="p02111498142"></a>当slogd心跳检测异常时，产生此告警；当slogd心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row19680175613497"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1668011561497"><a name="p1668011561497"></a><a name="p1668011561497"></a>0x000E0005</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p8680456134911"><a name="p8680456134911"></a><a name="p8680456134911"></a>dmp_daemon心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p2575104419267"><a name="p2575104419267"></a><a name="p2575104419267"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p068035612498"><a name="p068035612498"></a><a name="p068035612498"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p14680175614917"><a name="p14680175614917"></a><a name="p14680175614917"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p186801356194917"><a name="p186801356194917"></a><a name="p186801356194917"></a>当dmp_daemon心跳检测异常时，产生此告警；当dmp_daemon心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row858265911496"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p35821859124915"><a name="p35821859124915"></a><a name="p35821859124915"></a>0x000E0006</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p858218597490"><a name="p858218597490"></a><a name="p858218597490"></a>log-daemon心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p20575444112614"><a name="p20575444112614"></a><a name="p20575444112614"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p2582185919496"><a name="p2582185919496"></a><a name="p2582185919496"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p758255914919"><a name="p758255914919"></a><a name="p758255914919"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p35822059134912"><a name="p35822059134912"></a><a name="p35822059134912"></a>当log-daemon心跳检测异常时，产生此告警；当log-daemon心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row344819325010"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p544803145016"><a name="p544803145016"></a><a name="p544803145016"></a>0x000E0007</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p194481632501"><a name="p194481632501"></a><a name="p194481632501"></a>sklogd心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p8575134482612"><a name="p8575134482612"></a><a name="p8575134482612"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1448173145011"><a name="p1448173145011"></a><a name="p1448173145011"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1448193135010"><a name="p1448193135010"></a><a name="p1448193135010"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1444817310505"><a name="p1444817310505"></a><a name="p1444817310505"></a>当sklogd心跳检测异常时，产生此告警；当sklogd心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1866713610505"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p16688695016"><a name="p16688695016"></a><a name="p16688695016"></a>0x000E0008</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p2066819610500"><a name="p2066819610500"></a><a name="p2066819610500"></a>Iammgr心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p14575044202619"><a name="p14575044202619"></a><a name="p14575044202619"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p666896125014"><a name="p666896125014"></a><a name="p666896125014"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p11668461504"><a name="p11668461504"></a><a name="p11668461504"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p766813675016"><a name="p766813675016"></a><a name="p766813675016"></a>当Iammgr心跳检测异常时，产生此告警；当Iammgr心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row989771018502"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p5898121015500"><a name="p5898121015500"></a><a name="p5898121015500"></a>0x000E0009</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1489811107509"><a name="p1489811107509"></a><a name="p1489811107509"></a>ProcLauncher心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1257524412617"><a name="p1257524412617"></a><a name="p1257524412617"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p20898110205016"><a name="p20898110205016"></a><a name="p20898110205016"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p28983103509"><a name="p28983103509"></a><a name="p28983103509"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p141822161025"><a name="p141822161025"></a><a name="p141822161025"></a>当ProcLauncher心跳检测异常时，产生此告警；当ProcLauncher心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row179871595010"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p99815152507"><a name="p99815152507"></a><a name="p99815152507"></a>0x000E000A</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p129810151504"><a name="p129810151504"></a><a name="p129810151504"></a>ProcMgr心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p12575124412269"><a name="p12575124412269"></a><a name="p12575124412269"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p129813159502"><a name="p129813159502"></a><a name="p129813159502"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p129819158500"><a name="p129819158500"></a><a name="p129819158500"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p898121511504"><a name="p898121511504"></a><a name="p898121511504"></a>当ProcMgr心跳检测异常时，产生此告警；当ProcMgr心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1054905735218"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p191765315218"><a name="p191765315218"></a><a name="p191765315218"></a>0x000E000B</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1454911574521"><a name="p1454911574521"></a><a name="p1454911574521"></a>非致命高温异常告警</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p557624417261"><a name="p557624417261"></a><a name="p557624417261"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p25494570529"><a name="p25494570529"></a><a name="p25494570529"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p55491357205217"><a name="p55491357205217"></a><a name="p55491357205217"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p45491957165212"><a name="p45491957165212"></a><a name="p45491957165212"></a>当产生非致命高温异常告警时，产生此告警；当温度恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row199019116536"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p119901615531"><a name="p119901615531"></a><a name="p119901615531"></a>0x000E000C</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p799016195315"><a name="p799016195315"></a><a name="p799016195315"></a>LPM子系统心跳检测异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p357614482612"><a name="p357614482612"></a><a name="p357614482612"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1399011112535"><a name="p1399011112535"></a><a name="p1399011112535"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p29905119537"><a name="p29905119537"></a><a name="p29905119537"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1199015195320"><a name="p1199015195320"></a><a name="p1199015195320"></a>当LPM子系统心跳检测异常时，产生此告警；当LPM子系统心跳恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row950219618539"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p205023665320"><a name="p205023665320"></a><a name="p205023665320"></a>0x000E000D</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p12502186115311"><a name="p12502186115311"></a><a name="p12502186115311"></a>LPM检测到调压功能异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1257610441267"><a name="p1257610441267"></a><a name="p1257610441267"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p650210655318"><a name="p650210655318"></a><a name="p650210655318"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p145027616538"><a name="p145027616538"></a><a name="p145027616538"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p150214612532"><a name="p150214612532"></a><a name="p150214612532"></a>当LPM检测到调压功能异常时，产生此告警；当LPM检测到调压功能恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row12673810205319"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p146732010185314"><a name="p146732010185314"></a><a name="p146732010185314"></a>0x000E000E</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p156739100538"><a name="p156739100538"></a><a name="p156739100538"></a>LPM检测到调频功能异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p4576124432614"><a name="p4576124432614"></a><a name="p4576124432614"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p4673181025316"><a name="p4673181025316"></a><a name="p4673181025316"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1767331065318"><a name="p1767331065318"></a><a name="p1767331065318"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p12673191013532"><a name="p12673191013532"></a><a name="p12673191013532"></a>当LPM检测到调频功能异常时，产生此告警；当LPM检测到调频功能恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row616391410531"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p15163114205316"><a name="p15163114205316"></a><a name="p15163114205316"></a>0x000E000F</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p216391410538"><a name="p216391410538"></a><a name="p216391410538"></a>LPM检测到芯片电流检测功能异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p45761344142616"><a name="p45761344142616"></a><a name="p45761344142616"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1316351455310"><a name="p1316351455310"></a><a name="p1316351455310"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p151642143530"><a name="p151642143530"></a><a name="p151642143530"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p161648142537"><a name="p161648142537"></a><a name="p161648142537"></a>当LPM检测到芯片电流检测功能异常时，产生此告警；当LPM检测到芯片电流检测功能恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row189141945314"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p129919135312"><a name="p129919135312"></a><a name="p129919135312"></a>0x000E0010</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p9951919533"><a name="p9951919533"></a><a name="p9951919533"></a>LPM检测到Tsensor模块异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1457664452619"><a name="p1457664452619"></a><a name="p1457664452619"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p18910195534"><a name="p18910195534"></a><a name="p18910195534"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p199419135318"><a name="p199419135318"></a><a name="p199419135318"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p79201975313"><a name="p79201975313"></a><a name="p79201975313"></a>当LPM检测到Tsensor模块异常时，产生此告警；当LPM检测到Tsensor模块恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row149977216534"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p799718210532"><a name="p799718210532"></a><a name="p799718210532"></a>0x000E0011</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p299815212533"><a name="p299815212533"></a><a name="p299815212533"></a>LPM检测到pmbus模块异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1357624442615"><a name="p1357624442615"></a><a name="p1357624442615"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p3998721195319"><a name="p3998721195319"></a><a name="p3998721195319"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p17998121135318"><a name="p17998121135318"></a><a name="p17998121135318"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p799814214539"><a name="p799814214539"></a><a name="p799814214539"></a>当LPM检测到Tsensor模块异常时，产生此告警；当LPM检测到pmbus模块恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row152636597551"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1626375935515"><a name="p1626375935515"></a><a name="p1626375935515"></a>0x000E0012</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p726305975516"><a name="p726305975516"></a><a name="p726305975516"></a>AIC多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p557620448264"><a name="p557620448264"></a><a name="p557620448264"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p026312595559"><a name="p026312595559"></a><a name="p026312595559"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1826375917552"><a name="p1826375917552"></a><a name="p1826375917552"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p102634591555"><a name="p102634591555"></a><a name="p102634591555"></a>当产生AIC多bit ECC错误时，产生此告警；当AIC多bit ECC错误恢复正常时，此告警消失</p>
</td>
</tr>
<tr id="row1687351205612"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1887316115612"><a name="p1887316115612"></a><a name="p1887316115612"></a>0x000E0013</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p4873914564"><a name="p4873914564"></a><a name="p4873914564"></a>AIC检测到外部输入错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1257624417263"><a name="p1257624417263"></a><a name="p1257624417263"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1987331105616"><a name="p1987331105616"></a><a name="p1987331105616"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1287318135619"><a name="p1287318135619"></a><a name="p1287318135619"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p108737105619"><a name="p108737105619"></a><a name="p108737105619"></a>当AIC检测到外部输入错误时，产生此告警；当AIC检测到外部输入正常时，此告警消失。</p>
</td>
</tr>
<tr id="row158085518565"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p148088585612"><a name="p148088585612"></a><a name="p148088585612"></a>0x000E0014</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p118087519562"><a name="p118087519562"></a><a name="p118087519562"></a>AIC总线访问错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1457694452611"><a name="p1457694452611"></a><a name="p1457694452611"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p148081585618"><a name="p148081585618"></a><a name="p148081585618"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p580895105614"><a name="p580895105614"></a><a name="p580895105614"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p10808115185614"><a name="p10808115185614"></a><a name="p10808115185614"></a>当AIC总线存在访问错误时，产生此告警；当AIC总线访问恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row13859945618"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p15851999565"><a name="p15851999565"></a><a name="p15851999565"></a>0x000E0015</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1285099569"><a name="p1285099569"></a><a name="p1285099569"></a>AIC Dispatch多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p205761447269"><a name="p205761447269"></a><a name="p205761447269"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p18852918565"><a name="p18852918565"></a><a name="p18852918565"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p585179115612"><a name="p585179115612"></a><a name="p585179115612"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0208481164_zh-cn_topic_0176114009_p1175917962"><a name="zh-cn_topic_0208481164_zh-cn_topic_0176114009_p1175917962"></a><a name="zh-cn_topic_0208481164_zh-cn_topic_0176114009_p1175917962"></a>当AIC Dispatch存在多bit ECC错误时，产生此告警；当AIC Dispatch恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row10963511115615"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p3963811205611"><a name="p3963811205611"></a><a name="p3963811205611"></a>0x000E0016</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p10964811105615"><a name="p10964811105615"></a><a name="p10964811105615"></a>AIC Dispatch输入错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p25761144142611"><a name="p25761144142611"></a><a name="p25761144142611"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p2964201111568"><a name="p2964201111568"></a><a name="p2964201111568"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p296412115569"><a name="p296412115569"></a><a name="p296412115569"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p159641411145615"><a name="p159641411145615"></a><a name="p159641411145615"></a>当AIC Dispatch输入错误时，产生此告警；当AIC Dispatch输入恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1669916530576"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p7699953115718"><a name="p7699953115718"></a><a name="p7699953115718"></a>0x000E0017</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1769975365718"><a name="p1769975365718"></a><a name="p1769975365718"></a>AO Dispatch多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p9576344152620"><a name="p9576344152620"></a><a name="p9576344152620"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p4699175355716"><a name="p4699175355716"></a><a name="p4699175355716"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1169955313577"><a name="p1169955313577"></a><a name="p1169955313577"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p56991353165715"><a name="p56991353165715"></a><a name="p56991353165715"></a>当AO Dispatch存在多bit ECC错误时，产生此告警；当AO Dispatch多bit ECC错误恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row8862165795710"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p386215713572"><a name="p386215713572"></a><a name="p386215713572"></a>0x000E0018</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p13862357135716"><a name="p13862357135716"></a><a name="p13862357135716"></a>AO Dispatch输入错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p6576164452615"><a name="p6576164452615"></a><a name="p6576164452615"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p12862165716575"><a name="p12862165716575"></a><a name="p12862165716575"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p6862957185719"><a name="p6862957185719"></a><a name="p6862957185719"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p6862957195715"><a name="p6862957195715"></a><a name="p6862957195715"></a>当AO Dispatch存在输入错误时，产生此告警；当AO Dispatch输入恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row99871438145814"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p16987193835816"><a name="p16987193835816"></a><a name="p16987193835816"></a>0x000E0019</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1998713814589"><a name="p1998713814589"></a><a name="p1998713814589"></a>TaishanCore多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p14576154418261"><a name="p14576154418261"></a><a name="p14576154418261"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p11987638115819"><a name="p11987638115819"></a><a name="p11987638115819"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p6987438175814"><a name="p6987438175814"></a><a name="p6987438175814"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1198753865814"><a name="p1198753865814"></a><a name="p1198753865814"></a>当TaishanCore存在多bit ECC错误时，产生此告警；当TaishanCore恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row126571342205810"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p465754225817"><a name="p465754225817"></a><a name="p465754225817"></a>0x000E001A</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p8657194235819"><a name="p8657194235819"></a><a name="p8657194235819"></a>DDR内存颗粒多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1657654412615"><a name="p1657654412615"></a><a name="p1657654412615"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p665744210580"><a name="p665744210580"></a><a name="p665744210580"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p865724214585"><a name="p865724214585"></a><a name="p865724214585"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p96570421585"><a name="p96570421585"></a><a name="p96570421585"></a>当DDR内存颗粒存在多bit ECC错误时，产生此告警；当DDR内存颗粒多bit ECC错误恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row852512195917"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1652515113598"><a name="p1652515113598"></a><a name="p1652515113598"></a>0x000E001B</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1152514155917"><a name="p1152514155917"></a><a name="p1152514155917"></a>DDRA多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p657644422618"><a name="p657644422618"></a><a name="p657644422618"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1452510165916"><a name="p1452510165916"></a><a name="p1452510165916"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1452512155919"><a name="p1452512155919"></a><a name="p1452512155919"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p205259155911"><a name="p205259155911"></a><a name="p205259155911"></a>当DDRA存在多bit ECC错误时，产生此告警；当DDRA多bit ECC错误恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row8214191591"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p42144945911"><a name="p42144945911"></a><a name="p42144945911"></a>0x000E001C</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1421416935910"><a name="p1421416935910"></a><a name="p1421416935910"></a>来自DDRC的错误响应</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p3576044152620"><a name="p3576044152620"></a><a name="p3576044152620"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1921412975918"><a name="p1921412975918"></a><a name="p1921412975918"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p12214149165914"><a name="p12214149165914"></a><a name="p12214149165914"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p172141999599"><a name="p172141999599"></a><a name="p172141999599"></a>当存在来自DDRC的错误响应时，产生此告警；当来自DDRC的错误响应恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row184036520592"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p10403253599"><a name="p10403253599"></a><a name="p10403253599"></a>0x000E001D</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p20403115175913"><a name="p20403115175913"></a><a name="p20403115175913"></a>DDRC硬件模块内部逻辑异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p2576174411267"><a name="p2576174411267"></a><a name="p2576174411267"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p44033511598"><a name="p44033511598"></a><a name="p44033511598"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1340310555912"><a name="p1340310555912"></a><a name="p1340310555912"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0208481165_zh-cn_topic_0176113981_p1175917962"><a name="zh-cn_topic_0208481165_zh-cn_topic_0176113981_p1175917962"></a><a name="zh-cn_topic_0208481165_zh-cn_topic_0176113981_p1175917962"></a>DDRC硬件模块内部逻辑异常时，产生此告警；DDRC硬件模块内部逻辑异常正常时，此告警消失。</p>
</td>
</tr>
<tr id="row18396104675818"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p139644617588"><a name="p139644617588"></a><a name="p139644617588"></a>0x000E001E</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p43961946105811"><a name="p43961946105811"></a><a name="p43961946105811"></a>DDRC总线访问错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p357694415268"><a name="p357694415268"></a><a name="p357694415268"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p939674616584"><a name="p939674616584"></a><a name="p939674616584"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1039616464583"><a name="p1039616464583"></a><a name="p1039616464583"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p11396104695816"><a name="p11396104695816"></a><a name="p11396104695816"></a>当DDRC总线访问错误时，产生此告警；当DDRC总线访问正常时，此告警消失。</p>
</td>
</tr>
<tr id="row187531157125810"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p575315719588"><a name="p575315719588"></a><a name="p575315719588"></a>0x000E001F</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p117531057155818"><a name="p117531057155818"></a><a name="p117531057155818"></a>DDRC多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p7576154415265"><a name="p7576154415265"></a><a name="p7576154415265"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p2753957115816"><a name="p2753957115816"></a><a name="p2753957115816"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p18753195711584"><a name="p18753195711584"></a><a name="p18753195711584"></a>紧急</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1675313579589"><a name="p1675313579589"></a><a name="p1675313579589"></a>当DDRC多bit ECC错误时，产生此告警；当DDRC恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row18732114914589"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p14732154917580"><a name="p14732154917580"></a><a name="p14732154917580"></a>0x000E0020</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p9732164911588"><a name="p9732164911588"></a><a name="p9732164911588"></a>DDR颗粒高温异常：非致命高温异常告警</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1757654416269"><a name="p1757654416269"></a><a name="p1757654416269"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p12732049135816"><a name="p12732049135816"></a><a name="p12732049135816"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p14732134911584"><a name="p14732134911584"></a><a name="p14732134911584"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p187203619201"><a name="p187203619201"></a><a name="p187203619201"></a>当DDR颗粒高温异常时，产生此告警；当DDR颗粒温度恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row13797155355812"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p15797553185817"><a name="p15797553185817"></a><a name="p15797553185817"></a>0x000E0021</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1479765315819"><a name="p1479765315819"></a><a name="p1479765315819"></a>DVPP Dispatch多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1257619448262"><a name="p1257619448262"></a><a name="p1257619448262"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p18797125312589"><a name="p18797125312589"></a><a name="p18797125312589"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p17973534582"><a name="p17973534582"></a><a name="p17973534582"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p17971853165819"><a name="p17971853165819"></a><a name="p17971853165819"></a>当DVPP Dispatch多bit ECC错误时，产生此告警；当DVPP Dispatch多bit ECC正常时，此告警消失。</p>
</td>
</tr>
<tr id="row165131207588"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p651460205820"><a name="p651460205820"></a><a name="p651460205820"></a>0x000E0022</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p45141202586"><a name="p45141202586"></a><a name="p45141202586"></a>DVPP Dispatch输入错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p4576134462612"><a name="p4576134462612"></a><a name="p4576134462612"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p14514110125816"><a name="p14514110125816"></a><a name="p14514110125816"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p15514170125811"><a name="p15514170125811"></a><a name="p15514170125811"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p45147075811"><a name="p45147075811"></a><a name="p45147075811"></a>当DVPP Dispatch输入错误时，产生此告警；当DVPP Dispatch输入正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1432917355815"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p93298325813"><a name="p93298325813"></a><a name="p93298325813"></a>0x000E0023</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1032943175817"><a name="p1032943175817"></a><a name="p1032943175817"></a>HSM密钥管理模块错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p115769447269"><a name="p115769447269"></a><a name="p115769447269"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p143303395816"><a name="p143303395816"></a><a name="p143303395816"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p153306305812"><a name="p153306305812"></a><a name="p153306305812"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1233023155813"><a name="p1233023155813"></a><a name="p1233023155813"></a>当HSM密钥管理模块错误时，产生此告警；当HSM密钥管理模块正常时，此告警消失。</p>
</td>
</tr>
<tr id="row01301135195810"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p4130153512580"><a name="p4130153512580"></a><a name="p4130153512580"></a>0x000E0024</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p8130183513584"><a name="p8130183513584"></a><a name="p8130183513584"></a>HSM密码算法模块错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p957634422619"><a name="p957634422619"></a><a name="p957634422619"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1113093516585"><a name="p1113093516585"></a><a name="p1113093516585"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p8130163535818"><a name="p8130163535818"></a><a name="p8130163535818"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p171306352588"><a name="p171306352588"></a><a name="p171306352588"></a>当HSM密码算法模块错误时，产生此告警；当HSM密码算法模块正常时，此告警消失。</p>
</td>
</tr>
<tr id="row56593910411"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p665439944"><a name="p665439944"></a><a name="p665439944"></a>0x000E0025</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p12651390413"><a name="p12651390413"></a><a name="p12651390413"></a>HWTS总线访问错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p165761644172619"><a name="p165761644172619"></a><a name="p165761644172619"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p18651391146"><a name="p18651391146"></a><a name="p18651391146"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1165139848"><a name="p1165139848"></a><a name="p1165139848"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p765163918413"><a name="p765163918413"></a><a name="p765163918413"></a>当HWTS总线访问错误时，产生此告警；当HWTS总线访问正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1410617424411"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1110624215420"><a name="p1110624215420"></a><a name="p1110624215420"></a>0x000E0026</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p41061421345"><a name="p41061421345"></a><a name="p41061421345"></a>HWTS多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p2576944202619"><a name="p2576944202619"></a><a name="p2576944202619"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p10106154214411"><a name="p10106154214411"></a><a name="p10106154214411"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p101071442149"><a name="p101071442149"></a><a name="p101071442149"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1210719422048"><a name="p1210719422048"></a><a name="p1210719422048"></a>当HWTS多bit ECC错误时，产生此告警；当HWTS多bit ECC正常时，此告警消失。</p>
</td>
</tr>
<tr id="row65338451544"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p145331645742"><a name="p145331645742"></a><a name="p145331645742"></a>0x000E0027</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p553313454413"><a name="p553313454413"></a><a name="p553313454413"></a>JPEGD总线访问错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1957624422612"><a name="p1957624422612"></a><a name="p1957624422612"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1453384512419"><a name="p1453384512419"></a><a name="p1453384512419"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p353334516418"><a name="p353334516418"></a><a name="p353334516418"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p7533114515419"><a name="p7533114515419"></a><a name="p7533114515419"></a>当JPEGD总线访问错误时，产生此告警，当JPEGD总线访问恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1906144814420"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p69065483412"><a name="p69065483412"></a><a name="p69065483412"></a>0x000E0028</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p15906848645"><a name="p15906848645"></a><a name="p15906848645"></a>JPEGE硬件编码异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p05776447263"><a name="p05776447263"></a><a name="p05776447263"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p3906164815419"><a name="p3906164815419"></a><a name="p3906164815419"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p9907648742"><a name="p9907648742"></a><a name="p9907648742"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1090754814414"><a name="p1090754814414"></a><a name="p1090754814414"></a>当JPEGE硬件编码异常时，产生此告警，当JPEGE硬件编码恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row127781052549"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p8779145214420"><a name="p8779145214420"></a><a name="p8779145214420"></a>0x000E0029</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p17791852245"><a name="p17791852245"></a><a name="p17791852245"></a>JPEGE总线访问错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p105771344152617"><a name="p105771344152617"></a><a name="p105771344152617"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p3779552743"><a name="p3779552743"></a><a name="p3779552743"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p177912522411"><a name="p177912522411"></a><a name="p177912522411"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p87794521542"><a name="p87794521542"></a><a name="p87794521542"></a>当JPEGE总线访问错误时，产生此告警，当JPEGE总线访问恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row27477551746"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1274818551142"><a name="p1274818551142"></a><a name="p1274818551142"></a>0x000E002A</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p18748355049"><a name="p18748355049"></a><a name="p18748355049"></a>L2BUFF多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1757720443269"><a name="p1757720443269"></a><a name="p1757720443269"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p9748115515419"><a name="p9748115515419"></a><a name="p9748115515419"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1174810551545"><a name="p1174810551545"></a><a name="p1174810551545"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p574819556416"><a name="p574819556416"></a><a name="p574819556416"></a>当L2BUFF发生多bit ECC错误时，产生此告警，当L2BUFF恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row0325859946"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1032514596412"><a name="p1032514596412"></a><a name="p1032514596412"></a>0x000E002B</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1732545913410"><a name="p1732545913410"></a><a name="p1732545913410"></a>L2BUFF内部软件配置错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p14577204416262"><a name="p14577204416262"></a><a name="p14577204416262"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p13325115911416"><a name="p13325115911416"></a><a name="p13325115911416"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p8325159143"><a name="p8325159143"></a><a name="p8325159143"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1389841617422"><a name="p1389841617422"></a><a name="p1389841617422"></a>当L2BUFF内部软件配置错误时，产生此告警，当L2BUFF内部软件配置恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1518092158"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1018118217518"><a name="p1018118217518"></a><a name="p1018118217518"></a>0x000E002C</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p263511279428"><a name="p263511279428"></a><a name="p263511279428"></a>L3D多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p157794462610"><a name="p157794462610"></a><a name="p157794462610"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p118162754"><a name="p118162754"></a><a name="p118162754"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p151812021758"><a name="p151812021758"></a><a name="p151812021758"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p118116218513"><a name="p118116218513"></a><a name="p118116218513"></a>当L3D发生多bit ECC错误时，产生此告警，当L3D恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1147445959"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p647419513514"><a name="p647419513514"></a><a name="p647419513514"></a>0x000E002D</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1647495353"><a name="p1647495353"></a><a name="p1647495353"></a>L3T多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p1857714410268"><a name="p1857714410268"></a><a name="p1857714410268"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p247420514513"><a name="p247420514513"></a><a name="p247420514513"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p44754516510"><a name="p44754516510"></a><a name="p44754516510"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1138918165437"><a name="p1138918165437"></a><a name="p1138918165437"></a>当L3T发生多bit ECC错误时，产生此告警，当L3T恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row17770162214432"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p14771192284312"><a name="p14771192284312"></a><a name="p14771192284312"></a>0x000E002E</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p377113220438"><a name="p377113220438"></a><a name="p377113220438"></a>NIC多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p13141716204416"><a name="p13141716204416"></a><a name="p13141716204416"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1131416168443"><a name="p1131416168443"></a><a name="p1131416168443"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p87711722164319"><a name="p87711722164319"></a><a name="p87711722164319"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p3771822194319"><a name="p3771822194319"></a><a name="p3771822194319"></a>当NIC发生多bit ECC错误时，产生此告警，当NIC模块恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1236417255438"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p8364192564319"><a name="p8364192564319"></a><a name="p8364192564319"></a>0x000E002F</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p18364325174319"><a name="p18364325174319"></a><a name="p18364325174319"></a>NIC模块异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p431591664410"><a name="p431591664410"></a><a name="p431591664410"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p183151816154419"><a name="p183151816154419"></a><a name="p183151816154419"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p17364112518430"><a name="p17364112518430"></a><a name="p17364112518430"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p12364525104313"><a name="p12364525104313"></a><a name="p12364525104313"></a>当NIC模块异常时，产生此告警，当NIC模块恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row144368277435"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p243722774315"><a name="p243722774315"></a><a name="p243722774315"></a>0x000E0030</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p0437427144310"><a name="p0437427144310"></a><a name="p0437427144310"></a>PERI Dispatch多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p163158163440"><a name="p163158163440"></a><a name="p163158163440"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p331571624413"><a name="p331571624413"></a><a name="p331571624413"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p243722719437"><a name="p243722719437"></a><a name="p243722719437"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p164376275432"><a name="p164376275432"></a><a name="p164376275432"></a>当PERI Dispatch多bit ECC错误时，产生此告警，当PERI Dispatch恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row3937617444"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p29313694414"><a name="p29313694414"></a><a name="p29313694414"></a>0x000E0031</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p139346144414"><a name="p139346144414"></a><a name="p139346144414"></a>PERI Dispatch输入错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p131514167446"><a name="p131514167446"></a><a name="p131514167446"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p13315111674411"><a name="p13315111674411"></a><a name="p13315111674411"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p109312620447"><a name="p109312620447"></a><a name="p109312620447"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p5938616443"><a name="p5938616443"></a><a name="p5938616443"></a>当PERI Dispatch输入错误时，产生此告警，当PERI Dispatch输入恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row17228108174412"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p72282874411"><a name="p72282874411"></a><a name="p72282874411"></a>0x000E0032</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p102281886448"><a name="p102281886448"></a><a name="p102281886448"></a>SDMA多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p33151163443"><a name="p33151163443"></a><a name="p33151163443"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1131514162446"><a name="p1131514162446"></a><a name="p1131514162446"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p422810844417"><a name="p422810844417"></a><a name="p422810844417"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p2228188154417"><a name="p2228188154417"></a><a name="p2228188154417"></a>当SDMA发生多bit ECC时，产生此告警；当SDMA恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row174761110104414"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p347761044419"><a name="p347761044419"></a><a name="p347761044419"></a>0x000E0033</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1447720103445"><a name="p1447720103445"></a><a name="p1447720103445"></a>SDMA模块bus error</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p14315191613444"><a name="p14315191613444"></a><a name="p14315191613444"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p1031511624418"><a name="p1031511624418"></a><a name="p1031511624418"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p247751044419"><a name="p247751044419"></a><a name="p247751044419"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p1947781012447"><a name="p1947781012447"></a><a name="p1947781012447"></a>当SDMA模块发生bus error时，产生此告警；当SDMA模块恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row12277151217442"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1327791210442"><a name="p1327791210442"></a><a name="p1327791210442"></a>0x000E0034</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p527761234414"><a name="p527761234414"></a><a name="p527761234414"></a>VDEC多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p631591644418"><a name="p631591644418"></a><a name="p631591644418"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p4315216194419"><a name="p4315216194419"></a><a name="p4315216194419"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p13277151264418"><a name="p13277151264418"></a><a name="p13277151264418"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p14277101254419"><a name="p14277101254419"></a><a name="p14277101254419"></a>当VDEC发生多bit ECC错误时，产生此告警；当VDEC恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row92121430184315"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p12127304439"><a name="p12127304439"></a><a name="p12127304439"></a>0x000E0035</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p15213830204319"><a name="p15213830204319"></a><a name="p15213830204319"></a>VENC硬件编码超时</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p621303011430"><a name="p621303011430"></a><a name="p621303011430"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p132131030104315"><a name="p132131030104315"></a><a name="p132131030104315"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p421316308438"><a name="p421316308438"></a><a name="p421316308438"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p2213123004319"><a name="p2213123004319"></a><a name="p2213123004319"></a>当VENC硬件编码超时时，产生此告警；当VENC硬件编码恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row1643718328430"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p12437153211431"><a name="p12437153211431"></a><a name="p12437153211431"></a>0x000E0036</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p15437163213435"><a name="p15437163213435"></a><a name="p15437163213435"></a>VENC硬件编码异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p28477125015"><a name="p28477125015"></a><a name="p28477125015"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p108470114508"><a name="p108470114508"></a><a name="p108470114508"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1543733215439"><a name="p1543733215439"></a><a name="p1543733215439"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p9437132144310"><a name="p9437132144310"></a><a name="p9437132144310"></a>当VENC硬件编码异常时，产生此告警；当VVENC硬件编码恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row682811414819"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p6829121454819"><a name="p6829121454819"></a><a name="p6829121454819"></a>0x000E0037</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p1182981412487"><a name="p1182981412487"></a><a name="p1182981412487"></a>VPC图像处理硬件异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p88474118507"><a name="p88474118507"></a><a name="p88474118507"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p484710145017"><a name="p484710145017"></a><a name="p484710145017"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p5829171417486"><a name="p5829171417486"></a><a name="p5829171417486"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p082971418484"><a name="p082971418484"></a><a name="p082971418484"></a>当VPC图像处理硬件异常时，产生此告警；当VPC图像处理硬件恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row18853131611481"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p4853151684820"><a name="p4853151684820"></a><a name="p4853151684820"></a>0x000E0038</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p185341614486"><a name="p185341614486"></a><a name="p185341614486"></a>VPC图像处理配置异常</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p128478115020"><a name="p128478115020"></a><a name="p128478115020"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p6848151145012"><a name="p6848151145012"></a><a name="p6848151145012"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p1285417165484"><a name="p1285417165484"></a><a name="p1285417165484"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p17854216204817"><a name="p17854216204817"></a><a name="p17854216204817"></a>当VPC图像处理配置异常时，产生此告警；当VPC图像处理配置恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row2356141911489"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p235613199488"><a name="p235613199488"></a><a name="p235613199488"></a>0x000E0039</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p835661912484"><a name="p835661912484"></a><a name="p835661912484"></a>VPC多bit ECC错误</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p88481812507"><a name="p88481812507"></a><a name="p88481812507"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p18848713501"><a name="p18848713501"></a><a name="p18848713501"></a>NPU</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p3356919144815"><a name="p3356919144815"></a><a name="p3356919144815"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p13568192485"><a name="p13568192485"></a><a name="p13568192485"></a>当VPC发生多bit ECC错误时，产生此告警；当VPC恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row17254092056"><td class="cellrowborder" rowspan="2" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p1860361712317"><a name="p1860361712317"></a><a name="p1860361712317"></a>Wireless_Module告警</p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p22541692513"><a name="p22541692513"></a><a name="p22541692513"></a>0x00150000</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p1925439655"><a name="p1925439655"></a><a name="p1925439655"></a>Wireless_Module网络不可用</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p12577844172617"><a name="p12577844172617"></a><a name="p12577844172617"></a>否</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p72547915510"><a name="p72547915510"></a><a name="p72547915510"></a>Wireless_Module</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p132557920515"><a name="p132557920515"></a><a name="p132557920515"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="p22551392515"><a name="p22551392515"></a><a name="p22551392515"></a>当4G/5G网络状态异常时，产生此告警；当4G/5G网络正常时，此告警消失。</p>
</td>
</tr>
<tr id="row92513321089"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p162511632583"><a name="p162511632583"></a><a name="p162511632583"></a>0x00150001</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p192511321580"><a name="p192511321580"></a><a name="p192511321580"></a>Wireless_Module开关打开失败</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p85771944182612"><a name="p85771944182612"></a><a name="p85771944182612"></a>否</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p625111321382"><a name="p625111321382"></a><a name="p625111321382"></a>Wireless_Module</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p192514323816"><a name="p192514323816"></a><a name="p192514323816"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="zh-cn_topic_0208246978_p1743634210451"><a name="zh-cn_topic_0208246978_p1743634210451"></a><a name="zh-cn_topic_0208246978_p1743634210451"></a>如果系统重启前，Wireless_Module开关配置为开启，系统启动过程中打开LTE开关失败，产生此告警；系统启动过程中打开Wireless_Module开关成功或者手动打开LTE开关成功时，此告警消失。</p>
</td>
</tr>
<tr id="row54298352080"><td class="cellrowborder" rowspan="2" valign="top" width="7.5359202424005485%" headers="mcps1.2.8.1.1 "><p id="p177071341162313"><a name="p177071341162313"></a><a name="p177071341162313"></a>其他告警</p>
<p id="p876402411445"><a name="p876402411445"></a><a name="p876402411445"></a></p>
</td>
<td class="cellrowborder" valign="top" width="9.725344541100577%" headers="mcps1.2.8.1.2 "><p id="p11430635786"><a name="p11430635786"></a><a name="p11430635786"></a>0x000D0000</p>
</td>
<td class="cellrowborder" valign="top" width="13.94780568859349%" headers="mcps1.2.8.1.3 "><p id="p14301335186"><a name="p14301335186"></a><a name="p14301335186"></a>USB Hub异常</p>
</td>
<td class="cellrowborder" valign="top" width="7.682533476688497%" headers="mcps1.2.8.1.4 "><p id="p1357714462613"><a name="p1357714462613"></a><a name="p1357714462613"></a>是</p>
</td>
<td class="cellrowborder" valign="top" width="10.741862965497019%" headers="mcps1.2.8.1.5 "><p id="p74305355812"><a name="p74305355812"></a><a name="p74305355812"></a>USB集线器</p>
</td>
<td class="cellrowborder" valign="top" width="7.027661030202327%" headers="mcps1.2.8.1.6 "><p id="p16430193514817"><a name="p16430193514817"></a><a name="p16430193514817"></a>一般</p>
</td>
<td class="cellrowborder" valign="top" width="43.338872055517555%" headers="mcps1.2.8.1.7 "><p id="p10430123519811"><a name="p10430123519811"></a><a name="p10430123519811"></a>当USB集线器被损坏时，产生此告警；当USB集线器恢复正常时，此告警消失。</p>
</td>
</tr>
<tr id="row11262038189"><td class="cellrowborder" valign="top" headers="mcps1.2.8.1.1 "><p id="p1727153819819"><a name="p1727153819819"></a><a name="p1727153819819"></a>0x00180000</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.2 "><p id="p209683461018"><a name="p209683461018"></a><a name="p209683461018"></a>证书告警</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.3 "><p id="p105771244122617"><a name="p105771244122617"></a><a name="p105771244122617"></a>是</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.4 "><p id="p11278385820"><a name="p11278385820"></a><a name="p11278385820"></a>CERT，FD_CERT</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.5 "><p id="p9275381817"><a name="p9275381817"></a><a name="p9275381817"></a>严重</p>
</td>
<td class="cellrowborder" valign="top" headers="mcps1.2.8.1.6 "><p id="p62712380811"><a name="p62712380811"></a><a name="p62712380811"></a>证书即将过期时产生此告警；当更新为有效证书后，此告警消失。</p>
</td>
</tr>
</tbody>
</table>