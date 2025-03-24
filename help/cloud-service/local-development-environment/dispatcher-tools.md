---
title: 为AEM as a Cloud Service开发设置Dispatcher工具
description: AEM SDK的Dispatcher Tools通过简化本地安装、运行和故障诊断Adobe Experience Manager (AEM)项目的过程，为Dispatcher的本地开发提供了便利。
version: Experience Manager as a Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 4%

---

# 设置本地 Dispatcher 工具 {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="本地 Dispatcher 工具"
>abstract="Dispatcher 是整个 Experience Manager 架构的组成部分，应该是本地开发设置的一部分。AEM as a Cloud Service SDK 包括推荐的 Dispatcher 工具版本，该版本有助于在本地配置、验证和模拟 Dispatcher。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html" text="Cloud 中的 Dispatcher"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html" text="下载 AEM as a Cloud Service SDK"

Adobe Experience Manager (AEM)的Dispatcher是一个Apache HTTP Web服务器模块，在CDN和AEM发布层之间提供了一个安全和性能层。 Dispatcher是Experience Manager整体架构的组成部分，应当是本地开发设置的一部分。

AEM as a Cloud Service SDK包括推荐的Dispatcher工具版本，该版本有助于在本地配置验证和模拟Dispatcher。 Dispatcher Tools由以下部分组成：

+ 位于`.../dispatcher-sdk-x.x.x/src`的Apache HTTP Web Server和Dispatcher配置文件的基线集
+ 配置验证器CLI工具，位于`.../dispatcher-sdk-x.x.x/bin/validate`
+ 配置生成CLI工具，位于`.../dispatcher-sdk-x.x.x/bin/validator`
+ 配置部署CLI工具，位于`.../dispatcher-sdk-x.x.x/bin/docker_run`
+ 不可变配置文件覆盖CLI工具，位于`.../dispatcher-sdk-x.x.x/bin/update_maven`
+ 使用Dispatcher模块运行Apache HTTP Web Server的Docker图像

请注意，`~`用作用户目录的简写。 在Windows中，这相当于`%HOMEPATH%`。

>[!NOTE]
>
> 本页中的视频是在macOS上录制的。 Windows用户可以跟随，但使用随每个视频提供的等效的Dispatcher Tools Windows命令。

## 先决条件

1. Windows用户必须使用Windows 10专业版（或支持Docker的版本）
1. 在本地开发计算机上安装[Experience Manager发布快速入门Jar](./aem-runtime.md)。

