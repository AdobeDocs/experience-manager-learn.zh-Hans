---
title: 将本地AEM Runtime for AEM设置为Cloud Service开发
description: 使用AEM作为Cloud ServiceSDK的快速启动Jar设置本地AEM运行时。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 1%

---


# 设置本地AEM运行时

Adobe Experience Manager(AEM)可以使用AEM作为Cloud ServiceSDK的快速启动程序Jar在本地运行。 这允许开发人员在将自定义代码、配置和内容提交到源代码控件并将其部署到AEM作为Cloud Service环境之前，先进行部署和测试。

请注 `~` 意，它用作用户目录的简写。 在Windows中，这相当于 `%HOMEPATH%`。

>[!VIDEO](https://video.tv.adobe.com/v/32551/?quality=12&learn=on)

>[!NOTE]
>
> 此视频演示如何使用AEM SDK的本地快速启动在几分钟内安装和运行Adobe Experience Manager的本地实例。 此视频通过多次单击快速启动Jar文件来显示启动AEM SDK的本地快速启动的过程，但在计算机上安装了Java 8时，此操作将不起作用。 或者，也可以使用本页所述的命令从命令行启 `java -jar ...` 动AEM [SDK的本地快速启动](#set-up-local-aem-author-service)。

## 安装Java

Experience Manager是一个Java应用程序，因此需要Java SDK支持开发工具。

1. [下载并安装最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+11%7E&amp;orderby=%40jcr%3content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=列表&amp;p.offset=0&amp;p.limit=14)
1. 通过运行以下命令验证Java 11 SDK是否已安装：
   + Windows:`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## 将AEM作为Cloud ServiceSDK下载

AEM作为Cloud ServiceSDK或AEM SDK，包含用于在本地运行AEM作者和发布以进行开发的快速启动Jar以及兼容版本的调度程序工具。

1. 登录https://experience.adobe.com/#/downloads [与您](https://experience.adobe.com/#/downloads) 的Adobe ID
   + 请注意，您的Adobe __组织__ 必须提供AEM作为Cloud Service才能下载AEM作为Cloud ServiceSDK。
1. 导航到AEM __作为Cloud Service选项卡__
1. 按发布 __日期__ ，降 __序排__ 序
1. 单击最新的 __AEM__ SDK结果行
1. 查看并接受EULA，然后点击“下 __载__ ”按钮

## 从AEM SDK zip解压快速启动程序Jar

1. 解压缩下载的文 `aem-sdk-XXX.zip` 件
1. 确保您的Experience Manager开 __发人员license.properties__ 文件可用

请注意，同一Quickstart Jar和license.properties文件用于开始 _AEM_ Author和Publish Services。

## 设置本地AEM作者服务{#set-up-local-aem-author-service}

本地AEM作者服务为开发人员提供本地体验，数字营销人员／内容作者将共享该体验以创建和管理内容。  AEM作者服务既设计为创作环境，又设计为预览，允许对其执行大多数功能开发验证，从而使其成为本地开发流程的关键元素。

1. 创建文件夹 `~/aem-sdk/author`
1. 将快速 __启动JAR文__ 件复制 `~/aem-sdk/author` 并重命名为 `aem-author-p4502.jar`
1. 将license. __properties文件复制__ 到  `~/aem-sdk/author`
1. 开始本地AEM作者服务，方法是从命令行执行以下操作：
   + `java -jar aem-author-p4502.jar`
      + 请提供管理员密码 `admin`。 可以接受任何管理员密码，但建议使用本地开发的默认密码，以减少重新配置的需要。

   您不 *能通过* 多次单击将AEM [开始为Cloud Service快速启动Jar](#troubleshooting-double-click)。
1. 在Web浏览器中通过http://localhost:4502访 [问本](http://localhost:4502) 地AEM作者服务

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\author
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cp ../license.properties ~/aem-sdk/author
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 设置本地AEM发布服务

本地AEM发布服务为开发人员提供AEM的最终用户将具有的本地体验，如浏览AEM上的网站。 本地AEM发布服务与AEM SDK的Dispatcher工具集 [成](./dispatcher-tools.md) ，使开发人员能对最终面向最终用户的体验进行烟雾测试和微调，因此非常重要。

1. 创建文件夹 `~/aem-sdk/publish`
1. 将快速 __启动JAR文__ 件复制 `~/aem-sdk/publish` 并重命名为 `aem-publish-p4503.jar`
1. 将license. __properties文件复制__ 到  `~/aem-sdk/publish`
1. 开始本地AEM发布服务，方法是从命令行执行以下操作：
   + `java -jar aem-publish-p4503.jar`
      + 请提供管理员密码 `admin`。 可以接受任何管理员密码，但建议使用本地开发的默认密码，以减少重新配置的需要。

   您不 *能通过* 多次单击将AEM [开始为Cloud Service快速启动Jar](#troubleshooting-double-click)。
1. 在Web浏览器中通过http://localhost:4503访 [问本](http://localhost:4503) 地AEM发布服务

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\publish
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cp ../license.properties ~/aem-sdk/publish
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 快速启动Jar开始模式

快速启动程序Jar的命名 `aem-<tier>_<environment>-p<port number>.jar` 指定其开始方式。 AEM在特定层、作者或发布中启动后，便不能将其更改为替代层。 为此，必须删 `crx-Quickstart` 除在首次运行期间生成的文件夹，并且必须再次运行快速启动Jar。 环境和端口可以更改，但需要停止/开始本地AEM实例。

更改环境 `dev`和 `stage` 对 `prod`开发人员很有用，可确保AEM正确定义和解决环境特定配置。 建议主要针对默认环境运行模式进行本 `dev` 地开发。

可用的排列如下：

+ `aem-author-p4502.jar`
   + 作为作者在4502端口上的开发运行模式下
+ `aem-author_dev-p4502.jar`
   + 作为作者在4502端口上的开发运行模式(与相同 `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + 在4502端口上以临时运行模式作为作者
+ `aem-author_prod-p4502.jar`
   + 作为作者在4502端口上的“生产”运行模式
+ `aem-publish-p4503.jar`
   + 作为作者在4503端口的开发运行模式下
+ `aem-publish_dev-p4503.jar`
   + 作为作者在4503端口上的开发运行模式(与相同 `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + 在4503端口上以临时运行模式作为作者
+ `aem-publish_prod-p4503.jar`
   + 作为作者在4503端口上的“生产”运行模式

请注意，端口号可以是本地开发机器上的任何可用端口，但是按惯例：

+ 端口 __4502__ 用于本 __地AEM作者服务__
+ 端 __口4503__ 用于本地 __AEM发布服务__

更改这些设置可能需要调整AEM SDK配置

## 停止本地AEM运行时

要停止本地AEM运行时（AEM作者服务或发布服务），请打开用于开始AEM运行时的命令行窗口，然后点按 `Ctrl-C`。 等待AEM关闭。 当关闭过程完成时，命令行提示将可用。

## 何时更新快速启动程序Jar

每月的最后一个星期四(即AEM的发行日期)至少每月更新AEM SDK，作为Cloud Service“功能发布”。

>[!WARNING]
>
> 将快速启动程序Jar更新到新版本需要替换整个本地开发环境，从而导致本地AEM存储库中的所有代码、配置和内容丢失。 确保任何不应销毁的代码、配置或内容都安全提交到Git，或从本地AEM实例作为AEM包导出。

### 如何避免升级AEM SDK时丢失内容

升级AEM SDK将有效地创建一个全新的AEM运行时，包括一个新的存储库，这意味着对先前的AEM SDK存储库所做的任何更改都将丢失。 以下是帮助AEM SDK升级之间保持内容的可行策略，可单独使用或协同使用：

1. 创建专门用于包含“示例”内容以帮助开发的内容包，并在Git中对其进行维护。 应通过AEM SDK升级保留的任何内容将保留到此包中，并在升级AEM SDK后重新部署。
1. 将 [oak-upgrade与](https://jackrabbit.apache.org/oak/docs/migration.html)`includepaths` 指令结合使用，将内容从先前的AEM SDK存储库复制到新的AEM SDK存储库。
1. 使用AEM Package Manager备份任何内容，并在以前的AEM SDK上重新安装内容，然后在新的AEM SDK上重新安装。

请记住，使用上述方法在AEM SDK升级之间维护代码表明开发反模式。 非一次性代码应源自您的开发IDE，并通过部署流入AEM SDK。

## 疑难解答

## 多次单击快速启动Jar文件会导致错误{#troubleshooting-double-click}

当多次单击“快速启动Jar”开始时，会显示一个错误模式，阻止AEM在本地启动。

![疑难解答-多次单击快速启动Jar文件](./assets/aem-runtime/troubleshooting__double-click.png)

这是因为AEM作为Cloud Service快速启动Jar不支持将快速启动Jar多次单击到本地开始AEM。 而是必须从该命令行运行Jar文件。

要开始AEM作者服务，请 `cd` 进入包含快速启动Jar的目录，然后执行以下命令：

`$ java -jar aem-author-p4502.jar`

或者，要开始AEM发布服务，请 `cd` 进入包含快速启动Jar的目录并执行以下命令：

`$ java -jar aem-author-p4503.jar`

## 从命令行启动快速启动程序Jar会立即中止{#troubleshooting-java-8}

从命令行启动快速启动Jar时，进程会立即中止，AEM服务不会开始，并出现以下错误：

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

这是因为AEM作为Cloud Service需要Java SDK 11，而您运行的版本很可能是Java 8。 要解决此问题，请下载 [并安装Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+11%7E&amp;orderby=%40jcr%3content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=列表&amp;p.offset=0&amp;p.limit=14)。
安装Java SDK 11后，通过从命令行执行以下命令来验证它是否为活动版本。

安装Java 11 SDK后，通过从命令行运行命令来验证它是活动版本：

+ Windows: `java -version`
+ macOS/Linux: `java --version`

## 其他资源

+ [下载AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe云管理器](https://my.cloudmanager.adobe.com/)
+ [下载Docker](https://www.docker.com/)
+ [Experience Manager调度程序文档](https://docs.adobe.com/content/help/zh-Hans/experience-manager-dispatcher/using/dispatcher.html)
