---
title: AEM Sites入门 — 项目设置
seo-title: AEM Sites入门 — 项目设置
description: 包括创建Maven多模块项目以管理AEM站点的代码和配置。
sub-product: 站点
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: AEM 项目原型
topic: 内容管理，开发
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1888'
ht-degree: 4%

---


# 项目设置{#project-setup}

本教程介绍如何创建一个Maven多模块项目来管理Adobe Experience Manager站点的代码和配置。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](../overview.md#local-dev-environment)所需的工具和说明。 确保您在本地提供了Adobe Experience Manager的新实例，并且未安装任何其他示例/演示包（所需服务包除外）。

## 目标 {#objective}

1. 了解如何使用Maven原型生成新的AEM项目。
1. 了解AEM Project Archetype生成的不同模块以及它们如何协同工作。
1. 了解AEM核心组件如何包含在AEM项目中。

## 将构建{#what-build}的内容

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

在本章中，您将使用[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)生成一个新的Adobe Experience Manager项目。 您的AEM项目包含用于站点实施的所有代码、内容和配置。 本章中生成的项目将作为执行世界开发组织网站的基础，并将在今后各章中加以建设。

**什么是Maven项目？** -  [Apache ](https://maven.apache.org/) Mavenis是用于构建项目的软件管理工具。*所有Adobe Experience Manager实* 施都使用Maven项目在AEM的基础上构建、管理和部署自定义代码。

**什么是马文原型？** - Maven原 [型](https://maven.apache.org/archetype/index.html) 是用于生成新项目的模板或模式。AEM Project原型允许我们使用自定义命名空间生成新项目，并包含遵循最佳实践的项目结构，从而大大加快了我们的项目。

## 创建项目{#create}

为AEM创建Maven多模块项目有几个选项。 本教程将利用[Maven AEM Project Archetype **26**](https://github.com/adobe/aem-project-archetype)。 Cloud Manager还[提供UI向导](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html)以启动AEM应用程序项目的创建。 由Cloud Manager UI生成的基础项目与直接使用原型的结构相同。

>[!NOTE]
>
>本教程使用原型的&#x200B;**26**&#x200B;版本。 使用&#x200B;**最新**&#x200B;版本的原型生成新项目始终是最佳做法。

接下来的一系列步骤将使用基于UNIX的命令行终端进行，但在使用Windows终端时应类似。

1. 打开命令行终端。 验证Maven是否已安装：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 通过运行以下命令验证&#x200B;**adobe-public**&#x200B;用户档案是否处于活动状态：

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   如果您执行&#x200B;****&#x200B;操作，请参阅&#x200B;**adobe-public**，则表明您的`~/.m2/settings.xml`文件中未正确引用Adobe回购。 请重新访问在[本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven)中安装和配置Apache Maven的步骤。

1. 导览至要在其中生成AEM项目的目录。 这可以是要在其中维护项目源代码的任何目录。 例如，在用户主目录下方的名为`code`的目录：

   ```shell
   $ cd ~/code
   ```

1. 将以下内容粘贴到命令行中，以[以批处理模式](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)生成项目：

   ```shell
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=26 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 如果以AEM 6.5.5+为目标，则将`aemVersion="cloud"`替换为`aemVersion="6.5.5"`。 如果目标为6.4.8+，请使用`aemVersion="6.4.8"`。

   可在此](https://github.com/adobe/aem-project-archetype#available-properties)找到用于配置项目[的可用属性的完整列表。

1. 以下文件夹和文件结构将由本地文件系统上的Maven原型生成：

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- analyse/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 部署和构建项目{#build}

构建项目代码并将其部署到AEM的本地实例。

1. 请确保在端口&#x200B;**4502**&#x200B;上本地运行AEM的作者实例。
1. 从命令行导航到`aem-guides-wknd`项目目录。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 运行以下命令以构建整个项目并将其部署到AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   构建将需要约一分钟时间，并应以以下消息结束：

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Maven用户档案`autoInstallSinglePackage`编译项目的各个模块，并将单个包部署到AEM实例。 默认情况下，此包将部署到在端口&#x200B;**4502**&#x200B;上本地运行的AEM实例，凭据为`admin:admin`。

1. 导航到本地AEM实例上的包管理器：[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应当看到`aem-guides-wknd.ui.apps`、`aem-guides-wknd.ui.config`、`aem-guides-wknd.ui.content`和`aem-guides-wknd.all`的包。

1. 导航到站点控制台：[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。 WKND站点将是其中一个站点。 它将包括一个具有美国和语言硕士层次结构的站点结构。 当使用原型生成项目时，此站点层次结构基于`language_country`和`isSingleCountryWebsite`的值。

1. 选择&#x200B;**US** `>` **English**&#x200B;页面，然后单击菜单栏中的&#x200B;**编辑**&#x200B;按钮，打开该页面：

   ![站点控制台](assets/project-setup/aem-sites-console.png)

1. 已创建起动内容，可向页面添加多个组件。 尝试这些组件，了解其功能。 您将在下一章中学习组件的基础知识。

   ![家庭入门内容](assets/project-setup/start-home-page.png)

   *由原型生成的示例内容*

## Inspect项目{#project-structure}

生成的AEM项目由各个Maven模块组成，每个模块具有不同的角色。 本教程和大部分开发侧重于以下模块：

* [核心](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - Java Code，主要是后端开发人员。
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)  — 包含CSS、JavaScript、Sass、Type Script的源代码，主要面向前端开发人员。
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)  — 包含组件和对话框定义，将编译的CSS和JavaScript嵌入为客户端库。
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 包含结构内容和配置，如可编辑的模板、元数据模式(/content、/conf)。

* **all**  — 这是一个空的Maven模块，它将上述模块组合到可部署到AEM环境的单个包中。

![Maven项目图](assets/project-setup/project-pom-structure.png)

有关&#x200B;**所有** Maven模块的更多详细信息，请参阅[AEM Project Archetype文档](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)。

### 包含核心组件{#core-components}

[AEM核心](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html) 组件是一套针对AEM的标准化Web内容管理(WCM)组件。这些组件提供了一组功能的基准，设计为针对个别项目设置样式、自定义和扩展。

AEM作为Cloud Service环境包括最新版的[AEM核心组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)。 因此，为AEM生成的作为Cloud Service的项目&#x200B;**不**&#x200B;包含嵌入的AEM核心组件。

对于AEM 6.5/6.4生成的项目，原型会自动在项目中嵌入[AEM核心组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)。 嵌入AEM核心组件是AEM 6.5/6.4的最佳实践，可确保将最新版本部署到您的项目。 有关如何在项目中包含[核心组件的详细信息，请参阅](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/using.html#core-components)。

## 源控制管理{#source-control}

最好使用某种形式的源代码控件来管理应用程序中的代码。 本教程使用git和GitHub。 Maven和/或所选IDE生成的多个文件应被SCM忽略。

Maven在您构建和安装代码包时将创建一个目标文件夹。 目标文件夹和内容应从SCM中排除。

在`ui.apps`下方，可以看到已创建许多`.content.xml`文件。 这些XML文件映射JCR中安装的内容的节点类型和属性。 这些文件至关重要，应&#x200B;**不应**&#x200B;被忽略。

AEM项目原型将生成一个示例`.gitignore`文件，该文件可用作可安全忽略文件的起点。 文件在`<src>/aem-guides-wknd/.gitignore`处生成。

## 恭喜！{#congratulations}

祝贺您，您刚刚创建了您的第一个AEM项目！

### 后续步骤{#next-steps}

通过[组件基础知识](component-basics.md)教程的简单`HelloWorld`示例，了解Adobe Experience Manager(AEM)站点组件的底层技术。

## 高级Maven命令（红色）{#advanced-maven-commands}

在开发过程中，您可能只使用其中一个模块，并希望避免构建整个项目以节省时间。 您还可能希望直接部署到AEM发布实例，或者可能部署到未在端口4502上运行的AEM实例。

接下来，我们将看一些Maven用户档案和命令，您可以在开发过程中使用它们来获得更大的灵活性。

### 核心模块{#core-module}

**[核心](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)**&#x200B;模块包含与项目关联的所有Java代码。 构建后，它将OSGi捆绑包部署到AEM。 仅构建此模块：

1. 导览至`core`文件夹（`aem-guides-wknd`下方）：

   ```shell
   $ cd core/
   ```

1. 运行以下命令：

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. 导航到[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)。 这是OSGi Web控制台，包含有关安装在AEM实例上的所有捆绑包的信息。

1. 切换&#x200B;**Id**&#x200B;排序列，您将看到WKND包已安装并处于活动状态。

   ![核心捆绑](assets/project-setup/wknd-osgi-console.png)

1. 您可以在[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar)中看到jar的“physical”位置：

   ![CRXDE Location of Jar](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模块{#apps-content-module}

**[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)**&#x200B;主模块包含`/apps`下站点所需的所有呈现代码。 这包括将以AEM格式存储的CSS/JS，该格式名为[clientlibs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/clientlibs.html)。 这还包括用于呈现动态HTML的[ HTL](https://docs.adobe.com/content/help/zh-Hans/experience-manager-htl/using/overview.html)脚本。 您可以将&#x200B;**ui.apps**&#x200B;模块视为JCR中结构的映射，但格式可以存储在文件系统中并提交到源代码控制。 **ui.apps**&#x200B;模块只包含代码。

要构建仅此模块：

1. 从命令行。 导览至`ui.apps`文件夹（`aem-guides-wknd`下方）：

   ```shell
   $ cd ../ui.apps
   ```

1. 运行以下命令：

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 导航到[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应将`ui.apps`包看作第一个安装的包，它应具有比其他任何包更新的时间戳。

   ![已安装UI.apps包](assets/project-setup/ui-apps-package.png)

1. 返回命令行并运行以下命令（在`ui.apps`文件夹中）：

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   用户档案`autoInstallPackagePublish`旨在将包部署到端口&#x200B;**4503**&#x200B;上运行的发布环境。 如果找不到在http://localhost:4503上运行的AEM实例，则会出现上述错误。

1. 最后，运行以下命令以在端口&#x200B;**4504**&#x200B;上部署`ui.apps`包：

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   同样，如果端口&#x200B;**4504**&#x200B;上没有运行的AEM实例可用，则预计会发生生成失败。 参数`aem.port`在POM文件`aem-guides-wknd/pom.xml`中定义。

**[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)**&#x200B;模块的结构与&#x200B;**ui.apps**&#x200B;模块相同。 唯一的区别是，**ui.content**&#x200B;模块包含称为&#x200B;**mutable**&#x200B;内容的内容。 **多** 表内容主要指非代码配置，如存储在源代码管理中但可以直接在AEM实例上修 **** 改的模板、策略或文件夹结构。将在页面和模板的章节中详细探讨此功能。

用于构建&#x200B;**ui.apps**&#x200B;模块的相同Maven命令可用于构建&#x200B;**ui.content**&#x200B;模块。 您可以从&#x200B;**ui.content**&#x200B;文件夹中重复上述步骤。
