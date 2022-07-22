---
title: 设置本地AEM运行时以进行AEMas a Cloud Service开发
description: 使用AEMas a Cloud ServiceSDK的快速入门Jar设置本地AEM运行时。
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '1801'
ht-degree: 3%

---

# 设置本地AEM运行时 {#set-up-local-aem-runtime}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本地AEM运行时"
>abstract="Adobe Experience Manager(AEM)可以使用AEMas a Cloud Service SDK的快速入门Jar在本地运行。 这允许开发人员在将自定义代码、配置和内容提交到源控件之前，先对其进行部署和测试，然后将其部署到AEMas a Cloud Service环境。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下载AEMas a Cloud ServiceSDK"

Adobe Experience Manager(AEM)可以使用AEMas a Cloud Service SDK的快速入门Jar在本地运行。 这允许开发人员在将自定义代码、配置和内容提交到源控件之前，先对其进行部署和测试，然后将其部署到AEMas a Cloud Service环境。

请注意 `~` 用作用户目录的简写形式。 在Windows中，这等同于 `%HOMEPATH%`.

## 安装Java

Experience Manager是一个Java应用程序，因此需要Java SDK支持开发工具。

1. [下载并安装最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;by.sort=desc&amp;list=0&amp;p.offset=14)
1. 通过运行以下命令验证是否已安装Java 11 SDK:
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## 下载AEMas a Cloud ServiceSDK

AEMas a Cloud ServiceSDK或AEM SDK包含用于在本地运行AEM创作和发布以进行开发的快速入门Jar，以及兼容版本的调度程序工具。

1. 登录到 [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) 与Adobe ID
   + 请注意，您的Adobe组织 __必须__ 配置AEMas a Cloud Service以下载AEMas a Cloud Service SDK。
1. 导航到 __AEMas a Cloud Service__ 选项卡
1. 排序依据 __发布日期__ in __降序__ 订购
1. 单击最新 __AEM SDK__ 结果行
1. 查看并接受EULA，然后点按 __下载__ 按钮

## 从AEM SDK zip解压缩快速入门Jar

1. 解压缩下载的 `aem-sdk-XXX.zip` 文件

## 设置本地AEM创作服务{#set-up-local-aem-author-service}

本地AEM创作服务为开发人员提供将共享以创建和管理内容的本地体验数字营销人员/内容作者。  AEM创作服务既是创作环境，又是预览环境，因此可以对其执行大多数功能开发验证，从而使其成为本地开发流程的关键元素。

