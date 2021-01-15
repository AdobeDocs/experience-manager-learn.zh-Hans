---
title: 将AEM的调度程序工具设置为Cloud Service开发
description: AEM SDK的Dispatcher Tools通过使本地安装、运行和诊断Dispatcher变得简单，从而方便了Adobe Experience Manager(AEM)项目的本地开发。
sub-product: 基础
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: 178ba3dbcb6f2050a9c56303bbabbcfcbead3e79
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 2%

---


# 设置本地调度程序工具

Adobe Experience Manager(AEM)的Dispatcher是一个Apache HTTP Web服务器模块，在CDN和AEM发布层之间提供安全性和性能层。 调度程序是整体Experience Manager体系结构的一个组成部分，应是本地开发设置的一部分。

AEM作为Cloud ServiceSDK，包括推荐的调度程序工具版本，该版本便于在本地配置、验证和模拟调度程序。 调度程序工具由以下部分组成：

+ 位于`.../dispatcher-sdk-x.x.x/src`的Apache HTTP Web服务器和调度程序配置文件的基线集
+ 配置validator CLI工具，位于`.../dispatcher-sdk-x.x.x/bin/validate`(Dispatcher SDK 2.0.29+)
+ 位于`.../dispatcher-sdk-x.x.x/bin/validator`的配置生成CLI工具
+ 配置部署CLI工具，位于`.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 使用调度程序模块运行Apache HTTP Web服务器的Docker图像

请注意，`~`用作用户目录的简写。 在Windows中，这等效于`%HOMEPATH%`。

>[!NOTE]
>
> 本页中的视频在macOS上录制。 Windows用户可以跟进，但使用与每个视频一起提供的对等的Dispatcher Tools Windows命令。

## 前提条件

1. Windows用户必须使用Windows 10 Professional
1. 在本地开发机器上安装[Experience Manager发布快速启动Jar](./aem-runtime.md)。
   + 或者，也可以在本地AEM发布服务上安装最新的[AEM参考网站](https://github.com/adobe/aem-guides-wknd/releases)。 本教程使用此网站来可视化正在工作的Dispatcher。
1. 在本地开发机器上安装并开始最新版本的[Docker](https://www.docker.com/)(Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)。

## 下载Dispatcher Tools(作为AEM SDK的一部分)

AEM作为Cloud ServiceSDK或AEM SDK，包含用于在本地运行Apache HTTP Web服务器的调度程序工具（调度程序模块）进行开发，以及兼容的QuickStart Jar。

如果AEM作为Cloud ServiceSDK已下载到[设置本地AEM运行时](./aem-runtime.md)，则无需重新下载它。

1. 使用您的Adobe ID登录[experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Content%2Fjcr%3AlastModifiest.3Modified&amp;stedSt.stedSt.st.st.d.st.st.st.st.st.st.st.st.st.st.st.st.st.st.st.sSt.st.st.sordordested&amp;st.st.st.st.st.s.st.s.s.st.s.s.st.s.sord=列表&amp;p.offset=0&amp;p.limit=1)
   + 您的Adobe组织&#x200B;__必须__&#x200B;设置为AEMCloud Service才能下载AEM作为Cloud ServiceSDK
1. 单击最新的&#x200B;__AEM SDK__&#x200B;结果行以下载
   + 确保下载说明中记录AEM SDK的Dispatcher Tools v2.0.29+

## 从AEM SDK zip解压调程序工具

>[!TIP]
>
> Windows用户在包含本地调度程序工具的文件夹的路径中不能包含任何空格或特殊字符。 如果路径中存在空格，则`docker_run.cmd`将失败。

调度程序工具的版本与AEM SDK的版本不同。 确保通过AEM SDK版本提供调度程序工具的版本，该版本与AEM相匹配，是Cloud Service版本。

1. 解压缩下载的`aem-sdk-xxx.zip`文件
1. 将调度程序工具解压到`~/aem-sdk/dispatcher`中
   + Windows:将`aem-sdk-dispatcher-tools-x.x.x-windows.zip`解压缩到`C:\Users\<My User>\aem-sdk\dispatcher`中（根据需要创建缺少的文件夹）
   + macOS/Linux:执行随附的shell脚本`aem-sdk-dispatcher-tools-x.x.x-unix.sh`以解压缩调度程序工具
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

请注意，下面发出的所有命令都假定当前工作目录包含正在扩展的“调度程序工具”内容。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*此视频使用macOS进行说明。等效的Windows/Linux命令可用于获得类似结果*

## 了解Dispatcher配置文件

>[!TIP]
> 从[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)创建的Experience Manager项目已预填充这组调度程序配置文件，因此无需从Dispatcher Tools src文件夹进行复制。

调度程序工具提供一组Apache HTTP Web服务器和调度程序配置文件，它们为所有环境（包括本地开发）定义行为。

如果Experience ManagerMaven项目中尚不存在这些文件，则这些文件将复制到`dispatcher/src`文件夹中的Experience ManagerMaven项目。

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*此视频使用macOS进行说明。等效的Windows/Linux命令可用于获得类似结果*

解压缩的调度程序工具中提供配置文件的完整说明，如`dispatcher-sdk-x.x.x/docs/Config.html`。

## 验证配置

或者，可以使用`validate`脚本验证调度程序和Apache Web服务器配置（通过`httpd -t`）（请勿与`validator`可执行文件混淆）。

+ 使用:
   + Windows: `bin\validate src`
   + macOS/Linux:`./bin/validate.sh ./src`

## 在本地运行Dispatcher

要在本地运行调度程序，必须使用调度程序工具的`validator` CLI工具生成调度程序配置文件。

+ 使用:
   + Windows:`bin\validator full -d out src`
   + macOS/Linux:`./bin/validator full -d ./out ./src`

此命令将配置传输到与Docker容器的Apache HTTP Web服务器兼容的文件集中。

生成后，将使用Docker容器中本地运行Dispatcher的透明配置。 务必确保使用`validate` __和__&#x200B;输出使用验证程序的`-d`选项验证最新配置。

+ 使用:
   + Windows:`bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux:`./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host`可设置为`host.docker.internal`,Docker在解析到主机IP的容器中提供一个特殊的DNS名称。 如果`host.docker.internal`未解析，请参见下面的[疑难解答](#troubleshooting-host-docker-internal)部分。

例如，使用“调度程序工具”提供的默认配置文件开始Dispatcher Docker容器:

1. 每次配置发生更改时，从头开始生成按惯例命名为`out`的`deployment-folder`:

   + Windows:`del /Q out && bin\validator full -d out src`
   + macOS/Linux:`rm -rf ./out && ./bin/validator full -d ./out ./src`

2. （重新）开始调度程序Docker容器，提供部署文件夹的路径：

   + Windows:`bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux:`./bin/docker_run.sh ./out host.docker.internal:4503 8080`

AEM作为Cloud ServiceSDK的发布服务，在端口4503上本地运行，可通过位于`http://localhost:8080`的调度程序使用。

要针对Experience Manager项目的Dispatcher配置运行Dispatcher Tools，只需使用项目的`dispatcher/src`文件夹生成`deployment-folder`。

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*此视频使用macOS进行说明。等效的Windows/Linux命令可用于获得类似结果*

## 调度程序工具日志

调度程序日志在本地开发过程中非常有用，可以了解HTTP请求是否被阻止以及为何被阻止。 可以通过使用环境参数预先执行`docker_run`来设置日志级别。

运行`docker_run`时，调度程序工具日志将发出到标准输出。

调试Dispatcher的有用参数包括：

+ `DISP_LOG_LEVEL=Debug` 将调度程序模块日志记录设置为“调试”级别
   + 默认值为: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 将Apache HTTP Web服务器重写模块日志记录设置为调试级别
   + 默认值为: `Warn`
+ `DISP_RUN_MODE` 设置调度程序环境的“运行模式”，并加载相应的运行模式调度程序配置文件。
   + 默认为 `dev`
+ 有效值：`dev`、`stage`或`prod`

可以将一个或多个参数传递给`docker_run`

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*此视频使用macOS进行说明。等效的Windows/Linux命令可用于获得类似结果*

### 日志文件访问

Apache Web服务器和AEM Dispatcher日志可以直接在Docker容器中访问：

+ [访问Docker容器中的日志](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [将Docker日志复制到本地文件系统](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 何时更新调度程序工具{#dispatcher-tools-version}

与Experience Manager相比，调度程序工具版本的增量更少，因此调度程序工具在本地开发环境中需要的更新更少。

建议的调度程序工具版本是与AEM捆绑为Cloud ServiceSDK的Experience Manager，该与Cloud Service版本相匹配。 AEM作为Cloud Service的版本可通过[云管理器](https://my.cloudmanager.adobe.com/)找到。

+ __Cloud Manager >环境__，根据AEM Releaselabel指定的 __环境__ 数

![Experience Manager版](./assets/dispatcher-tools/aem-version.png)

_请注意，调度程序工具版本本身与Experience Manager版本不匹配。_

## 疑难解答

### docker_run结果为“等到host.docker.internal可用”消息{#troubleshooting-host-docker-internal}

`host.docker.internal` 是提供给Docker的主机名，其中包含解析给主机的主机名。每docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host),[Windows](https://docs.docker.com/docker-for-windows/networking/)):

> 从Docker 18.03开始，我们的建议是连接到特殊的DNS名称host.docker.internal，该名称解析为主机使用的内部IP地址

如果`bin/docker_run out host.docker.internal:4503 8080`导致消息&#x200B;__等待host.docker.internal可用__，则：

1. 确保Docker的安装版本为18.03或更高
2. 您可能设置了阻止注册／解析`host.docker.internal`名称的本地计算机。 请改用本地IP。
   + Windows:
      + 在命令提示符下，执行`ipconfig`并记录主机的&#x200B;__IPv4地址__。
      + 然后，使用此IP地址执行`docker_run`:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + 从终端执行`ifconfig`并记录主机&#x200B;__inet__ IP地址，通常为&#x200B;__en0__&#x200B;设备。
      + 然后使用主机IP地址执行`docker_run`:
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### 示例错误

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run结果出现“**错误：找不到部署文件夹

运行`docker_run.cmd`时，将显示一个读取&#x200B;__**错误的错误：找不到部署文件夹：__。 这通常是因为路径中存在空格。 如果可能，请删除文件夹中的空格，或将`aem-sdk`文件夹移动到不包含空格的路径。

例如，Windows用户文件夹通常为`<First name> <Last name>`，中间有空格。 在下面的示例中，文件夹`...\My User\...`包含一个空格，用于中断本地调度程序工具的`docker_run`执行。 如果空格在Windows用户文件夹中，请勿尝试重命名此文件夹，因为它会破坏Windows，而是将`aem-sdk`文件夹移至用户有权完全修改的新位置。 请注意，假定`aem-sdk`文件夹位于用户的主目录中的说明需要调整到新位置。

#### 示例错误

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run在Windows上无法开始{#troubleshooting-windows-compatible}

在Windows上运行`docker_run`可能会导致以下错误，从而阻止调度程序启动。 这是Windows上的Dispatcher报告的问题，将在将来的版本中修复。

#### 示例错误

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## 其他资源

+ [下载AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe云管理器](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [下载AEM参考网站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager调度程序文档](https://docs.adobe.com/content/help/zh-Hans/experience-manager-dispatcher/using/dispatcher.html)
