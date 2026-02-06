# OM SDK

- [最新消息](#最新消息)
- [简介](#简介)
- [目录结构](#目录结构)
- [版本说明](#版本说明)
- [兼容性信息](#兼容性信息)
- [环境部署](#环境部署)
- [编译流程](#编译流程)
- [快速入门](#快速入门)
- [功能介绍](#功能介绍)
- [API参考](#API参考)
- [安全声明](#安全声明)
- [分支维护策略](#分支维护策略)
- [版本维护策略](#版本维护策略)
- [免责声明](#免责声明)
- [License](#License)
- [贡献声明](#贡献声明)
- [建议与交流](#建议与交流)

## 最新消息

- [2025.12.30]：🚀 OM SDK开源发布

## 简介

OM SDK作为开发态组件，使能第三方合作伙伴基于昇腾AI处理器快速搭建智能边缘硬件管理平台，自定义构建设备运维系统，简化设备运维部署。

<div align="center">

[![Zread](https://img.shields.io/badge/Zread-Ask_AI-_.svg?style=flat&color=0052D9&labelColor=000000&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB3aWR0aD0iMTYiIGhlaWdodD0iMTYiIHZpZXdCb3g9IjAgMCAxNiAxNiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTQuOTYxNTYgMS42MDAxSDIuMjQxNTZDMS44ODgxIDEuNjAwMSAxLjYwMTU2IDEuODg2NjQgMS42MDE1NiAyLjI0MDFWNC45NjAxQzEuNjAxNTYgNS4zMTM1NiAxLjg4ODEgNS42MDAxIDIuMjQxNTYgNS42MDAxSDQuOTYxNTZDNS4zMTUwMiA1LjYwMDEgNS42MDE1NiA1LjMxMzU2IDUuNjAxNTYgNC45NjAxVjIuMjQwMUM1LjYwMTU2IDEuODg2NjQgNS4zMTUwMiAxLjYwMDEgNC45NjE1NiAxLjYwMDFaIiBmaWxsPSIjZmZmIi8%2BCjxwYXRoIGQ9Ik00Ljk2MTU2IDEwLjM5OTlIMi4yNDE1NkMxLjg4ODEgMTAuMzk5OSAxLjYwMTU2IDEwLjY4NjQgMS42MDE1NiAxMS4wMzk5VjEzLjc1OTlDMS42MDE1NiAxNC4xMTM0IDEuODg4MSAxNC4zOTk5IDIuMjQxNTYgMTQuMzk5OUg0Ljk2MTU2QzUuMzE1MDIgMTQuMzk5OSA1LjYwMTU2IDE0LjExMzQgNS42MDE1NiAxMy43NTk5VjExLjAzOTlDNS42MDE1NiAxMC42ODY0IDUuMzE1MDIgMTAuMzk5OSA0Ljk2MTU2IDEwLjM5OTlaIiBmaWxsPSIjZmZmIi8%2BCjxwYXRoIGQ9Ik0xMy43NTg0IDEuNjAwMUgxMS4wMzg0QzEwLjY4NSAxLjYwMDEgMTAuMzk4NCAxLjg4NjY0IDEwLjM5ODQgMi4yNDAxVjQuOTYwMUMxMC4zOTg0IDUuMzEzNTYgMTAuNjg1IDUuNjAwMSAxMS4wMzg0IDUuNjAwMUgxMy43NTg0QzE0LjExMTkgNS42MDAxIDE0LjM5ODQgNS4zMTM1NiAxNC4zOTg0IDQuOTYwMVYyLjI0MDFDMTQuMzk4NCAxLjg4NjY0IDE0LjExMTkgMS42MDAxIDEzLjc1ODQgMS42MDAxWiIgZmlsbD0iI2ZmZiIvPgo8cGF0aCBkPSJNNCAxMkwxMiA0TDQgMTJaIiBmaWxsPSIjZmZmIi8%2BCjxwYXRoIGQ9Ik00IDEyTDEyIDQiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIxLjUiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIvPgo8L3N2Zz4K&logoColor=ffffff)](https://zread.ai/Ascend/OMSDK)&nbsp;&nbsp;&nbsp;&nbsp;
[![DeepWiki](https://img.shields.io/badge/DeepWiki-Ask_AI-_.svg?style=flat&color=0052D9&labelColor=000000&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACwAAAAyCAYAAAAnWDnqAAAAAXNSR0IArs4c6QAAA05JREFUaEPtmUtyEzEQhtWTQyQLHNak2AB7ZnyXZMEjXMGeK/AIi+QuHrMnbChYY7MIh8g01fJoopFb0uhhEqqcbWTp06/uv1saEDv4O3n3dV60RfP947Mm9/SQc0ICFQgzfc4CYZoTPAswgSJCCUJUnAAoRHOAUOcATwbmVLWdGoH//PB8mnKqScAhsD0kYP3j/Yt5LPQe2KvcXmGvRHcDnpxfL2zOYJ1mFwrryWTz0advv1Ut4CJgf5uhDuDj5eUcAUoahrdY/56ebRWeraTjMt/00Sh3UDtjgHtQNHwcRGOC98BJEAEymycmYcWwOprTgcB6VZ5JK5TAJ+fXGLBm3FDAmn6oPPjR4rKCAoJCal2eAiQp2x0vxTPB3ALO2CRkwmDy5WohzBDwSEFKRwPbknEggCPB/imwrycgxX2NzoMCHhPkDwqYMr9tRcP5qNrMZHkVnOjRMWwLCcr8ohBVb1OMjxLwGCvjTikrsBOiA6fNyCrm8V1rP93iVPpwaE+gO0SsWmPiXB+jikdf6SizrT5qKasx5j8ABbHpFTx+vFXp9EnYQmLx02h1QTTrl6eDqxLnGjporxl3NL3agEvXdT0WmEost648sQOYAeJS9Q7bfUVoMGnjo4AZdUMQku50McDcMWcBPvr0SzbTAFDfvJqwLzgxwATnCgnp4wDl6Aa+Ax283gghmj+vj7feE2KBBRMW3FzOpLOADl0Isb5587h/U4gGvkt5v60Z1VLG8BhYjbzRwyQZemwAd6cCR5/XFWLYZRIMpX39AR0tjaGGiGzLVyhse5C9RKC6ai42ppWPKiBagOvaYk8lO7DajerabOZP46Lby5wKjw1HCRx7p9sVMOWGzb/vA1hwiWc6jm3MvQDTogQkiqIhJV0nBQBTU+3okKCFDy9WwferkHjtxib7t3xIUQtHxnIwtx4mpg26/HfwVNVDb4oI9RHmx5WGelRVlrtiw43zboCLaxv46AZeB3IlTkwouebTr1y2NjSpHz68WNFjHvupy3q8TFn3Hos2IAk4Ju5dCo8B3wP7VPr/FGaKiG+T+v+TQqIrOqMTL1VdWV1DdmcbO8KXBz6esmYWYKPwDL5b5FA1a0hwapHiom0r/cKaoqr+27/XcrS5UwSMbQAAAABJRU5ErkJggg==)](https://deepwiki.com/Ascend/OMSDK)

</div>

## 目录结构

关键目录如下，详细目录介绍参见[项目目录](docs/zh/dir_structure.md)。

    om-sdk                    # 项目根目录
    ├── build                 # 构建相关目录
    ├── docs                  # 文档目录
    │   └── zh                # 中文文档目录
    └── src                   # 源码目录
        ├── om                # OM SDK 组件代码
        └── om-web            # OM Web 前端组件代码

## 版本说明
OM SDK版本配套详情请参考：[版本配套说明](docs/zh/release_notes.md#版本配套说明)。

## 兼容性信息

表3 OM SDK支持的产品和产品所支持的操作系统
<table>
    <tr>
        <th>产品名称</th>
        <th>操作系统</th>
    </tr>
    <tr>
        <td>Atlas 200I A2 加速模块（RC模式）</td>
        <td rowspan="2">OpenEuler 22.03<br>Ubuntu 22.04</td>
    </tr>
    <tr>
        <td>Atlas 200I DK A2 开发者套件</td>
    </tr>
</table>

## 环境部署

在安装和使用前，用户需要了解安装须知、环境准备，具体内容请参考"[安装部署](docs/zh/user_guide/installation_guide.md#安装部署)"章节文档。

- 获取软件包
- 准备安装环境
- 通过命令行安装
    - 将软件包上传到环境任意目录下（如“/home”）
    - 在软件包目录下，执行以下命令，创建临时目录om_install
       ```shell
       mkdir om_install
       ```
    - 执行以下命令，解压tar.gz软件包
       ```shell
       tar -zxf om-sdk.tar.gz -C om_install
       ```
    - 执行以下命令，为安装脚本添加可执行权限
       ```shell
       chmod +x om_install/install.sh
       ```
    - 执行以下命令，安装软件包
       ```shell
       om_install/install.sh
       ```
    - 回显示例如下，表示安装完成
       ```shell
       check install environment success
       prepare service file success
       executing install success
       start service success
       Install MindXOM success, MindXOM service is ready.
       ```  

## 编译流程

### 依赖准备
OM SDK构建仅支持ARM64 CPU。

本节以ubuntu22.04系统为例，介绍如何通过源码编译生成OM软件包。

执行OM SDK编译前，请保证环境上安装了必要编译工具和依赖库，参考安装命令如下： 

```shell
apt-get update && apt-get upgrade -y
apt-get install -y python3 python3-pip build-essential zip unzip dos2unix git cmake autoconf automake libtool curl

# 由于ubuntu 22.04默认的node.js版本过低，需要手动下载安装
# 使用国内镜像下载node.js
NODE_MIRROR_URL=https://mirrors.huaweicloud.com/nodejs
# 编译OM SDK要求node.js版本最低为16.0
NODE_VERSION=v16.20.2
NODE_INSTALL_DIR=/usr/local/lib/nodejs
mkdir -p $NODE_INSTALL_DIR
curl -sSL $NODE_MIRROR_URL/$NODE_VERSION/node-$NODE_VERSION-linux-arm64.tar.gz | tar -zxvf - -C $NODE_INSTALL_DIR
find $NODE_INSTALL_DIR/node-$NODE_VERSION-linux-arm64/bin -mindepth 1 -exec sh -c 'ln -s {} /usr/bin/$(basename {})' \;
```

### 编译

1. 拉取OM SDK整体源码，例如放在/home目录下。
2. 进入/home/OMSDK/build目录
    ```shell
    cd /home/OMSDK/build
    ```
3. 修改组件版本配置文件service_config.ini中om-sdk-version字段值为所需编译版本，默认值如下：
    ```
    om-sdk-version=7.3.0
    ```
4. (可选)通过环境变量NPM_REGISTRY_MIRROR设置npm镜像地址，用户可以根据自身场景修改为其它的npm镜像地址。默认值如下：
    ```shell
    export NPM_REGISTRY_MIRROR=https://repo.huaweicloud.com/repository/npm
    ```
5. 执行以下命令，执行构建脚本：
    ```shell
    dos2unix *.sh && chmod +x *.sh
    ./build_all.sh
    ```
6. 执行完成后，可在/home/OMSDK/output目录下获取编译完成的软件包。

## 快速入门

安装OM SDK后，可登录边缘管理系统进行基础操作、系统管理和数据配置。具体内容请参考"[新手入门](docs/zh/user_guide/usage.md#新手入门)"章节文档。

- 用户登录
- 基础操作
- 首页
- 管理
- 设置

## 功能介绍

边缘管理系统支持对边缘设备进行初始化配置、硬件监测、软件安装、系统运维等功能；同时还支持与SmartKit软件、华为FusionDirector管理软件对接，实现集中式运维管理。具体内容请参考"[Web功能介绍](docs/zh/user_guide/usage.md#Web功能介绍)"章节文档。

表4 边缘管理系统功能介绍

| 功能类型 | 详细功能介绍                                                                                                                            |
|:-----|:----------------------------------------------------------------------------------------------------------------------------------|
| 硬件管理 | <ul><li>硬件信息查询</li><li>硬件故障检测</li></ul>                                                                                           |
| 软件管理 | <ul><li>系统OS、驱动固件升级</li><li>软件信息查询</li><li>一键式开局和免软调上线</li><li>OM SDK的安装和升级</li></ol>                                             |
| 时间管理 | <ul><li>系统时区、系统时间配置</li><li>支持NTP从服务器同步时间</li></ol>                                                                               |
| 网络管理 | <ul><li>支持ETH、WiFi、LTE等多种网络设备配置</li><li>支持手动配置系统网口的IP、端口、VLAN、网关、DNS</li><li>支持DHCP从Server端获取系统IP</li></ol>                       |
| 存储管理 | <ul><li>支持查询和配置本地存储</li><li>查询系统分区、存储容量和分区健康状态</li><li>支持配置、查询NFS存储系统，如NFS挂载，容量显示，连接健康状态</li></ol>                                |
| 用户管理 | <ul><li>支持密码有效期，登录规则、弱口令设置、查询，支持用户密码修改</li><li>支持用户可定制化的安全策略，支持客户可信根导入</li><li>支持Web证书导入、查询和有效期检查</li></ol>                       |
| 系统监测 | <ul><li>支持告警上报，告警屏蔽、历史告警查询、支持当前告警显示</li><li>支持客户增量设备、关键进程的告警集成显示、管理</li><li>系统支持安全日志、操作日志、运行日志、黑匣子记录，支持日志收集、查询、远程syslog</li></ol> |
| 北向接口 | <ul><li>系统功能支持FusionDirector集中纳管协议，支持RESTful开放接口，RESTful满足服务器北向接口标准</li></ol>                                                     |

## API参考

API请参考"[RESTful接口](docs/zh/secondary_development/api/api1.md#RESTful接口)"和"[云边协同接口](docs/zh/secondary_development/api/api2.md#云边协同接口)"。

## 安全声明

- 请参考[安全配置和加固](docs/zh/user_guide/security_hardening.md#安全配置和加固)对系统进行安全加固。
- 安全加固建议中的安全加固措施为基本的加固建议项。用户应根据自身业务，重新审视整个系统的网络安全加固措施。
- 外部下载的软件代码或程序可能存在风险，功能的安全性需由用户保证。
- 公网地址详见：[公网地址](docs/zh/user_guide/appendix.md#公网地址)
- 用户信息列表详见：[用户信息列表](docs/zh/user_guide/appendix.md#用户信息列表)


## 分支维护策略

版本分支的维护阶段如下：

| 状态          | 时间     | 说明                                                      |
|-------------|--------|---------------------------------------------------------|
| 计划          | 1-3个月  | 计划特性                                                    |
| 开发          | 3个月    | 开发新特性并修复问题，定期发布新版本                                      | 
| 维护          | 3-12个月 | 常规分支维护3个月，长期支持分支维护12个月。对重大BUG进行修复，不合入新特性，并视BUG的影响发布补丁版本 | 
| 生命周期终止（EOL） | N/A    | 分支不再接受任何修改                                              |

## 版本维护策略

| 版本       | 维护策略 | 当前状态 | 发布日期       | 后续状态 | EOL日期      |
|----------|------|------|------------|------|------------|
| master   | 长期支持 | 开发   | 在研分支，不发布   | -    | -          |

## 免责声明

- 本代码仓库中包含多个开发分支，这些分支可能包含未完成、实验性或未测试的功能。在正式发布之前，这些分支不应被用于任何生产环境或依赖关键业务的项目中。请务必仅使用我们的正式发行版本，以确保代码的稳定性和安全性。
  使用开发分支所导致的任何问题、损失或数据损坏，本项目及其贡献者概不负责。
- 正式版本请参考正式release版本: https://gitcode.com/Ascend/OMSDK/releases

## License

OM SDK以Mulan PSL v2许可证许可，对应许可证文本可查阅[LICENSE](LICENSE.md)。

## 贡献声明

1. 提交错误报告：如果您在OM SDK中发现了一个不存在安全问题的漏洞，请在OM SDK仓库中的Issues中搜索，以防该漏洞被重复提交，如果找不到漏洞可以创建一个新的Issues。如果发现了一个安全问题请不要将其公开，请参阅安全问题处理方式。提交错误报告时应该包含完整信息。
2. 安全问题处理：本项目中对安全问题处理的形式，请通过邮箱通知项目核心人员确认编辑。
3. 解决现有问题：通过查看仓库的Issues列表可以发现需要处理的问题信息, 可以尝试解决其中的某个问题。
4. 如何提出新功能：请使用Issues的Feature标签进行标记，我们会定期处理和确认开发。
5. 开始贡献：
    - Fork本项目的仓库
    - Clone到本地
    - 创建开发分支
    - 本地自测，提交前请通过所有的单元测试，包括为您要解决的问题新增的单元测试
    - 提交代码
    - 新建Pull Request
    - 代码检视，您需要根据评审意见修改代码，并重新提交更新。此流程可能涉及多轮迭代
    - 当您的PR获得足够数量的检视者批准后，Committer会进行最终审核
    - 审核和测试通过后，CI会将您的PR合并入到项目的主干分支

## 建议与交流

欢迎大家为社区做贡献。如果有任何疑问或建议，请提交[issues](https://gitcode.com/Ascend/OMSDK/issues)，我们会尽快回复。感谢您的支持。
