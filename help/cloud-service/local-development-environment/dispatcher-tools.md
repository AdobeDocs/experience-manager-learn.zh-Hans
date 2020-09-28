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
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 2%

---


# 设置本地调度程序工具

Adobe Experience Manager(AEM)的Dispatcher是一个Apache HTTP Web服务器模块，在CDN和AEM发布层之间提供安全性和性能层。 调度程序是整体Experience Manager体系结构的一个组成部分，应是本地开发设置的一部分。

AEM作为Cloud ServiceSDK，包括推荐的调度程序工具版本，该版本便于在本地配置、验证和模拟调度程序。 调度程序工具由以下部分组成：

+ 位于 `.../dispatcher-sdk-x.x.x/src`
+ 配置验证程序CLI工具，位于 `.../dispatcher-sdk-x.x.x/bin/validator`
+ 配置部署CLI工具，位于 `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 使用调度程序模块运行Apache HTTP Web服务器的Docker图像

请注 `~` 意，它用作用户目录的简写。 在Windows中，这相当于 `%HOMEPATH%`。

>[!NOTE]
>
> 本页中的视频在macOS上录制。 Windows用户可以跟进，但使用与每个视频一起提供的对等的Dispatcher Tools Windows命令。

## 前提条件

1. Windows用户必须使用Windows 10 Professional
1. 在本 [地开发机器上](./aem-runtime.md) ，安装Experience Manager发布快速启动。
   + （可选）在本地AEM [发布服务上](https://github.com/adobe/aem-guides-wknd/releases) ，安装最新的AEM参考网站。 本教程使用此网站来可视化正在工作的Dispatcher。
1. 在本地开发机器上安 [装并开始](https://www.docker.com/) Docker的最新版本(Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)。

## 下载Dispatcher Tools(作为AEM SDK的一部分)

AEM作为Cloud ServiceSDK或AEM SDK，包含用于在本地运行Apache HTTP Web服务器的调度程序工具（调度程序模块）进行开发，以及兼容的QuickStart Jar。

如果AEM作为Cloud ServiceSDK已下载以设置本 [地AEM运行时](./aem-runtime.md)，则无需重新下载它。

1. 使用您 [的Adobe ID登录experience.adobe.com/#](https://experience.adobe.com/#/downloads) /downloads
   + 请注意，您的Adobe __组织__ 必须提供AEM作为Cloud Service才能下载AEM作为Cloud ServiceSDK。
1. 导航到AEM __作为Cloud Service选项卡__
1. 按发布 __日期__ ，降 __序排__ 序
1. 单击最新的 __AEM__ SDK结果行
1. 查看并接受EULA，然后点击“下 __载__ ”按钮
1. 确保使用AEM SDK的Dispatcher Tools v2.0.21+

## 从AEM SDK zip解压调程序工具

>[!TIP]
>
> Windows用户在包含本地调度程序工具的文件夹的路径中不能包含任何空格或特殊字符。 如果路径中存在空格，则 `docker_run.cmd` 将失败。

调度程序工具的版本与AEM SDK的版本不同。 确保通过AEM SDK版本提供调度程序工具的版本，该版本与AEM相匹配，是Cloud Service版本。

1. 解压缩下载的文 `aem-sdk-xxx.zip` 件
1. 将调度程序工具解压缩到 `~/aem-sdk/dispatcher`
   + Windows:解压 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` 到( `C:\Users\<My User>\aem-sdk\dispatcher` 根据需要创建缺少的文件夹)
   + macOS/Linux:执行随附的shell脚本以解 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` 压缩调度程序工具
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

请注意，下面发出的所有命令都假定当前工作目录包含正在扩展的“调度程序工具”内容。

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)
*此视频使用macOS进行说明。 等效的Windows/Linux命令可用于获得类似的结果*

## 了解Dispatcher配置文件

>[!TIP]
> 从AEM Project Maven Archetype [创建的Experience Manager项目](https://github.com/adobe/aem-project-archetype) ，会预先填充这组Dispatcher配置文件，因此无需从Dispatcher Tools src文件夹进行复制。

调度程序工具提供一组Apache HTTP Web服务器和调度程序配置文件，它们为所有环境（包括本地开发）定义行为。

如果Experience ManagerMaven项目中尚不存在这些文件， `dispatcher/src` 则这些文件将被复制到Experience ManagerMaven项目中。

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)
*此视频使用macOS进行说明。 等效的Windows/Linux命令可用于获得类似的结果*

解压缩的调度程序工具中提供了配置文件的完整说明，如所示 `dispatcher-sdk-x.x.x/docs/Config.html`。

## 在本地运行Dispatcher

要在本地运行Dispatcher，必须使用Dispatcher Tools的CLI工具验证要用于配置Dispatcher的Dispatcher配 `validator` 置文件。

+ 使用:
   + Windows: `bin\validator full -d out src`
   + macOS/Linux: `./bin/validator full -d ./out ./src`

验证具有双重用途：

+ 验证Apache HTTP Web服务器和调度程序配置文件的正确性
+ 将配置传输到与Docker容器的Apache HTTP Web Server兼容的文件集中。

经过验证后，将使用在Docker容器中本地运行Dispatcher的透明配置。 务必确保使用验证程序选项验证 __和输__ 出最新配置 `-d` 。

+ 使用:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

可以 `aem-publish-host` 将其设 `host.docker.internal`置为，Docker在解析到主机IP的容器中提供一个特殊的DNS名称。 如果他 `host.docker.internal` 未解析，请参阅下面的疑 [难排解](#troubleshooting-host-docker-internal) 部分。

例如，使用“调度程序工具”提供的默认配置文件开始Dispatcher Docker容器:

1. 每次配 `deployment-folder`置发生 `out` 更改时，从头开始生成按惯例命名的：

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS/Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. （重新）开始调度程序Docker容器，提供部署文件夹的路径：

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

AEM作为Cloud ServiceSDK的发布服务，在端口4503上本地运行，可在上通过调度程序获得 `http://localhost:8080`。

