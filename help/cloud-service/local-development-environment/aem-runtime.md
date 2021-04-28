---
title: 将本地AEM Runtime for AEM设置为Cloud Service开发
description: 使用AEM作为Cloud Service SDK的快速启动Jar设置本地AEM运行时。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
topic: 开发
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '1734'
ht-degree: 2%

---


# 设置本地AEM Runtime

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本地AEM Runtime"
>abstract="Adobe Experience Manager(AEM)可以在本地运行，使用AEM作为Cloud Service SDK的快速启动程序Jar。 这允许开发人员在将自定义代码、配置和内容提交到源代码控制并将其部署到AEM作为Cloud Service环境之前，先对其进行部署和测试。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="将AEM下载为Cloud Service SDK"

Adobe Experience Manager(AEM)可以在本地运行，使用AEM作为Cloud Service SDK的快速启动程序Jar。 这允许开发人员在将自定义代码、配置和内容提交到源代码控制并将其部署到AEM作为Cloud Service环境之前，先对其进行部署和测试。

请注意，`~`用作用户目录的简写。 在Windows中，这等效于`%HOMEPATH%`。

## 安装Java

Experience Manager是一个Java应用程序，因此需要Java SDK支持开发工具。

1. [下载并安装最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3E&amp;orderby%3content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=列表&amp;p.offset=0&amp;p.limit=14)
1. 通过运行以下命令验证Java 11 SDK是否已安装：
   + Windows:`java -version`
   + macOS/Linux:`java --version`

![Java](./assets/aem-runtime/java.png)

## 下载AEM作为Cloud Service SDK

AEM作为Cloud Service SDK或AEM SDK，包含用于在本地运行AEM作者和发布以进行开发的快速启动程序Jar以及兼容版本的调度程序工具。

1. 使用您的Adobe ID登录到[https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads)
   + 请注意，您的Adobe组织&#x200B;__必须__&#x200B;为AEM提供Cloud Service才能将AEM作为Cloud ServiceSDK下载。
1. 导览至&#x200B;__AEM a Cloud Service__&#x200B;选项卡
1. 按&#x200B;__降序__&#x200B;顺序中的&#x200B;__发布日期__&#x200B;排序
1. 单击最新的&#x200B;__AEM SDK__&#x200B;结果行
1. 查看并接受EULA，然后点按&#x200B;__下载__&#x200B;按钮

## 从AEM SDK zip解压快速启动程序Jar

1. 解压缩下载的`aem-sdk-XXX.zip`文件

## 设置本地AEM作者服务{#set-up-local-aem-author-service}

本地AEM作者服务为开发人员提供本地体验，数字营销人员/内容作者将共享该体验以创建和管理内容。  AEM创作服务既设计为创作环境，又设计为预览，允许对其执行功能开发的大多数验证，这使它成为本地开发过程中的一个关键元素。

1. 创建文件夹`~/aem-sdk/author`
1. 将&#x200B;__快速启动JAR__&#x200B;文件复制到`~/aem-sdk/author`，并将其重命名为`aem-author-p4502.jar`
1. 开始本地AEM作者服务，方法是从命令行执行以下操作：
   + `java -jar aem-author-p4502.jar`
      + 将管理员密码提供为`admin`。 任何管理员密码都是可以接受的，但是，它建议使用本地开发的默认密码来减少重新配置的需要。

   *不能*&#x200B;通过多次单击](#troubleshooting-double-click)将AEM开始为Cloud Service快速启动Jar [。
