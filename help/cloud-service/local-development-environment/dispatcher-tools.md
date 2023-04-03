---
title: 为AEMas a Cloud Service开发设置Dispatcher工具
description: AEM SDK的Dispatcher工具通过在本地安装、运行Dispatcher并排查其故障，帮助本地开发Adobe Experience Manager(AEM)项目。
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 9%

---

# 设置本地Dispatcher工具 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="本地 Dispatcher 工具"
>abstract="Dispatcher 是整个 Experience Manager 架构的组成部分，应该是本地开发设置的一部分。AEM as a Cloud Service SDK 包括推荐的 Dispatcher 工具版本，该版本有助于在本地配置、验证和模拟 Dispatcher。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html" text="云中的 Dispatcher"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下载 AEM as a Cloud Service SDK"

Adobe Experience Manager(AEM)的Dispatcher是一个Apache HTTP Web服务器模块，在CDN和AEM发布层之间提供一个安全和性能层。 Dispatcher 是整个 Experience Manager 架构的组成部分，应该是本地开发设置的一部分。

AEM as a Cloud Service SDK 包括推荐的 Dispatcher 工具版本，该版本有助于在本地配置、验证和模拟 Dispatcher。调度程序工具由以下部分组成：

+ 位于以下位置的一组基线Apache HTTP Web服务器和Dispatcher配置文件： `.../dispatcher-sdk-x.x.x/src`
+ 配置验证器CLI工具，位于 `.../dispatcher-sdk-x.x.x/bin/validate`
+ 配置生成CLI工具，位于 `.../dispatcher-sdk-x.x.x/bin/validator`
+ 配置部署CLI工具，位于 `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 不可更改的配置文件覆盖位于 `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ 使用Dispatcher模块运行Apache HTTP Web服务器的Docker图像

请注意 `~` 用作用户目录的简写形式。 在Windows中，这等同于 `%HOMEPATH%`.

>[!NOTE]
>
> 本页中的视频在macOS上录制。 Windows用户可以跟随，但使用随每个视频一起提供的对等的Dispatcher Tools Windows命令。

## 前提条件

1. Windows用户必须使用Windows 10 Professional（或支持Docker的版本）
1. 安装 [Experience Manager发布快速入门Jar](./aem-runtime.md) 在本地开发机器上。