要针对Experience Manager项目的调度程序配置运行调度程序工具，只需 `deployment-folder` 使用项目的文件夹生 `dispatcher/src` 成。

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
*此视频使用macOS进行说明。 等效的Windows/Linux命令可用于获得类似的结果*

## 调度程序工具日志

调度程序日志在本地开发过程中非常有用，可以了解HTTP请求是否被阻止以及为何被阻止。 可以通过使用环境参数预定执行来设 `docker_run` 置日志级别。

运行时调度程序工具日志会发出 `docker_run` 到标准输出。

调试Dispatcher的有用参数包括：

+ `DISP_LOG_LEVEL=Debug` 将调度程序模块日志记录设置为“调试”级别
   + 默认值为: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 将Apache HTTP Web服务器重写模块日志记录设置为调试级别
   + 默认值为: `Warn`
+ `DISP_RUN_MODE` 设置调度程序环境的“运行模式”，并加载相应的运行模式调度程序配置文件。
   + 默认为 `dev`
+ 有效值： `dev`、 `stage`或 `prod`

可以将一个或多个参数传递给 `docker_run`

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
*此视频使用macOS进行说明。 等效的Windows/Linux命令可用于获得类似的结果*

## 何时更新Dispatcher工具{#dispatcher-tools-version}

与Experience Manager相比，调度程序工具版本的增量更少，因此调度程序工具在本地开发环境中需要的更新更少。

建议的调度程序工具版本是与AEM捆绑为Cloud ServiceSDK的Experience Manager，该与Cloud Service版本相匹配。 AEM作为Cloud Service的版本可通过云管理 [器找到](https://my.cloudmanager.adobe.com/)。

+ __Cloud Manager >环境__，根据AEM发行版标签指 __定的环境__

![Experience Manager版](./assets/dispatcher-tools/aem-version.png)

_请注意，调度程序工具版本本身与Experience Manager版本不匹配。_

## 疑难解答

### docker_run结果中的“等到host.docker.internal可用”消息{#troubleshooting-host-docker-internal}

`host.docker.internal` 是提供给Docker的主机名，其中包含解析给主机的主机名。 每docs.docker.com([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> 从Docker 18.03开始，我们的建议是连接到特殊的DNS名称host.docker.internal，该名称解析为主机使用的内部IP地址

如果，当 `bin/docker_run out host.docker.internal:4503 8080` 出现消息“ __Waiting until host.docker.internal is available__（等待到host.docker.internal可用）”时，:

1. 确保Docker的安装版本为18.03或更高
2. 您可能设置了阻止注册／解析名称的本地计算 `host.docker.internal` 机。 请改用本地IP。
   + Windows:
      + 在命令提示符下，执 `ipconfig`行并记录主机 __的IPv4地址__ 。
      + 然后，使 `docker_run` 用此IP地址执行：
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + 从终端执行 `ifconfig` 并记录主机 __内__ IP地址，通常 __是en0__ 设备。
      + 然后使 `docker_run` 用主机IP地址执行：
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

运行 `docker_run.cmd`时，显示一个读 __取**错误：找不到部署文件夹：__。 这通常是因为路径中存在空格。 如果可能，请删除文件夹中的空格，或将 `aem-sdk` 文件夹移动到不包含空格的路径。

例如，Windows用户文件夹通常为， `<First name> <Last name>`中间有一个空格。 在下面的示例中，该文 `...\My User\...` 件夹包含一个空格，用于中断本地调度程序工具的 `docker_run` 执行。 如果空格在Windows用户文件夹中，请勿尝试重命名此文件夹，因为它将破坏Windows，而是将该文件夹移 `aem-sdk` 至您的用户有权完全修改的新位置。 请注意，假定文 `aem-sdk` 件夹位于用户主目录中的说明需要调整到新位置。

#### 示例错误

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run在Windows上无法开始{#troubleshooting-windows-compatible}

在Windows `docker_run` 上运行可能会导致以下错误，从而阻止Dispatcher启动。 这是Windows上的Dispatcher报告的问题，将在将来的版本中修复。

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