+ 或者，在本地AEM Publish服务上安装最新的[AEM引用网站](https://github.com/adobe/aem-guides-wknd/releases)。 本教程将使用此网站来可视化正在运行的Dispatcher。

1. 在本地开发计算机上安装并启动最新版本的[Docker](https://www.docker.com/)（Docker Desktop 2.2.0.5+ / Docker引擎v19.03.9+）。

## 下载Dispatcher工具(作为AEM SDK的一部分)

AEM as a Cloud Service SDK(或AEM SDK)包含用于在本机运行带有Dispatcher模块的Apache HTTP Web服务器以进行开发的Dispatcher工具，以及兼容的快速入门Jar。

如果已将AEM as a Cloud Service SDK下载到[设置本地AEM运行时](./aem-runtime.md)，则无需重新下载。

1. 使用您的Adobe ID登录到[experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
   + 必须针对您的Adobe组织&#x200B;__进行__&#x200B;配置以便AEM as a Cloud Service下载AEM as a Cloud Service SDK
1. 单击要下载的最新&#x200B;__AEM SDK__&#x200B;结果行

## 从AEM SDK zip文件中提取Dispatcher工具

>[!TIP]
>
> Windows用户在包含本地Dispatcher工具的文件夹的路径中不能有任何空格或特殊字符。 如果路径中存在空格，`docker_run.cmd`将失败。

Dispatcher Tools的版本与AEM SDK的版本不同。 确保通过与Dispatcher版本匹配的AEM SDK版本提供了AEM as a Cloud Service Tools的版本。

1. 解压缩下载的`aem-sdk-xxx.zip`文件
1. 将Dispatcher工具解压缩到`~/aem-sdk/dispatcher`中

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

将`aem-sdk-dispatcher-tools-x.x.x-windows.zip`解压缩到`C:\Users\<My User>\aem-sdk\dispatcher`中（根据需要创建缺少的文件夹）。

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

下面发出的所有命令都假定当前工作目录包含扩展的Dispatcher Tools内容。

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*本视频使用macOS进行说明。 可使用等效的Windows/Linux命令获得类似的结果。*

## 了解Dispatcher配置文件

>[!TIP]
> 从[Experience Manager项目Maven Archetype](https://github.com/adobe/aem-project-archetype)创建的AEM项目已预填充这组Dispatcher配置文件，因此无需从Dispatcher Tools src文件夹进行复制。

Dispatcher Tools提供了一组Apache HTTP Web Server和Dispatcher配置文件，这些文件定义所有环境（包括本地开发）的行为。

这些文件拟复制到Experience Manager Maven项目的`dispatcher/src`文件夹中(如果Experience Manager Maven项目中不存在这些文件)。

在解压缩的Dispatcher Tools中，配置文件的完整说明以`dispatcher-sdk-x.x.x/docs/Config.html`形式提供。

## 验证配置

或者，可以使用`validate`脚本验证Dispatcher和Apache Web服务器配置（通过`httpd -t`）（不要与`validator`可执行文件混淆）。 `validate`脚本提供了一种运行`validator`的[三个阶段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en)的简便方法。


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## 在本地运行Dispatcher

AEM Dispatcher是针对`src` Dispatcher和Apache Web Server配置文件使用Docker在本地运行的。


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run_hot_reload`可执行文件优先于`docker_run`，因为它在配置文件更改时重新加载配置文件，而无需手动终止和重新启动`docker_run`。 或者，可以使用`docker_run`，但是当配置文件发生更改时，需要手动终止和重新启动`docker_run`。

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

`docker_run_hot_reload`可执行文件优先于`docker_run`，因为它在配置文件更改时重新加载配置文件，而无需手动终止和重新启动`docker_run`。 或者，可以使用`docker_run`，但是当配置文件发生更改时，需要手动终止和重新启动`docker_run`。

>[!ENDTABS]

可以将`<aem-publish-host>`设置为`host.docker.internal`，这是解析为主机的IP的容器中由Docker提供的特殊DNS名称。 如果`host.docker.internal`无法解析，请参阅下面的[疑难解答](#troubleshooting-host-docker-internal)部分。

例如，要使用Dispatcher工具提供的默认配置文件启动Dispatcher Docker容器，请执行以下操作：

启动Dispatcher Docker容器，提供Dispatcher配置src文件夹的路径：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

AEM as a Cloud Service SDK的发布服务在端口4503上本地运行，可以通过Dispatcher在`http://localhost:8080`处获取。

要针对Experience Manager项目的Dispatcher配置运行Dispatcher工具，请指向项目的`dispatcher/src`文件夹。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Dispatcher Tools日志

在本地开发期间，Dispatcher日志有助于了解HTTP请求是否被阻止以及为什么被阻止。 可以通过为执行`docker_run`添加环境参数前缀来设置日志级别。

运行`docker_run`时，Dispatcher Tools日志将发出给标准。

用于调试Dispatcher的有用参数包括：

+ `DISP_LOG_LEVEL=Debug`将Dispatcher模块日志记录设置为“调试”级别
   + 默认值为： `Warn`
+ `REWRITE_LOG_LEVEL=Debug`将Apache HTTP Web服务器重写模块日志记录设置为调试级别
   + 默认值为： `Warn`
+ `DISP_RUN_MODE`设置Dispatcher环境的“运行模式”，加载相应的运行模式Dispatcher配置文件。
   + 默认为`dev`
+ 有效值： `dev`、`stage`或`prod`

一个或多个参数可以传递给`docker_run`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### 日志文件访问

可直接在Docker容器中访问Apache Web Server和AEM Dispatcher日志：

+ [访问Docker容器中的日志](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [将Docker日志复制到本地文件系统](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## 何时更新Dispatcher工具{#dispatcher-tools-version}

Dispatcher工具版本的增量频率低于Experience Manager，因此Dispatcher工具在本地开发环境中所需的更新较少。

推荐的Dispatcher Tools版本是与Experience Manager as a Cloud Service版本匹配的AEM as a Cloud Service SDK捆绑在一起的版本。 可以通过[Cloud Manager](https://my.cloudmanager.adobe.com/)找到AEM as a Cloud Service的版本。

+ __Cloud Manager >环境__，按&#x200B;__AEM版本__&#x200B;标签指定的环境

![Experience Manager版本](./assets/dispatcher-tools/aem-version.png)

*请注意，Dispatcher Tools版本与Experience Manager版本不匹配。*

## 如何更新Apache和Dispatcher配置的基线集

Apache和Dispatcher配置的基线集已定期得到增强，并随AEM as a Cloud Service SDK版本一起发布。 最佳做法是将基线配置增强功能合并到您的AEM项目中，并避免[本地验证](#validate-configurations)和Cloud Manager管道故障。 使用`.../dispatcher-sdk-x.x.x/bin`文件夹中的`update_maven.sh`脚本更新它们。

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*本视频使用macOS进行说明。 可使用等效的Windows/Linux命令获得类似的结果。*


假设您以前使用[AEM项目原型](https://github.com/adobe/aem-project-archetype)创建了一个AEM项目，基准Apache和Dispatcher配置是最新的。 使用这些基线配置，通过重复使用`dispatcher/src/conf.d`和`dispatcher/src/conf.dispatcher.d`文件夹中的文件，并复制`*.vhost`、`*.conf`、`*.farm`和`*.any`等文件，创建了项目特定的配置。 您的本地Dispatcher验证和Cloud Manager管道工作正常。

同时，由于新增功能、安全修复和优化等多种原因，基准Apache和Dispatcher配置得到了增强。 它们通过作为AEM as a Cloud Service版本一部分的Dispatcher Tools的较新版本发布。

现在，针对最新的Dispatcher工具版本验证特定于项目的Dispatcher配置时，这些配置会开始失败。 要解决此问题，需要使用以下步骤更新基线配置：

+ 验证针对最新Dispatcher Tools版本的验证是否失败

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ 使用`update_maven.sh`脚本更新不可变文件

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

+ 验证已更新的不可变文件，如`dispatcher_vhost.conf`、`default.vhost`和`default.farm`，并根据需要在自定义文件中进行相关更改（这些更改派生自这些文件）。

+ 重新验证配置，它应该会通过

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

`host.docker.internal`是提供给Docker包含的主机名，可解析为主机。 每个docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/)，[Windows](https://docs.docker.com/desktop/networking/))：

> 从Docker 18.03开始，建议连接到特殊的DNS名称host.docker.internal，它解析为主机使用的内部IP地址

当`bin/docker_run src host.docker.internal:4503 8080`导致消息&#x200B;__等待host.docker.internal可用时__，然后：

1. 确保Docker的安装版本为18.03或更高版本
1. 您可能设置了本地计算机，阻止注册/解析`host.docker.internal`名称。 请改用本地IP。

>[!BEGINTABS]

>[!TAB macOS]

+ 在终端中，执行`ifconfig`并记录主机&#x200B;__inet__ IP地址，通常是&#x200B;__en0__&#x200B;设备。
+ 然后使用主机IP地址执行`docker_run`： `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ 在命令提示符下，执行`ipconfig`并记录主机的&#x200B;__IPv4地址__。
+ 然后，使用此IP地址执行`docker_run`： `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ 在终端中，执行`ifconfig`并记录主机&#x200B;__inet__ IP地址，通常是&#x200B;__en0__&#x200B;设备。
+ 然后使用主机IP地址执行`docker_run`： `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

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
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [下载AEM参考网站(WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher文档](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hans)