+ （可选）安装最新版本 [AEM参考网站](https://github.com/adobe/aem-guides-wknd/releases) 本地AEM发布服务上的。 本教程中使用此网站可视化Dispatcher运行情况。

1. 安装并启动最新版本的 [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+)。

## 下载Dispatcher工具(作为AEM SDK的一部分)

AEMas a Cloud ServiceSDK或AEM SDK包含用于在本地使用调度程序模块运行Apache HTTP Web服务器以进行开发的调度程序工具，以及兼容的快速入门Jar。

如果AEMas a Cloud ServiceSDK已下载到 [设置本地AEM运行时](./aem-runtime.md)，则无需重新下载。

1. 登录到 [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) 与Adobe ID
   + 您的Adobe组织 __必须__ 配置AEMas a Cloud Service以下载AEMas a Cloud Service SDK
1. 单击最新 __AEM SDK__ 下载结果行

## 从AEM SDK zip解压缩Dispatcher工具

>[!TIP]
>
> Windows用户在包含本地Dispatcher工具的文件夹的路径中不能有任何空格或特殊字符。 如果路径中存在空格，则 `docker_run.cmd` 失败。

Dispatcher工具的版本与AEM SDK的版本不同。 确保Dispatcher工具的版本通过与AEMas a Cloud Service版本匹配的AEM SDK版本提供。

1. 解压缩下载的 `aem-sdk-xxx.zip` 文件
1. 将调度程序工具解压缩到 `~/aem-sdk/dispatcher`

+ Windows:解压缩 `aem-sdk-dispatcher-tools-x.x.x-windows.zip` into `C:\Users\<My User>\aem-sdk\dispatcher` （根据需要创建缺少的文件夹）
+ macOS Linux®:执行随附的Shell脚本 `aem-sdk-dispatcher-tools-x.x.x-unix.sh` 解包调度程序工具
   + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

下面发出的所有命令都假定当前工作目录包含扩展的调度程序工具内容。

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*此视频使用macOS作说明性用途。 可以使用等效的Windows/Linux命令来获得类似的结果。*

## 了解Dispatcher配置文件

>[!TIP]
> Experience Manager从 [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) 已预填充此Dispatcher配置文件集，因此无需从Dispatcher Tools src文件夹进行复制。

“调度程序工具”提供一组Apache HTTP Web服务器和调度程序配置文件，这些文件定义所有环境（包括本地开发）的行为。

这些文件将复制到Experience ManagerMaven项目中的 `dispatcher/src` 文件夹(如果Experience ManagerMaven项目中不存在)。

未打包的Dispatcher工具中提供了配置文件的完整说明，如 `dispatcher-sdk-x.x.x/docs/Config.html`.

## 验证配置

（可选）Dispatcher和Apache Web服务器配置(通过 `httpd -t`) `validate` 脚本(不要与 `validator` 可执行文件)。 的 `validate` 脚本提供了一种运行 [三个阶段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) 的 `validator`.

+ 用途:
   + Windows: `bin\validate src`
   + macOS Linux®: `./bin/validate.sh ./src`

## 在本地运行Dispatcher

AEM Dispatcher是使用Docker针对 `src` 调度程序和Apache Web服务器配置文件。

+ 用途:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS Linux®: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

的 `<aem-publish-host>` 可以设置为 `host.docker.internal`, Docker在容器中提供一个特殊的DNS名称，可解析为主机的IP。 如果 `host.docker.internal` 未解析，请查看 [疑难解答](#troubleshooting-host-docker-internal) 部分。

例如，使用Dispatcher工具提供的默认配置文件启动Dispatcher Docker容器：

启动Dispatcher Docker容器，提供Dispatcher配置src文件夹的路径：

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS Linux®: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

在端口4503本地运行的AEMas a Cloud ServiceSDK发布服务可通过Dispatcher()获取 `http://localhost:8080`.

要针对Experience Manager项目的Dispatcher配置运行Dispatcher工具，请指向项目的 `dispatcher/src` 文件夹。

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS Linux®:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher工具日志

在本地开发过程中，Dispatcher日志有助于了解HTTP请求是否被阻止以及为何被阻止。 日志级别可以通过预定执行 `docker_run` 和环境参数。

当 `docker_run` 运行。

用于调试Dispatcher的有用参数包括：

+ `DISP_LOG_LEVEL=Debug` 将调度程序模块日志记录设置为“调试”级别
   + 默认值为: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` 将Apache HTTP Web服务器重写模块日志记录设置为“调试”级别
   + 默认值为: `Warn`
+ `DISP_RUN_MODE` 设置Dispatcher环境的“运行模式”，加载相应的Dispatcher配置文件运行模式。
   + 默认为 `dev`
+ 有效值： `dev`, `stage`或 `prod`

可以将一个或多个参数传递到 `docker_run`

+ Windows:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

+ macOS Linux®:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

### 日志文件访问

可以在Docker容器中直接访问Apache Web服务器和AEM Dispatcher日志：

+ [访问Docker容器中的日志](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [将Docker日志复制到本地文件系统](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 何时更新Dispatcher工具{#dispatcher-tools-version}

Dispatcher工具版本的增加频率低于Experience Manager，因此Dispatcher工具在本地开发环境中需要的更新较少。

推荐的Dispatcher工具版本是与与Experience Manageras a Cloud Service版本匹配的AEM Dispatcher SDK捆绑在一起的版本。 AEMas a Cloud Service的版本可以通过 [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager >环境__，按 __AEM版本__ 标签

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

*请注意，调度程序工具版本与Experience Manager版本不匹配。*

## 如何更新Apache和Dispatcher配置的基线集

Apache和Dispatcher配置的基准集会定期得到增强，并随AEMas a Cloud ServiceSDK版本一起发布。 最佳做法是将基线配置增强功能整合到您的AEM项目中，并避免 [本地验证](#validate-configurations) 和Cloud Manager管道失败。 使用 `update_maven.sh` 脚本 `.../dispatcher-sdk-x.x.x/bin` 文件夹。

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*此视频使用macOS作说明性用途。 可以使用等效的Windows/Linux命令来获得类似的结果。*


假设您过去使用 [AEM项目原型](https://github.com/adobe/aem-project-archetype)，则基准Apache和Dispatcher配置是当前配置。 使用这些基线配置，通过重用和复制文件(如 `*.vhost`, `*.conf`, `*.farm` 和 `*.any` 从 `dispatcher/src/conf.d` 和 `dispatcher/src/conf.dispatcher.d` 文件夹。 您的本地Dispatcher验证和Cloud Manager管道运行正常。

同时，基线Apache和Dispatcher配置因各种原因（如新增功能、安全修复和优化）而得到增强。 它们是作为AEMas a Cloud Service版本的一部分通过较新版本的Dispatcher工具发布的。

现在，在根据最新的Dispatcher工具版本验证特定于项目的Dispatcher配置时，开始失败。 要解决此问题，需要使用以下步骤更新基线配置：

+ 验证验证是否针对最新的Dispatcher工具版本失败

   ```shell
   $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Phase 3: Immutability check
   empty mode param, assuming mode = 'check'
   ...
   ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
   ```

+ 使用 `update_maven.sh` script

   ```shell
   $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Updating dispatcher configuration at folder 
   running in 'extract' mode
   running in 'extract' mode
   reading immutable file list from /etc/httpd/immutable.files.txt
   preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
   ...
   immutable files extraction COMPLETE
   fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
   Cloud manager validator 2.0.53
   ```

+ 验证更新的不可变文件，如 `dispatcher_vhost.conf`, `default.vhost`和 `default.farm` 和（如果需要）对从这些文件派生的自定义文件进行相关更改。

+ 重新验证配置，应通过

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ 在本地验证更改后，提交更新的配置文件

## 疑难解答

### docker_run导致“等待host.docker.internal可用”消息{#troubleshooting-host-docker-internal}

的 `host.docker.internal` 是提供给Docker包含的可解析到主机的主机名。 根据docs.docker.com([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> 从Docker 18.03开始，建议连接到特殊的DNS名称host.docker.internal，该名称解析为主机使用的内部IP地址

When `bin/docker_run src host.docker.internal:4503 8080` 结果显示消息 __等待host.docker.internal可用__，则：

1. 确保已安装的Docker版本为18.03或更高版本
2. 您可能已设置本地计算机，该计算机会阻止 `host.docker.internal` 名称。 请改用本地IP。
   + Windows:
   + 在命令提示符下，执行 `ipconfig`，并记录主机的 __IPv4地址__ 主机的。
   + 然后，执行 `docker_run` 使用此IP地址：
      `bin\docker_run src <HOST IP>:4503 8080`
   + macOS Linux®:
   + 从终端执行 `ifconfig` 并记录主机 __inet__ IP地址，通常为 __en0__ 设备。
   + 然后执行 `docker_run` 使用主机IP地址：
      `bin/docker_run.sh src <HOST IP>:4503 8080`

#### 示例错误

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## 其他资源

+ [下载AEM SDK](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [下载AEM参考网站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience ManagerDispatcher文档](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)
