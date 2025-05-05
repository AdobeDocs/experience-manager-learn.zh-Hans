---
title: AEM Sites — 项目设置入门
description: 创建一个Maven Multi Module项目以管理Experience Manager站点的代码和配置。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-3418
thumbnail: 30152.jpg
doc-type: Tutorial
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
duration: 502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1684'
ht-degree: 1%

---

# 项目设置 {#project-setup}

本教程介绍如何创建Maven Multi Module项目来管理Adobe Experience Manager站点的代码和配置。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](./overview.md#local-dev-environment)所需的工具和说明。 确保您有新的Adobe Experience Manager实例可在本地使用，并且未安装其他示例/演示包（所需的Service Pack除外）。

## 目标 {#objective}

1. 了解如何使用Maven原型生成新的AEM项目。
1. 了解AEM项目原型生成的各个模块以及它们如何协同工作。
1. 了解AEM核心组件如何包含在AEM项目中。

## 您即将构建的内容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

在本章中，您使用[Adobe Experience Manager项目原型](https://github.com/adobe/aem-project-archetype)生成新的AEM项目。 您的AEM项目包含用于Sites实施的完整代码、内容和配置。 本章中生成的项目将作为WKND站点实施的基础，并在以后的章节中构建该项目。

**什么是Maven项目？** - [Apache Maven](https://maven.apache.org/)是用于构建项目的软件管理工具。 *所有Adobe Experience Manager*&#x200B;实施都使用Maven项目在AEM之上生成、管理和部署自定义代码。

**什么是Maven原型？** - [Maven原型](https://maven.apache.org/archetype/index.html)是用于生成新项目的模板或模式。 AEM项目原型有助于生成具有自定义命名空间的新项目，并包括遵循最佳实践的项目结构，从而大大加快了项目开发。

## 创建项目 {#create}

提供了几个选项来为AEM创建Maven多模块项目。 本教程使用[Maven AEM项目原型&#x200B;**35**](https://github.com/adobe/aem-project-archetype)。 Cloud Manager还[提供了一个UI向导](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html)以启动AEM应用程序项目的创建。 Cloud Manager UI生成的基础项目具有与直接使用原型相同的结构。

>[!NOTE]
>
>本教程使用原型的版本&#x200B;**35**。 使用原型的&#x200B;**最新**&#x200B;版本生成新项目始终是最佳实践。

下一系列步骤将使用基于UNIX®的命令行终端来执行，但如果使用Windows终端，这些步骤应该相似。

1. 打开命令行终端。 验证是否已安装Maven：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 导航到要生成AEM项目的目录。 该目录可以是任何要维护项目源代码的目录。 例如，用户主目录下名为`code`的目录：

   ```shell
   $ cd ~/code
   ```

1. 将以下内容粘贴到命令行以便[在批处理模式下生成项目](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)：

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 要定位AEM 6.5.14+，请将`aemVersion="cloud"`替换为`aemVersion="6.5.14"`。
   >
   > 此外，始终通过引用[AEM项目原型>用法](https://github.com/adobe/aem-project-archetype#usage)来使用最新的`archetypeVersion`

   可以在此处[&#128279;](https://github.com/adobe/aem-project-archetype#available-properties)找到用于配置项目的可用属性的完整列表。

1. 以下文件夹和文件结构由本地文件系统上的Maven原型生成：

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
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 部署和生成项目 {#build}

生成项目代码并将其部署到AEM的本地实例。

1. 请确保在端口&#x200B;**4502**&#x200B;上本地运行了一个AEM的创作实例。
1. 从命令行导航到`aem-guides-wknd`项目目录。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 运行以下命令以生成整个项目并将其部署到AEM：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   构建大约需要一分钟，并且应该以以下消息结束：

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   Maven配置文件`autoInstallSinglePackage`编译项目的各个模块，并将单个包部署到AEM实例。 默认情况下，此包将部署到在端口&#x200B;**4502**&#x200B;上本地运行的、凭据为`admin:admin`的AEM实例。

1. 导航到本地AEM实例上的包管理器： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应该看到`aem-guides-wknd.ui.apps`、`aem-guides-wknd.ui.config`、`aem-guides-wknd.ui.content`和`aem-guides-wknd.all`的包。

1. 导航到站点控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。 WKND站点是站点之一。 它包括一个站点结构，带有美国和语言母版层次结构。 此网站层次结构基于使用原型生成项目时`language_country`和`isSingleCountryWebsite`的值。

1. 通过选择页面，然后单击菜单栏中的&#x200B;**编辑**&#x200B;按钮，打开&#x200B;**美国** `>` **英语**&#x200B;页面：

   ![站点控制台](assets/project-setup/aem-sites-console.png)

1. 已创建入门内容，并且有多个组件可供添加到页面中。 尝试这些组件以了解其功能。 您将在下一章中学习组件的基础知识。

   ![家庭入门内容](assets/project-setup/start-home-page.png)

   *原型生成的示例内容*

## 检查项目 {#project-structure}

生成的AEM项目由单独的Maven模块组成，每个模块具有不同的角色。 本教程和大多数开发都侧重于以下模块：

* [core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java代码，主要是后端开发人员。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) — 包含CSS、JavaScript、Sass、TypeScript的源代码，主要适用于前端开发人员。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html) — 包含组件和对话框定义，将编译的CSS和JavaScript嵌入为客户端库。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html) — 包含结构化内容和配置，如可编辑模板、元数据架构(/content、/conf)。

* **所有** — 这是一个空的Maven模块，它将上述模块组合为一个可以部署到AEM环境的包。

![Maven项目图](assets/project-setup/project-pom-structure.png)

请参阅[AEM项目原型文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)，了解&#x200B;**所有** Maven模块的更多详细信息。

### 包含核心组件 {#core-components}

[AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)是AEM的一组标准化Web内容管理(WCM)组件。 这些组件提供了一套基本功能，并针对各个项目进行了样式、自定义和扩展。

AEM as a Cloud Service环境包含最新版本的[AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hans)。 因此，为AEM as a Cloud Service生成的项目&#x200B;**不**&#x200B;包含AEM核心组件的嵌入。

对于AEM 6.5/6.4生成的项目，原型会自动在项目中嵌入[AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hans)。 AEM 6.5/6.4的最佳做法是嵌入AEM核心组件，以确保随项目一起部署最新版本。 有关项目如何[包含核心组件的更多信息，请在此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components)找到。

## Source控制管理 {#source-control}

始终最好使用某种形式的源代码管理来管理应用程序中的代码。 本教程使用Git和GitHub。 Maven和/或所选IDE生成的多个文件应被SCM忽略。

当您构建和安装代码包时，Maven会创建一个目标文件夹。 目标文件夹和内容应从SCM中排除。

在下，`ui.apps`模块观察到已创建许多`.content.xml`文件。 这些XML文件映射JCR中安装的内容的节点类型和属性。 这些文件是关键文件，不能忽略&#x200B;**的**。

AEM项目原型生成一个示例`.gitignore`文件，可用作可以安全忽略这些文件的起点。 文件生成于`<src>/aem-guides-wknd/.gitignore`。

## 恭喜！ {#congratulations}

恭喜，您已创建第一个AEM项目！

### 后续步骤 {#next-steps}

通过一个带有[组件基础知识](component-basics.md)教程的简单`HelloWorld`示例了解Adobe Experience Manager (AEM) Sites组件的基础技术。

## 高级Maven命令（附加） {#advanced-maven-commands}

在开发过程中，您可能只使用其中一个模块，并且希望避免构建整个项目以节省时间。 您可能还希望直接部署到AEM Publish实例，或者部署到未在端口4502上运行的AEM实例。

接下来，让我们查看一些其他Maven配置文件和命令，您可以在开发期间使用这些配置文件和命令来提高灵活性。

### 核心模块 {#core-module}

**[core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)**&#x200B;模块包含与项目关联的所有Java™代码。 **core**&#x200B;模块的生成将OSGi捆绑包部署到AEM。 仅构建此模块：

1. 导航到`core`文件夹（`aem-guides-wknd`下）：

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

1. 切换&#x200B;**Id**&#x200B;排序列，您应该会看到已安装并活动的WKND捆绑包。

   ![核心捆绑包](assets/project-setup/wknd-osgi-console.png)

1. 您可以在[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar)中看到jar的“物理”位置：

   Jar![&#128279;](assets/project-setup/jcr-bundle-location.png)的CRXDE位置

### Ui.apps和Ui.content模块 {#apps-content-module}

**[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven模块包含`/apps`下的站点所需的所有渲染代码。 这包括以名为[clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)的AEM格式存储的CSS/JS。 这还包括用于渲染动态HTML的[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html)脚本。 您可以将&#x200B;**ui.apps**&#x200B;模块视为到JCR中的结构的映射，但采用一种可以存储在文件系统上并提交给源代码控制的格式。 **ui.apps**&#x200B;模块仅包含代码。

仅构建此模块：

1. 从命令行中。 导航到`ui.apps`文件夹（`aem-guides-wknd`下）：

   ```shell
   $ cd ../ui.apps
   ```

1. 运行以下命令：

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 导航到[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应该看到`ui.apps`包是第一个安装的包，并且其时间戳应比任何其他包都新。

   已安装![Ui.apps包](assets/project-setup/ui-apps-package.png)

1. 返回到命令行并运行以下命令（在`ui.apps`文件夹中）：

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   配置文件`autoInstallPackagePublish`用于将包部署到在端口&#x200B;**4503**&#x200B;上运行的发布环境。 如果找不到在http://localhost:4503上运行的AEM实例，则会出现上述错误。

1. 最后，运行以下命令在端口&#x200B;**4504**&#x200B;上部署`ui.apps`包：

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

   如果端口&#x200B;**4504**&#x200B;上没有可用的AEM实例，则再次出现生成失败。 在`aem-guides-wknd/pom.xml`处的POM文件中定义了参数`aem.port`。

**[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)**&#x200B;模块的结构与&#x200B;**ui.apps**&#x200B;模块相同。 唯一的区别是&#x200B;**ui.content**&#x200B;模块包含称为&#x200B;**可变**&#x200B;的内容。 **可变**&#x200B;内容基本上是指存储在源代码管理&#x200B;**中的非代码配置，如Templates、Policies或文件夹结构，但**&#x200B;可以直接在AEM实例上修改。 有关页面和模板的一章中将更详细地介绍此功能。

用于构建&#x200B;**ui.apps**&#x200B;模块的相同Maven命令可用于构建&#x200B;**ui.content**&#x200B;模块。 欢迎在&#x200B;**ui.content**&#x200B;文件夹中重复上述步骤。

## 疑难解答

如果使用AEM项目原型生成项目时出现问题，请查看[已知问题的列表](https://github.com/adobe/aem-project-archetype#known-issues)以及未结的[问题的列表](https://github.com/adobe/aem-project-archetype/issues)。

## 再次恭喜！ {#congratulations-bonus}

恭喜您浏览了奖金材料。

### 后续步骤 {#next-steps-bonus}

通过一个带有[组件基础知识](component-basics.md)教程的简单`HelloWorld`示例了解Adobe Experience Manager (AEM) Sites组件的基础技术。