1. 在Web浏览器中访问位于[http://localhost:4502](http://localhost:4502)的本地AEM作者服务

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 设置本地AEM发布服务

本地AEM发布服务为开发人员提供AEM最终用户将拥有的本地体验，如浏览AEM上的网站。 本地AEM发布服务与AEM SDK的[调度程序工具](./dispatcher-tools.md)集成，使开发人员能对最终面向最终用户的体验进行烟雾测试和微调，这一点非常重要。

1. 创建文件夹`~/aem-sdk/publish`
1. 将&#x200B;__快速启动JAR__&#x200B;文件复制到`~/aem-sdk/publish`，并将其重命名为`aem-publish-p4503.jar`
1. 开始本地AEM发布服务，方法是从命令行执行以下操作：
   + `java -jar aem-publish-p4503.jar`
      + 将管理员密码提供为`admin`。 任何管理员密码都是可以接受的，但是，它建议使用本地开发的默认密码来减少重新配置的需要。

   *不能*&#x200B;通过多次单击](#troubleshooting-double-click)将AEM开始为Cloud Service快速启动Jar [。
1. 在Web浏览器中访问位于[http://localhost:4503](http://localhost:4503)的本地AEM发布服务

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 模拟内容分发{#content-distribution}

在真正的Cloud Service环境中，使用[Sling内容分发](https://sling.apache.org/documentation/bundles/content-distribution.html)和Adobe管道将内容从创作服务分发到发布服务。 [Adobe管线](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution)是仅在云环境中可用的隔离微服务。

在开发过程中，可能希望使用本地作者和发布服务模拟内容的分发。 这可以通过启用旧版复制代理来实现。

>[!NOTE]
>
> 复制代理仅可在本地快速启动JAR中使用，并且仅提供内容分发模拟。

1. 登录&#x200B;**Author**&#x200B;服务并导航到[http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html)。
1. 单击&#x200B;**默认代理（发布）**&#x200B;以打开默认复制代理。
1. 单击&#x200B;**编辑**&#x200B;以打开代理的配置。
1. 在&#x200B;**设置**&#x200B;选项卡下，更新以下字段：

   + **已启用**  — 选中true
   + **代理用户Id**  — 将此字段留空

   ![复制代理配置 — 设置](assets/aem-runtime/settings-config.png)

1. 在&#x200B;**传输**&#x200B;选项卡下，更新以下字段：

   + **URI** -  `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **用户** -  `admin`
   + **密码** -  `admin`

   ![复制代理配置 — 传输](assets/aem-runtime/transport-config.png)

1. 单击&#x200B;**确定**&#x200B;以保存配置并启用&#x200B;**默认**&#x200B;复制代理。
1. 您现在可以更改创作服务上的内容，并将其发布到发布服务。

![发布页面](assets/aem-runtime/publish-page-changes.png)

## 快速启动Jar开始模式

快速启动程序Jar的命名，`aem-<tier>_<environment>-p<port number>.jar`指定它将如何开始。 AEM在特定层、作者或发布中启动后，便不能将其更改为替代层。 为此，必须删除在首次运行期间生成的`crx-Quickstart`文件夹，并且必须再次运行快速启动Jar。 环境和端口可以更改，但需要停止/开始本地AEM实例。

更改环境`dev`、`stage`和`prod`对于开发人员来说非常有用，可以确保AEM正确定义和解决特定于环境的配置。 建议主要针对默认的`dev`环境运行模式进行本地开发。

可用的组合如下：

+ `aem-author-p4502.jar`
   + 作为作者在4502端口的开发运行模式下
+ `aem-author_dev-p4502.jar`
   + 作为Dev运行模式的作者，在端口4502上运行（与`aem-author-p4502.jar`相同）
+ `aem-author_stage-p4502.jar`
   + 在4502端口的暂存运行模式下作为作者
+ `aem-author_prod-p4502.jar`
   + 作为作者在端口4502上的生产运行模式下
+ `aem-publish-p4503.jar`
   + 作为作者在4503端口的开发运行模式下
+ `aem-publish_dev-p4503.jar`
   + 作为Dev运行模式的作者，在端口4503上运行（与`aem-publish-p4503.jar`相同）
+ `aem-publish_stage-p4503.jar`
   + 在4503端口的暂存运行模式下作为作者
+ `aem-publish_prod-p4503.jar`
   + 作为作者在端口4503上的生产运行模式下

请注意，端口号可以是本地开发机器上的任何可用端口，但是按惯例：

+ 端口&#x200B;__4502__&#x200B;用于&#x200B;__本地AEM作者服务__
+ 端口&#x200B;__4503__&#x200B;用于&#x200B;__本地AEM发布服务__

更改这些设置可能需要调整AEM SDK配置

## 停止本地AEM运行时

要停止本地AEM运行时（AEM作者服务或发布服务），请打开用于开始AEM运行时的命令行窗口，然后点按`Ctrl-C`。 等待AEM关闭。 当关闭过程完成时，命令行提示将可用。

## 可选的本地AEM运行时设置任务

+ __OSGi配置环境变量和机__ 密变 [量是专门为AEM本地运行时设置的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)，而不是使用aio CLI管理它们。

## 何时更新快速启动程序Jar

在每月的最后一个星期四(即作为Cloud Service“功能版本”的AEM发行章程)上至少每月或之后不久更新AEM SDK。

>[!WARNING]
>
> 将快速启动程序Jar更新到新版本需要替换整个本地开发环境，从而导致丢失本地AEM存储库中的所有代码、配置和内容。 确保不应销毁的任何代码、配置或内容安全提交到Git，或从本地AEM实例导出为AEM包。

### 如何在升级AEM SDK时避免内容丢失

升级AEM SDK实际上是在创建全新的AEM运行时，包括新的存储库，这意味着对先前AEM SDK的存储库所做的任何更改都将丢失。 以下是帮助在AEM SDK升级之间保持内容的可行策略，可以单独使用或一致使用：

1. 创建专门用于包含“示例”内容以帮助开发的内容包，并在Git中进行维护。 应通过AEM SDK升级保留的任何内容将保留到此包中，并在升级AEM SDK后重新部署。
1. 将[oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)与`includepaths`指令结合使用，将先前AEM SDK存储库中的内容复制到新的AEM SDK存储库。
1. 使用AEM Package Manager备份任何内容以及先前AEM SDK上的内容包，并在新的AEM SDK上重新安装它们。

请记住，使用上述方法在AEM SDK升级之间维护代码表示开发反模式。 非一次性代码应源自您的开发IDE，并通过部署流入AEM SDK。

## 疑难解答

## 多次单击快速启动Jar文件会导致错误{#troubleshooting-double-click}

当多次单击要开始的快速启动程序Jar时，将显示一个错误模式，阻止AEM在本地启动。

![疑难解答 — 按住多次单击快速启动Jar文件](./assets/aem-runtime/troubleshooting__double-click.png)

这是因为AEM作为Cloud Service快速启动程序Jar不支持将快速启动程序Jar多次单击到本地开始AEM。 相反，您必须从该命令行运行Jar文件。

要开始AEM作者服务，请`cd`进入包含快速启动程序Jar的目录，然后执行以下命令：

`$ java -jar aem-author-p4502.jar`

或者，要开始AEM发布服务，请将`cd`添加到包含快速启动Jar的目录中，然后执行以下命令：

`$ java -jar aem-author-p4503.jar`

## 从命令行启动快速启动程序Jar会立即中止{#troubleshooting-java-8}

从命令行启动快速启动Jar时，进程会立即中止，AEM服务不会开始，出现以下错误：

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

这是因为AEM作为Cloud Service需要Java SDK 11，而您运行的是不同版本，很可能是Java 8。 要解决此问题，请下载并安装[Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3E&amp;orderby%3content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=列表&amp;p.offset=0&amp;p.limit=14)。
安装Java SDK 11后，从命令行执行以下命令，验证它是否为活动版本。

安装Java 11 SDK后，通过从命令行运行命令来验证它是活动版本：

+ Windows: `java -version`
+ macOS/Linux:`java --version`

## 其他资源

+ [下载AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [Experience Manager Dispatcher文档](https://docs.adobe.com/content/help/zh-Hans/experience-manager-dispatcher/using/dispatcher.html)
