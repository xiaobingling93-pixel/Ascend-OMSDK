# 产品简介<a name="ZH-CN_TOPIC_0000001628849853"></a>

随着边缘计算的兴起，边缘设备逐渐向智能化、平台化趋势发展。OM SDK作为开发态组件，使能第三方ISV（Independent Software Vendor，独立软件供应商）或者帮助开发者基于昇腾算力芯片快速搭建硬件管理平台，简化硬件设备的运维部署，快速构建自定义的设备运维系统。

**功能介绍<a name="section76204641111"></a>**

边缘管理系统支持对边缘设备进行初始化配置、硬件监测、软件安装、系统运维等功能；同时还支持与SmartKit软件、华为FusionDirector管理软件对接，实现集中式运维管理。

**表 1** 边缘管理系统功能介绍

<table><thead align="left"><tr id="row145994344815"><th class="cellrowborder" valign="top" width="31.130000000000003%" id="mcps1.2.3.1.1"><p id="p6599193420819"><a name="p6599193420819"></a><a name="p6599193420819"></a>功能类型</p>
</th>
<th class="cellrowborder" valign="top" width="68.87%" id="mcps1.2.3.1.2"><p id="p18599634388"><a name="p18599634388"></a><a name="p18599634388"></a>详细功能介绍</p>
</th>
</tr>
</thead>
<tbody><tr id="row135991334589"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p6180119141713"><a name="p6180119141713"></a><a name="p6180119141713"></a>硬件管理</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><a name="ul976875262312"></a><a name="ul976875262312"></a><ul id="ul976875262312"><li>硬件信息查询<a name="ul1441253114459"></a><a name="ul1441253114459"></a><ul id="ul1441253114459"><li>查询整机硬件版本、型号、健康状态</li><li>查询模组软硬件版本、型号、健康状态</li><li>查询电压、温度、功耗等信息</li></ul>
</li><li>硬件故障检测</li></ul>
</td>
</tr>
<tr id="row1560023417819"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p518121921715"><a name="p518121921715"></a><a name="p518121921715"></a>软件管理</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><a name="ul10866183304810"></a><a name="ul10866183304810"></a><ul id="ul10866183304810"><li>系统OS、驱动固件升级<a name="ul17761114517486"></a><a name="ul17761114517486"></a><ul id="ul17761114517486"><li>升级前检查</li><li>系统固件升级后回退</li></ul>
</li><li>软件信息查询<p id="p1896885724514"><a name="p1896885724514"></a><a name="p1896885724514"></a>查询软件版本、算力、内存占有率</p>
</li><li>一键式开局和免软调上线</li><li><span id="ph158731853192117"><a name="ph158731853192117"></a><a name="ph158731853192117"></a>OM SDK</span>的安装和升级</li></ul>
</td>
</tr>
<tr id="row2060015341681"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p19181161961718"><a name="p19181161961718"></a><a name="p19181161961718"></a>时间管理</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><a name="ul79401216105210"></a><a name="ul79401216105210"></a><ul id="ul79401216105210"><li>系统时区、系统时间配置</li><li>支持NTP从服务器同步时间</li></ul>
</td>
</tr>
<tr id="row3600434183"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p01811419161718"><a name="p01811419161718"></a><a name="p01811419161718"></a>网络管理</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><a name="ul4842151635312"></a><a name="ul4842151635312"></a><ul id="ul4842151635312"><li>支持ETH、WiFi、LTE等多种网络设备配置</li><li>支持手动配置系统网口的IP、端口、VLAN、网关、DNS</li><li>支持DHCP从Server端获取系统IP</li></ul>
</td>
</tr>
<tr id="row47247100172"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p2018101921715"><a name="p2018101921715"></a><a name="p2018101921715"></a>存储管理</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><a name="ul25631334569"></a><a name="ul25631334569"></a><ul id="ul25631334569"><li>支持查询和配置本地存储<p id="p15710737115616"><a name="p15710737115616"></a><a name="p15710737115616"></a>查询系统分区、存储容量和分区健康状态</p>
</li><li>支持配置、查询NFS存储系统，如NFS挂载，容量显示，连接健康状态</li></ul>
</td>
</tr>
<tr id="row253191310176"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p1618161919177"><a name="p1618161919177"></a><a name="p1618161919177"></a>用户管理</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><a name="ul11294189131110"></a><a name="ul11294189131110"></a><ul id="ul11294189131110"><li>支持密码有效期，登录规则、弱口令设置、查询，支持用户密码修改</li><li>支持用户可定制化的安全策略，支持客户可信根导入</li><li>支持Web证书导入、查询和有效期检查</li></ul>
</td>
</tr>
<tr id="row4319115131719"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p918171961711"><a name="p918171961711"></a><a name="p918171961711"></a>系统监测</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><a name="ul67551119151119"></a><a name="ul67551119151119"></a><ul id="ul67551119151119"><li>支持告警上报，告警屏蔽、历史告警查询、支持当前告警显示</li><li>支持客户增量设备、关键进程的告警集成显示、管理</li><li>系统支持安全日志、操作日志、运行日志、黑匣子记录，支持日志收集、查询、远程syslog</li></ul>
</td>
</tr>
<tr id="row8462201741712"><td class="cellrowborder" valign="top" width="31.130000000000003%" headers="mcps1.2.3.1.1 "><p id="p1518111941710"><a name="p1518111941710"></a><a name="p1518111941710"></a>北向接口</p>
</td>
<td class="cellrowborder" valign="top" width="68.87%" headers="mcps1.2.3.1.2 "><p id="p3181121991718"><a name="p3181121991718"></a><a name="p3181121991718"></a>系统功能支持<span id="ph135441743969"><a name="ph135441743969"></a><a name="ph135441743969"></a>FusionDirector</span>集中纳管协议，支持RESTful开放接口，RESTful满足服务器北向接口标准</p>
</td>
</tr>
</tbody>
</table>

