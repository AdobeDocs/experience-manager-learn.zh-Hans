---
title: AEM Sites — 项目设置入门
description: 创建一个Maven Multi Module项目以管理Experience Manager站点的代码和配置。
version: 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '1839'
ht-degree: 4%

---

# 项目设置 {#project-setup}

本教程介绍如何创建Maven Multi Module项目来管理Adobe Experience Manager站点的代码和配置。

## 前提条件 {#prerequisites}

查看所需的工具和设置说明 [本地开发环境](./overview.md#local-dev-environment). 确保您有新的Adobe Experience Manager实例在本地可用，并且未安装其他示例/演示包（除必需的Service Pack之外）。

## 目标 {#objective}

1. 了解如何使用Maven原型生成新的AEM项目。
1. 了解AEM项目原型生成的各个模块以及它们如何协同工作。
1. 了解AEM项目中如何包含AEM核心组件。

## 您即将构建的内容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

在本章中，您使用生成新的Adobe Experience Manager项目 [AEM项目原型](https://github.com/adobe/aem-project-archetype). 您的AEM项目包含用于Sites实施的完整代码、内容和配置。 本章中生成的项目将作为WKND站点实施的基础，并在以后的章节中构建该项目。

**什么是Maven项目？** - [Apache Maven](https://maven.apache.org/) 是用于构建项目的软件管理工具。 *所有Adobe Experience Manager* 实施使用Maven项目在AEM之上构建、管理和部署自定义代码。

**什么是Maven原型？** - A [Maven原型](https://maven.apache.org/archetype/index.html) 是用于生成新项目的模板或模式。 AEM项目原型有助于生成具有自定义命名空间的新项目，并包括遵循最佳实践的项目结构，从而大大加快了项目开发。

## 创建项目 {#create}

有几个选项可用于为AEM创建Maven多模块项目。 本教程使用 [Maven AEM项目原型 **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager也 [提供UI向导](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html) 启动AEM应用程序项目的创建。 Cloud Manager UI生成的基础项目具有与直接使用原型相同的结构。

>[!NOTE]
>
>本教程使用版本 **35** 原型的。 最佳做法始终是 **最新** 用于生成新项目的原型的版本。

下一系列步骤将使用基于UNIX®的命令行终端来执行，但如果使用Windows终端，则应该类似。

1. 打开命令行终端。 验证是否已安装Maven：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 导航到要在其中生成AEM项目的目录。 这可以是任何您想要维护项目源代码的目录。 例如，名为的目录 `code` 在用户的主目录下：

   ```shell
   $ cd ~/code
   ```

1. 将以下内容粘贴到命令行中以 [在批处理模式下生成项目](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)：

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
   > 要定位AEM 6.5.14+，请替换 `aemVersion="cloud"` 替换为 `aemVersion="6.5.14"`.
   >
   > 此外，始终使用最新的 `archetypeVersion` 通过参考 [AEM项目原型>使用情况](https://github.com/adobe/aem-project-archetype#usage)

   用于配置项目的可用属性的完整列表 [可在此处找到](https://github.com/adobe/aem-project-archetype#available-properties).

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

1. 确保在端口上本地运行的AEM的创作实例 **4502**.
1. 从命令行中，导航到 `aem-guides-wknd` 项目目录。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 运行以下命令以生成整个项目并将其部署到AEM：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   构建大约需要一分钟，并且应该以下列消息结束：

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

   Maven配置文件 `autoInstallSinglePackage` 编译项目的各个模块，并将单个包部署到AEM实例。 默认情况下，此包将部署到本地在端口上运行的AEM实例 **4502** 并且拥有 `admin:admin`.

1. 导航到本地AEM实例上的包管理器： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您应该会看到以下项目的包： `aem-guides-wknd.ui.apps`， `aem-guides-wknd.ui.config`， `aem-guides-wknd.ui.content`、和 `aem-guides-wknd.all`.

1. 导航到站点控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). WKND站点是站点之一。 它包括一个具有美国和语言母版层次结构的站点结构。 此网站层次结构基于 `language_country` 和 `isSingleCountryWebsite` 使用原型生成项目时。

1. 打开 **US** `>` **英语** ，然后单击 **编辑** 菜单栏中的按钮：

   ![站点控制台](assets/project-setup/aem-sites-console.png)

1. 已创建入门内容，并且有多个组件可供添加到页面中。 尝试使用这些组件以了解其功能。 您将在下一章中学习组件的基础知识。

   ![主页入门内容](assets/project-setup/start-home-page.png)

   *原型生成的示例内容*

## Inspect项目 {#project-structure}

生成的AEM项目由单独的Maven模块组成，每个模块具有不同的角色。 本教程和大多数开发都侧重于以下模块：

* [核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java代码，主要是后端开发人员。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)  — 包含CSS、JavaScript、Sass、TypeScript的源代码，主要面向前端开发人员。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)  — 包含组件和对话框定义，将编译的CSS和JavaScript嵌入为客户端库。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 包含结构化内容和配置，如可编辑模板、元数据架构(/content、/conf)。

* **所有**  — 这是一个空的Maven模块，它将上述模块组合到一个可以部署到AEM环境的包中。

![Maven项目图](assets/project-setup/project-pom-structure.png)

请参阅 [AEM项目原型文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 了解更多详细信息 **所有** Maven模块。

### 包含核心组件 {#core-components}

[AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans) 是一组适用于AEM的标准化Web内容管理(WCM)组件。 这些组件提供了一套功能的基准集，并针对各个项目进行了样式、自定义和扩展。

AEMas a Cloud Service环境包括最新版本的 [AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans). 因此，为AEMas a Cloud Service生成的项目会 **非** 包括AEM核心组件的嵌入。

对于AEM 6.5/6.4生成的项目，原型会自动嵌入 [AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans) 在项目中。 AEM 6.5/6.4的最佳实践是嵌入AEM核心组件，以确保随项目一起部署最新版本。 有关核心组件如何使用的更多信息 [可以在此处找到项目中包含的内容](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## 源代码控制管理 {#source-control}

始终最好使用某种形式的源代码管理来管理应用程序中的代码。 本教程使用Git和GitHub。 有一些由Maven和/或所选IDE生成的文件应被SCM忽略。

每当您构建和安装代码包时，Maven都会创建一个目标文件夹。 目标文件夹和内容应从SCM中排除。

在下 `ui.apps` 模块观察到许多 `.content.xml` 创建文件。 这些XML文件映射JCR中安装的内容的节点类型和属性。 这些文件非常重要，并且 **无法** 将被忽略。

AEM项目原型生成一个示例 `.gitignore` 可用作起始点的文件，可以安全地忽略这些文件。 文件生成于 `<src>/aem-guides-wknd/.gitignore`.

## 恭喜！ {#congratulations}

恭喜，您已创建第一个AEM项目！

### 后续步骤 {#next-steps}

通过简单的步骤了解Adobe Experience Manager (AEM) Sites组件的基础技术 `HelloWorld` 示例包含 [组件基础知识](component-basics.md) 教程。

## 高级Maven命令（附加） {#advanced-maven-commands}

在开发过程中，您可能只使用其中一个模块，并且希望避免构建整个项目以节省时间。 您可能还希望直接部署到AEM Publish实例，或部署到未在端口4502上运行的AEM实例。

接下来，让我们查看一些其他Maven配置文件和命令，您可以在开发期间使用这些配置文件和命令来提高灵活性。

### 核心模块 {#core-module}

此 **[核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** 模块包含与项目关联的所有Java™代码。 的构建 **核心** 模块将OSGi捆绑包部署到AEM。 仅构建此模块：

1. 导航到 `core` 文件夹（在下） `aem-guides-wknd`)：

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

1. 导航到 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). 这是OSGi Web控制台，包含有关AEM实例上安装的所有捆绑包的信息。

1. 切换 **Id** 排序列中，您应该会看到已安装并活动的WKND捆绑包。

   ![核心捆绑包](assets/project-setup/wknd-osgi-console.png)

1. 您可以在中查看jar的“物理”位置 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar)：

   ![Jar的CRXDE位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模块 {#apps-content-module}

此 **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven模块包含下方的站点所需的所有渲染代码 `/apps`. 这包括以AEM格式(称为 [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). 这还包括 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 用于呈现动态HTML的脚本。 你可以想到 **ui.apps** 模块映射到JCR中的结构，但采用可以存储在文件系统上并提交给源代码控制的格式。 此 **ui.apps** 模块仅包含代码。

仅构建此模块：

1. 从命令行中。 导航到 `ui.apps` 文件夹（在下） `aem-guides-wknd`)：

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

1. 导航到 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您应会看到 `ui.apps` 包作为第一个安装的包，并且其时间戳应比任何其他包都新。

   ![已安装Ui.apps包](assets/project-setup/ui-apps-package.png)

1. 返回到命令行并运行以下命令(在 `ui.apps` 文件夹)：

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

   配置文件 `autoInstallPackagePublish` 用于将包部署到在端口上运行的发布环境 **4503**. 如果找不到在http://localhost:4503上运行的AEM实例，则会出现上述错误。

1. 最后，运行以下命令以部署 `ui.apps` 端口上的包 **4504**：

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

   同样，如果端口上没有运行AEM实例，则预计会出现生成失败 **4504** 可用。 参数 `aem.port` 在POM文件中定义 `aem-guides-wknd/pom.xml`.

此 **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** 模块的结构与 **ui.apps** 模块。 唯一的区别是 **ui.content** 模块包含所谓的 **可变** 内容。 **可变** 内容实质上是指存储在源代码管理中的非代码配置，例如模板、策略或文件夹结构 **但是** 可以直接在AEM实例上修改。 有关页面和模板的一章中对此进行了详细探讨。

用于构建 **ui.apps** 模块可用于构建 **ui.content** 模块。 欢迎您从 **ui.content** 文件夹。

## 疑难解答

如果在使用AEM项目原型生成项目时出现问题，请参阅 [已知问题](https://github.com/adobe/aem-project-archetype#known-issues) 和打开的列表 [问题](https://github.com/adobe/aem-project-archetype/issues).

## 再次恭喜！ {#congratulations-bonus}

恭喜，您浏览了奖金材料。

### 后续步骤 {#next-steps-bonus}

通过简单的步骤了解Adobe Experience Manager (AEM) Sites组件的基础技术 `HelloWorld` 示例包含 [组件基础知识](component-basics.md) 教程。