1. 创建文件夹 `~/aem-sdk/author`
1. 复制 __快速入门JAR__ 文件到  `~/aem-sdk/author` 将其重命名为 `aem-author-p4502.jar`
1. 通过从命令行执行以下命令来启动本地AEM创作服务：
   + `java -jar aem-author-p4502.jar`
      + 将管理员密码提供为 `admin`. 可接受任何管理员密码，但建议将默认密码用于本地开发，以减少重新配置的需要。

   您 *无法* 将AEM启动为Cloud Service快速入门Jar [通过双击](#troubleshooting-double-click).
1. 访问本地AEM创作服务： [http://localhost:4502](http://localhost:4502) 在Web浏览器中

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 设置本地AEM发布服务

本地AEM发布服务为开发人员提供AEM的本地体验最终用户将拥有的体验，例如浏览托管在AEM上的网站。 本地AEM发布服务很重要，因为它与AEM SDK的集成 [调度程序工具](./dispatcher-tools.md) 并允许开发人员对面向最终用户的最终体验进行烟度测试和微调。

1. 创建文件夹 `~/aem-sdk/publish`
1. 复制 __快速入门JAR__ 文件到  `~/aem-sdk/publish` 将其重命名为 `aem-publish-p4503.jar`
1. 通过从命令行执行以下命令来启动本地AEM发布服务：
   + `java -jar aem-publish-p4503.jar`
      + 将管理员密码提供为 `admin`. 可接受任何管理员密码，但建议将默认密码用于本地开发，以减少重新配置的需要。

   您 *无法* 将AEM启动为Cloud Service快速入门Jar [通过双击](#troubleshooting-double-click).
1. 访问本地AEM发布服务： [http://localhost:4503](http://localhost:4503) 在Web浏览器中

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 在预发行模式下设置本地AEM服务

本地AEM运行时可在 [预发行模式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html) 允许开发人员根据AEM as a Cloud Service下一版本的功能进行构建。 通过传递 `-r prerelease` 本地AEM运行时的首次开始时的参数。 这可以与本地AEM创作和AEM发布服务一起使用。

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

## 模拟内容分发 {#content-distribution}

在真正的Cloud Service环境中，使用 [Sling内容分发](https://sling.apache.org/documentation/bundles/content-distribution.html) 和Adobe管道。 的 [Adobe管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) 是仅在云环境中可用的独立微服务。

在开发过程中，可能希望使用本地创作和发布服务来模拟内容的分发。 这可以通过启用旧版复制代理来实现。

>[!NOTE]
>
> 复制代理仅可在本地快速入门JAR中使用，并仅提供内容分发的模拟。

1. 登录到 **作者** 服务和导航到 [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. 单击 **默认代理（发布）** 打开默认复制代理。
1. 单击 **编辑** 打开代理的配置。
1. 在 **设置** ，请更新以下字段：

   + **已启用**  — 检查true
   + **代理用户Id**  — 将此字段留空

   ![复制代理配置 — 设置](assets/aem-runtime/settings-config.png)

1. 在 **运输** ，请更新以下字段：

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **用户** - `admin`
   + **密码** - `admin`

   ![复制代理配置 — 传输](assets/aem-runtime/transport-config.png)

1. 单击 **确定** 保存配置并启用 **默认** 复制代理。
1. 您现在可以对创作服务上的内容进行更改，然后将其发布到发布服务。

![发布页面](assets/aem-runtime/publish-page-changes.png)

## 快速入门Jar启动模式

快速入门Jar的命名， `aem-<tier>_<environment>-p<port number>.jar` 指定启动方式。 在特定层、创作或发布中启动AEM后，便无法将其更改为替代层。 为此， `crx-Quickstart` 必须删除在首次运行期间生成的文件夹，并且必须再次运行快速入门Jar。 可以更改环境和端口，但需要停止/启动本地AEM实例。

更改环境， `dev`, `stage` 和 `prod`，对于开发人员而言，这有助于确保AEM正确定义和解析特定于环境的配置。 建议主要在默认情况下完成本地开发 `dev` 环境运行模式。

可用的排列如下：

| 快速入门Jar文件名 | 模式描述 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | 作为作者在端口4502的开发运行模式下 |
| `aem-author_dev-p4502.jar` | 作为在端口4502上的开发运行模式下的作者(与 `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | 作为作者在端口4502的暂存运行模式下 |
| `aem-author_prod-p4502.jar` | 作为作者在端口4502的生产运行模式下 |
| `aem-publish-p4503.jar` | 在端口4503上以开发运行模式发布 |
| `aem-publish_dev-p4503.jar` | 在端口4503上以开发运行模式发布(与 `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | 在端口4503上以暂存运行模式发布 |
| `aem-publish_prod-p4503.jar` | 在端口4503上以生产运行模式发布 |

请注意，端口号可以是本地开发计算机上的任何可用端口，但按惯例：

+ 端口 __4502__ 用于 __本地AEM创作服务__
+ 端口 __4503__ 用于 __本地AEM发布服务__

更改这些配置可能需要对AEM SDK配置进行调整

## 停止本地AEM运行时

要停止本地AEM运行时（AEM创作或发布服务），请打开用于启动AEM运行时的命令行窗口，然后点按 `Ctrl-C`. 等待AEM关闭。 关闭过程完成后，将提供命令行提示符。

## 可选的本地AEM运行时设置任务

+ __OSGi配置环境变量和密钥变量__ are [专门为AEM本地运行时设置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)，而不是使用aio CLI管理。

## 何时更新快速入门Jar

每月最后一个星期四(即AEMas a Cloud Service“功能发布”的发行频率)，即至少每月或之后不久更新AEM SDK。

>[!WARNING]
>
> 将快速入门Jar更新为新版本需要替换整个本地开发环境，从而导致本地AEM存储库中的所有代码、配置和内容丢失。 确保任何不应被销毁的代码、配置或内容都安全地提交到Git，或从本地AEM实例中导出为AEM Packages。

### 如何避免在升级AEM SDK时丢失内容

升级AEM SDK实际上是在创建一个全新的AEM运行时，包括一个新的存储库，这意味着对之前AEM SDK的存储库所做的任何更改都将丢失。 以下是有助于在AEM SDK升级期间保留内容的可行策略，可以单独使用或协同使用：

1. 创建专用于包含“示例”内容的内容包，以便用于开发，并在Git中对其进行维护。 通过AEM SDK升级应保留的任何内容都将保留在此包中，并在升级AEM SDK后重新部署。
1. 使用 [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) 和 `includepaths` 指令，将内容从之前的AEM SDK存储库复制到新的AEM SDK存储库。
1. 使用AEM包管理器备份任何内容以及之前AEM SDK中的内容包，然后在新的AEM SDK中重新安装它们。

请记住，使用上述方法在AEM SDK升级期间维护代码，表示开发存在反模式问题。 非可弃用代码应源自您的开发IDE，并通过部署流入AEM SDK。

## 疑难解答

### 双击快速入门Jar文件会导致错误{#troubleshooting-double-click}

双击要启动的快速入门Jar时，会显示一个错误模式，阻止AEM从本地启动。

![疑难解答 — 双击快速入门Jar文件](./assets/aem-runtime/troubleshooting__double-click.png)

这是因为AEMas a Cloud Service快速入门Jar不支持双击快速入门Jar以在本地启动AEM。 而是必须从该命令行运行Jar文件。

要启动AEM创作服务，请执行以下操作： `cd` 进入包含快速入门Jar的目录，然后执行命令：

`$ java -jar aem-author-p4502.jar`

或者，要启动AEM发布服务， `cd` 进入包含快速入门Jar的目录，然后执行命令：

`$ java -jar aem-publish-p4503.jar`

### 从命令行启动快速入门Jar会立即中止{#troubleshooting-java-8}

从命令行启动快速入门Jar时，进程会立即中止，并且AEM服务不会启动，并出现以下错误：

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

这是因为AEM  as a Cloud Service需要Java SDK 11，并且您运行的是不同的版本，很可能是Java 8。 要解决此问题，请下载并安装 [OracleJava SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;by.sort=desc&amp;list=0&amp;p.offset=14).
安装Java SDK 11后，通过从命令行执行以下命令来验证它是活动版本。

安装Java 11 SDK后，通过从命令行中运行命令来验证它是活动版本：

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## 其他资源

+ [下载AEM SDK](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [Experience ManagerDispatcher文档](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hans)
