---
title: 为AEM开发设置本地AEM as a Cloud Service SDK
description: 使用AEM SDK的快速入门Jar设置本地AEM as a Cloud Service SDK运行时。
feature: Developer Tools
version: Experience Manager as a Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: 99e3cadc71ca4e26f9e4034085788dfc5407d1bb
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 7%

---

# 设置本地 AEM SDK {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本地 AEM 运行时"
>abstract="Adobe Experience Manager (AEM) 可以使用 AEM as a Cloud Service SDK 的快速入门 Jar 在本地运行。这样开发人员即可先部署到和测试自定义代码、配置和内容，然后再将它提交到源代码管理以及将它部署到 AEM as a Cloud Service 环境。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=zh-Hans" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下载 AEM as a Cloud Service SDK"

Adobe Experience Manager (AEM) 可以使用 AEM as a Cloud Service SDK 的快速入门 Jar 在本地运行。这样开发人员即可先部署到和测试自定义代码、配置和内容，然后再将它提交到源代码管理以及将它部署到 AEM as a Cloud Service 环境。

请注意，`~`用作用户目录的简写。 在Windows中，这相当于`%HOMEPATH%`。

## 安装Java™

Experience Manager是一种Java™应用程序，因此需要Oracle Java™ SDK支持开发工具。

1. [下载并安装最新的Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&fulltext=Oracle%7E+JDK%7E+11%7E&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=list&p.offset=0&p.limit=14&p.limit=144)
1. 通过运行以下命令，验证是否已安装Oracle Java™ 11 SDK：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## 下载AEM as a Cloud Service SDK

AEM as a Cloud Service SDK(或AEM SDK)包含用于在本机运行AEM创作和发布以进行开发的快速入门Jar，以及Dispatcher Tools的兼容版本。

1. 使用您的Adobe ID登录到[https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)
   + 请注意，要下载Adobe SDK，必须&#x200B;__配置AEM as a Cloud Service的AEM as a Cloud Service组织__。
1. 导航到&#x200B;__AEM as a Cloud Service__&#x200B;选项卡
1. 按&#x200B;__发布日期__&#x200B;排序，顺序为&#x200B;__降序__
1. 单击最新的&#x200B;__AEM SDK__&#x200B;结果行
1. 查看并接受EULA，然后点按&#x200B;__下载__&#x200B;按钮

## 从AEM SDK zip文件中提取快速入门Jar

1. 解压缩下载的`aem-sdk-XXX.zip`文件

## 设置本地AEM创作服务{#set-up-local-aem-author-service}

本地AEM创作服务为开发人员提供本地体验，数字营销人员/内容作者可以共享该体验来创建和管理内容。  AEM Author Service设计作为创作和预览环境，允许可以针对它执行大多数功能开发验证，使其成为本地开发过程的重要元素。

1. 创建文件夹`~/aem-sdk/author`
1. 将&#x200B;__快速入门JAR__&#x200B;文件复制到`~/aem-sdk/author`并将其重命名为`aem-author-p4502.jar`
1. 从命令行执行以下命令，启动本地AEM Author服务：
   + `java -jar aem-author-p4502.jar`
      + 提供管理员密码作为`admin`。 可接受任何管理员密码，但建议对本地开发使用默认密码以减少重新配置的需要。

   您&#x200B;*无法*&#x200B;通过双击[&#128279;](#troubleshooting-double-click)启动AEM作为Cloud Service快速入门Jar 。