**免责声明<a name="section5760134553519"></a>**

- 边缘管理系统仅作为参考设计提供，不构成对第三方开发者开发的任何软件产品及服务的功能、性能、安全性、合规性等做出保证或承诺。
- 第三方开发者应全权负责基于本产品开发的任何软件的设计、开发、测试、发布及运营维护，并自行承担因使用本产品导致的任何直接或间接性的损害赔偿责任。我方不承诺未来为本产品提供版本升级、功能扩展、漏洞修复等后续服务或支持。第三方开发者已预先熟知上述风险，自愿使用本产品，并完全同意承担一切可能的风险和法律后果。

# 支持的产品形态<a name="ZH-CN_TOPIC_0000001577530708"></a>

OM SDK支持的产品和产品所支持的操作系统如[表1 产品信息](#产品信息)所示。

**表 1**  产品信息 <a id="产品信息"></a>

<table><thead align="left"><tr id="row838305110198"><th class="cellrowborder" valign="top" width="50%" id="mcps1.2.3.1.1"><p id="p63832051141918"><a name="p63832051141918"></a><a name="p63832051141918"></a>产品名称</p>
</th>
<th class="cellrowborder" valign="top" width="50%" id="mcps1.2.3.1.2"><p id="p8383125111914"><a name="p8383125111914"></a><a name="p8383125111914"></a>操作系统</p>
</th>
</tr>
</thead>
<tbody><tr id="row1383125191912"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.2.3.1.1 "><p id="p0383115141917"><a name="p0383115141917"></a><a name="p0383115141917"></a><span id="ph1269314183209"><a name="ph1269314183209"></a><a name="ph1269314183209"></a>Atlas 200I A2 加速模块</span>（RC模式）</p>
</td>
<td class="cellrowborder" rowspan="2" valign="top" width="50%" headers="mcps1.2.3.1.2 "><a name="ul12559155942210"></a><a name="ul12559155942210"></a><ul id="ul12559155942210"><li><span id="ph266450145110"><a name="ph266450145110"></a><a name="ph266450145110"></a>openEuler</span> 22.03</li><li><span id="ph1052114212228"><a name="ph1052114212228"></a><a name="ph1052114212228"></a>Ubuntu</span> 22.04</li></ul>
<p id="p11383145131911"><a name="p11383145131911"></a><a name="p11383145131911"></a></p>
</td>
</tr>
<tr id="row738365113196"><td class="cellrowborder" valign="top" headers="mcps1.2.3.1.1 "><p id="p138325116198"><a name="p138325116198"></a><a name="p138325116198"></a><span id="ph218252310208"><a name="ph218252310208"></a><a name="ph218252310208"></a>Atlas 200I DK A2 开发者套件</span></p>
</td>
</tr>
</tbody>
</table>
