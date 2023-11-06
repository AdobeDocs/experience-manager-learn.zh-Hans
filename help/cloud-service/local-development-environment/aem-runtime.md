---
title: 为AEMas a Cloud Service开发设置本地AEM SDK
description: 使用AEMas a Cloud ServiceSDK的快速入门Jar设置本地AEM SDK运行时。
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: 2a412126ac7a67a756d4101d56c1715f0da86453
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 10%

---

# 设置本地 AEM SDK {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本地 AEM 运行时"
>abstract="Adobe Experience Manager (AEM) 可以使用 AEM as a Cloud Service SDK 的快速入门 Jar 在本地运行。这允许开发人员在将自定义代码、配置和内容提交到源控件之前，先对它们进行部署和测试，然后将其部署到 AEM as a Cloud Service 环境。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下载 AEM as a Cloud Service SDK"

Adobe Experience Manager (AEM) 可以使用 AEM as a Cloud Service SDK 的快速入门 Jar 在本地运行。这允许开发人员在将自定义代码、配置和内容提交到源控件之前，先对它们进行部署和测试，然后将其部署到 AEM as a Cloud Service 环境。

请注意 `~` 用作用户目录的简写。 在Windows中，这等同于 `%HOMEPATH%`.

## 安装Java

Experience Manager是一种Java应用程序，因此需要OracleJava SDK支持开发工具。

1. [下载并安装最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14444)
1. 通过运行以下命令验证是否已安装OracleJava 11 SDK：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## 下载AEMas a Cloud ServiceSDK

AEMas a Cloud ServiceSDK(或AEM SDK)包含用于在本地运行AEM创作和发布以进行开发的快速入门Jar，以及兼容版本的Dispatcher工具。

1. 登录 [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) 使用您的Adobe ID
   + 请注意，您的Adobe组织 __必须__ 配置AEMas a Cloud Service以下载AEMas a Cloud ServiceSDK。
1. 导航至 __AEMas a Cloud Service__ 选项卡
1. 排序方式 __发布日期__ 在 __降序__ 订购
1. 单击最新的 __AEM SDK__ 结果行
1. 查看并接受EULA ，然后点击 __下载__ 按钮

## 从AEM SDK zip文件中提取快速入门Jar

1. 解压缩下载的 `aem-sdk-XXX.zip` 文件

## 设置本地AEM创作服务{#set-up-local-aem-author-service}

本地AEM创作服务为开发人员提供了一个本地体验，数字营销人员/内容作者可以共享该体验来创建和管理内容。  AEM Author Service设计作为创作和预览环境，允许可以针对它执行大多数功能开发验证，使其成为本地开发过程的重要元素。