1. 在Web浏览器中，访问位于[http://localhost:4502](http://localhost:4502)的本地AEM创作服务

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## 设置本地AEM发布服务

本地AEM Publish服务为开发人员提供AEM的本地最终用户将会拥有的体验，例如浏览基于AEM的网站。 本地AEM Publish服务非常重要，因为它与AEM SDK的[Dispatcher工具](./dispatcher-tools.md)集成，允许开发人员对面向最终用户的最终体验进行冒烟测试和微调。

1. 创建文件夹`~/aem-sdk/publish`
1. 将&#x200B;__快速入门JAR__&#x200B;文件复制到`~/aem-sdk/publish`并将其重命名为`aem-publish-p4503.jar`
1. 通过从命令行执行以下操作来启动本地AEM Publish服务：
   + `java -jar aem-publish-p4503.jar`
      + 提供管理员密码作为`admin`。 可接受任何管理员密码，但建议对本地开发使用默认密码以减少重新配置的需要。

   您&#x200B;*无法*&#x200B;通过双击[&#128279;](#troubleshooting-double-click)启动AEM作为Cloud Service快速入门Jar 。
1. 在Web浏览器中，访问位于[http://localhost:4503](http://localhost:4503)的本地AEM发布服务

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## 在预发行模式中设置本地AEM服务

本地AEM运行时可在[预发行模式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=zh-Hans)下启动，允许开发人员针对AEM as a Cloud Service的下一发行版功能进行构建。 通过在本地AEM运行时的第一个启动中传递`-r prerelease`参数，启用了预发行版。 这可以同时用于本地AEM Author和AEM Publish服务。


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

>[!TAB Linux®]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## 模拟内容分发 {#content-distribution}

在真实的Cloud Service环境中，使用[Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html)和Adobe Pipeline将内容从Author Service分发到Publish Service。 [Adobe管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=zh-Hans#content-distribution)是仅在云环境中可用的独立微服务。

在开发期间，可能需要使用本地Author和Publish服务模拟内容的分发。 这可以通过启用旧版复制代理来实现。

>[!NOTE]
>
> 复制代理只能在本地Quickstart JAR中使用，并且只能提供内容分发的模拟。

1. 登录到&#x200B;**作者**&#x200B;服务并导航到[http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html)。
1. 单击&#x200B;**默认代理（发布）**&#x200B;以打开默认复制代理。
1. 单击&#x200B;**编辑**&#x200B;以打开代理的配置。
1. 在&#x200B;**设置**&#x200B;选项卡下，更新以下字段：

   + **已启用** — 检查true
   + **代理用户ID** — 将此字段留空

   ![复制代理配置 — 设置](assets/aem-runtime/settings-config.png)

1. 在&#x200B;**传输**&#x200B;选项卡下，更新以下字段：

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **用户** - `admin`
   + **密码** - `admin`

   ![复制代理配置 — 传输](assets/aem-runtime/transport-config.png)

1. 单击&#x200B;**确定**&#x200B;保存配置并启用&#x200B;**默认**&#x200B;复制代理。
1. 您现在可以更改Author服务上的内容并将其发布到Publish服务。

![发布页面](assets/aem-runtime/publish-page-changes.png)

## 快速入门Jar启动模式

快速入门Jar `aem-<tier>_<environment>-p<port number>.jar`的命名指定了它的启动方式。 AEM在特定层、创作层或发布层启动后，无法更改为备用层。 为此，必须删除首次运行时生成的`crx-Quickstart`文件夹，并且必须重新运行快速入门Jar。 可以更改环境和端口，但是它们需要停止/启动本地AEM实例。

更改环境`dev`、`stage`和`prod`对开发人员可能很有用，以确保AEM正确定义并解析特定于环境的配置。 建议主要针对默认的`dev`环境运行模式执行本地开发。

可用的排列如下：

| 快速入门Jar文件名 | 模式描述 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | 作为作者，在端口4502上处于开发运行模式 |
| `aem-author_dev-p4502.jar` | 作为作者，在端口4502上处于开发运行模式（与`aem-author-p4502.jar`相同） |
| `aem-author_stage-p4502.jar` | 作为作者，在端口4502上处于暂存运行模式 |
| `aem-author_prod-p4502.jar` | 作为作者，在端口4502上处于生产运行模式 |
| `aem-publish-p4503.jar` | 在端口4503上以开发运行模式作为发布 |
| `aem-publish_dev-p4503.jar` | 在端口4503上作为开发运行模式下的发布（与`aem-publish-p4503.jar`相同） |
| `aem-publish_stage-p4503.jar` | 在端口4503上以暂存运行模式作为发布 |
| `aem-publish_prod-p4503.jar` | 在端口4503上以生产运行模式作为发布 |

请注意，端口号可以是本地开发计算机上的任何可用端口，但按照惯例可以：

+ 端口&#x200B;__4502__&#x200B;用于&#x200B;__本地AEM创作服务__
+ 端口&#x200B;__4503__&#x200B;用于&#x200B;__本地AEM发布服务__

更改这些配置文件可能需要调整AEM SDK配置

## 停止本地AEM运行时

要停止本地AEM运行时(AEM创作或发布服务)，请打开用于启动AEM运行时的命令行窗口，然后点按`Ctrl-C`。 等待AEM关闭。 当关闭过程完成时，命令行提示符可用。

## 可选的本地AEM运行时设置任务

+ __OSGi配置环境变量和机密变量__&#x200B;是[专门为AEM本地运行时](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=zh-Hans#local-development)设置的，而不是使用aio CLI管理它们。

## 何时更新快速入门Jar

在每月最后一个星期四至少每月或之后不久更新AEM SDK，这是AEM as a Cloud Service“功能发布”的发布节奏。

>[!WARNING]
>
> 将快速入门Jar更新为新版本需要替换整个本地开发环境，从而导致本地AEM存储库中的所有代码、配置和内容丢失。 确保任何不应销毁的代码、配置或内容都将安全提交到Git，或从本地AEM实例导出为AEM包。

### 如何避免在升级AEM SDK时丢失内容

升级AEM SDK会有效地创建一个全新的AEM运行时，包括一个新存储库，这意味着对之前的AEM SDK存储库所做的任何更改都将丢失。 以下是一些在AEM SDK升级之间帮助保留内容的可行策略，可单独或一致使用：

1. 创建一个专门用于包含“示例”内容的内容包，以帮助进行开发，并在Git中对其进行维护。 任何应通过AEM SDK升级保留的内容将保留在此包中，并在升级AEM SDK后重新部署。
1. 将[oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)与`includepaths`指令一起使用，将内容从以前的AEM SDK存储库复制到新的AEM SDK存储库。
1. 在以前的AEM SDK中使用AEM包管理器和内容包备份任何内容，并在新的AEM SDK上重新安装它们。

请记住，在AEM SDK升级之间使用上述方法维护代码，表明存在开发反模式。 非一次性代码应源自开发IDE，并通过部署流入AEM SDK。

## 疑难解答

### 双击快速入门Jar文件会导致错误{#troubleshooting-double-click}

双击快速入门Jar以启动时，显示错误模式，阻止AEM本地启动。

![疑难解答 — 双击快速入门Jar文件](./assets/aem-runtime/troubleshooting__double-click.png)

这是因为AEM as a Cloud Service快速入门Jar不支持双击快速入门Jar以本地启动AEM。 相反，必须从该命令行运行Jar文件。

要启动AEM Author服务，`cd`进入包含Quickstart Jar的目录并执行命令：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

或者，要启动AEM Publish服务，`cd`进入包含Quickstart Jar的目录并执行命令：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-publish-p4503.jar
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

这是因为AEM as a Cloud Service需要Java™ SDK 11，而您运行的是其他版本，很可能是Java™ 8。 要解决此问题，请下载并安装[Oracle Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&fulltext=Oracle%7E+JDK%7E+11%7E&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=list&p.offset=0&p.limit=14&p.limit=144)。

安装Oracle Java™ 11 SDK后，通过从命令行运行命令来验证它是活动版本：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

## 其他资源

+ [下载AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [Experience Manager Dispatcher文档](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hans)