1. 创建文件夹 `~/aem-sdk/author`
1. 复制 __快速入门JAR__ 文件到  `~/aem-sdk/author` 并将其重命名为 `aem-author-p4502.jar`
1. 通过从命令行执行以下命令来启动本地AEM Author Service：
   + `java -jar aem-author-p4502.jar`
      + 提供管理员密码作为 `admin`. 可接受任何管理员密码，但建议使用默认密码进行本地开发，以减少重新配置的需要。

   您 *无法* 启动AEM作为Cloud Service快速入门Jar [通过双击](#troubleshooting-double-click).
1. 访问本地AEM创作服务，网址为 [http://localhost:4502](http://localhost:4502) 在Web浏览器中

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## 设置本地AEM Publish服务

本地AEM Publish Service为开发人员提供了AEM的本地体验最终用户，例如浏览基于AEM的网站。 本地AEM发布服务非常重要，因为它与AEM SDK的集成 [Dispatcher工具](./dispatcher-tools.md) 并允许开发人员对面向最终用户的体验进行抽烟测试和微调。

1. 创建文件夹 `~/aem-sdk/publish`
1. 复制 __快速入门JAR__ 文件到  `~/aem-sdk/publish` 并将其重命名为 `aem-publish-p4503.jar`
1. 通过从命令行执行以下操作来启动本地AEM Publish Service：
   + `java -jar aem-publish-p4503.jar`
      + 提供管理员密码作为 `admin`. 可接受任何管理员密码，但建议使用默认密码进行本地开发，以减少重新配置的需要。

   您 *无法* 启动AEM作为Cloud Service快速入门Jar [通过双击](#troubleshooting-double-click).
1. 访问本地AEM发布服务，网址为 [http://localhost:4503](http://localhost:4503) 在Web浏览器中

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## 在预发行模式中设置本地AEM服务

可以在中启动本地AEM运行时 [预发行模式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html) 允许开发人员针对AEMas a Cloud Service的下一个版本的功能进行构建。 通过传递 `-r prerelease` 本地AEM运行时的第一个启动时的参数。 这可以同时用于本地AEM Author和AEM Publish服务。


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## 模拟内容分发 {#content-distribution}

在真实的Cloud Service环境中，使用将内容从Author服务分发到Publish服务 [Sling内容分发](https://sling.apache.org/documentation/bundles/content-distribution.html) 和Adobe管道。 此 [Adobe管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) 是一个仅在云环境中可用的独立微服务。

在开发期间，可能需要使用本地Author和Publish服务模拟内容的分发。 这可以通过启用旧版复制代理来实现。

>[!NOTE]
>
> 复制代理只能在本地Quickstart JAR中使用，并且只能提供内容分发的模拟。

1. 登录到 **作者** 服务并导航到 [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. 单击 **默认代理（发布）** 以打开默认复制代理。
1. 单击 **编辑** 以打开代理的配置。
1. 在 **设置** 选项卡，更新以下字段：

   + **已启用**  — 检查true
   + **代理用户ID**  — 将此字段留空

   ![复制代理配置 — 设置](assets/aem-runtime/settings-config.png)

1. 在 **传输** 选项卡，更新以下字段：

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **用户** - `admin`
   + **密码** - `admin`

   ![复制代理配置 — 传输](assets/aem-runtime/transport-config.png)

1. 单击 **确定** 保存配置并启用 **默认** 复制代理。
1. 您现在可以更改Author服务上的内容并将其发布到Publish服务。

![发布页面](assets/aem-runtime/publish-page-changes.png)

## 快速入门Jar启动模式

快速入门Jar的命名， `aem-<tier>_<environment>-p<port number>.jar` 指定启动方式。 AEM在特定层、创作层或发布层启动后，无法更改为备用层。 要执行此操作， `crx-Quickstart` 必须删除首次运行时生成的文件夹，并且必须再次运行快速入门Jar。 可以更改环境和端口，但是它们需要停止/启动本地AEM实例。

不断变化的环境， `dev`， `stage` 和 `prod`，对于开发人员而言，能够确保AEM正确定义并解析特定于环境的配置。 建议主要针对默认情况执行本地开发 `dev` 环境运行模式。

可用的排列如下：

| 快速入门Jar文件名 | 模式描述 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | 作为作者，在端口4502上处于开发运行模式 |
| `aem-author_dev-p4502.jar` | 作为作者，在端口4502上处于开发运行模式(与 `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | 作为作者，在端口4502上处于暂存运行模式 |
| `aem-author_prod-p4502.jar` | 作为作者，在端口4502上处于生产运行模式 |
| `aem-publish-p4503.jar` | 在端口4503上以开发运行模式作为发布 |
| `aem-publish_dev-p4503.jar` | 在端口4503上作为开发运行模式下的发布(与 `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | 在端口4503上以暂存运行模式作为发布 |
| `aem-publish_prod-p4503.jar` | 在端口4503上以生产运行模式作为发布 |

请注意，端口号可以是本地开发计算机上的任何可用端口，但按照惯例可以：

+ 端口 __4502__ 用于 __本地AEM Author服务__
+ 端口 __4503__ 用于 __本地AEM Publish服务__

更改这些配置文件可能需要调整AEM SDK配置

## 停止本地AEM运行时

要停止本地AEM运行时(AEM Author或Publish服务)，请打开用于启动AEM运行时的命令行窗口，然后点击 `Ctrl-C`. 等待AEM关闭。 当关闭过程完成时，命令行提示符可用。

## 可选的本地AEM运行时设置任务

+ __OSGi配置环境变量和机密变量__ 是 [专门为AEM本地运行时设置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)，而不是使用aio CLI管理它们。

## 何时更新快速入门Jar

在每月的最后一个星期四或之后不久，至少每月更新AEM SDK，这是AEMas a Cloud Service“功能发布”的发布节奏。

>[!WARNING]
>
> 将快速入门Jar更新为新版本需要替换整个本地开发环境，从而导致本地AEM存储库中所有代码、配置和内容丢失。 确保任何不应销毁的代码、配置或内容都会安全提交到Git，或从本地AEM实例导出为AEM包。

### 如何避免在升级AEM SDK时丢失内容

升级AEM SDK会有效地创建一个全新的AEM运行时，其中包括一个新的存储库，这意味着对先前AEM SDK的存储库所做的任何更改都将丢失。 以下是帮助在AEM SDK升级之间保留内容的可行策略，可以单独或一致使用：

1. 创建一个专门用于包含“示例”内容的内容包，以帮助进行开发，并在Git中对其进行维护。 任何应通过AEM SDK升级保留的内容将保留在此包中，并在升级AEM SDK后重新部署。
1. 使用 [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) 使用 `includepaths` 指令，将内容从之前的AEM SDK存储库复制到新的AEM SDK存储库。
1. 使用以前的AEM SDK上的AEM包管理器和内容包备份任何内容，并在新的AEM SDK上重新安装它们。

请记住，在AEM SDK升级之间使用上述方法维护代码，表明开发存在反模式。 非一次性代码应源自开发IDE，并通过部署流入AEM SDK。

## 疑难解答

### 双击快速入门Jar文件会导致错误{#troubleshooting-double-click}

双击快速入门Jar启动时，显示错误模式，阻止AEM本地启动。

![故障排除 — 双击“快速入门Jar”文件](./assets/aem-runtime/troubleshooting__double-click.png)

这是因为AEMas a Cloud Service快速入门Jar不支持双击快速入门Jar以本地启动AEM。 相反，必须从该命令行运行Jar文件。

要启动AEM Author服务， `cd` 进入包含Quickstart Jar的目录并执行命令：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

或者，要启动AEM Publish服务， `cd` 进入包含Quickstart Jar的目录并执行命令：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4503.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-author-p4503.jar
```

>[!ENDTABS]

### 从命令行启动快速入门Jar会立即中止{#troubleshooting-java-8}

从命令行启动快速入门Jar时，进程立即中止，AEM服务未启动，并出现以下错误：

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

这是因为AEMas a Cloud Service需要Java SDK 11，而您运行的是其他版本，很可能是Java 8。 要解决此问题，请下载并安装 [oracleJava SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14444).

安装OracleJava 11 SDK后，通过从命令行运行命令来验证它是活动版本：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

## 其他资源

+ [下载AEM SDK](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [Experience ManagerDispatcher文档](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)
