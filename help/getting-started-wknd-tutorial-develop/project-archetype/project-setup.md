---
title: AEM Sites入门 — 项目设置
seo-title: Getting Started with AEM Sites - Project Setup
description: 介绍如何创建Maven多模块项目以管理AEM站点的代码和配置。
sub-product: sites
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
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '1818'
ht-degree: 3%

---

# 项目设置 {#project-setup}

本教程介绍如何创建Maven多模块项目以管理Adobe Experience Manager站点的代码和配置。

## 前提条件 {#prerequisites}

查看设置 [本地开发环境](./overview.md#local-dev-environment). 确保您在本地提供了新的Adobe Experience Manager实例，并且未安装其他示例/演示包（所需的Service Pack除外）。

## 目标 {#objective}

1. 了解如何使用Maven原型生成新的AEM项目。
1. 了解AEM项目原型生成的不同模块以及它们如何协同工作。
1. 了解AEM核心组件如何包含在AEM项目中。

## 将构建的内容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

在本章中，您将使用 [AEM项目原型](https://github.com/adobe/aem-project-archetype). 您的AEM项目包含用于Sites实施的所有代码、内容和配置。 本章中生成的项目将作为实施WKND站点的基础，并将在以后的章节中构建。

**什么是Maven项目？** - [阿帕奇·马文](https://maven.apache.org/) 是用于构建项目的软件管理工具。 *全部Adobe Experience Manager* 实施使用Maven项目来基于AEM构建、管理和部署自定义代码。

**什么是Maven原型？** - A [Maven原型](https://maven.apache.org/archetype/index.html) 是用于生成新项目的模板或模式。 AEM项目原型允许我们生成具有自定义命名空间的新项目，并包含遵循最佳实践的项目结构，从而大大加快了我们的项目速度。

## 创建项目 {#create}

有几个选项可用于为AEM创建Maven多模块项目。 本教程将利用 [Maven AEM项目原型 **35**](https://github.com/adobe/aem-project-archetype). Cloud Manager [提供UI向导](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/create-application-project/using-the-wizard.html) 启动创建AEM应用程序项目。 由Cloud Manager UI生成的基础项目的结构与直接使用原型的结构相同。

>[!NOTE]
>
>本教程使用的是版本 **35** 原型。 使用 **最新** 原型版本以生成新项目。

接下来的一系列步骤将使用基于UNIX的命令行终端进行，但如果使用Windows终端，则应类似。

1. 打开命令行终端。 验证Maven是否已安装：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 导航到要在其中生成AEM项目的目录。 这可以是要在其中维护项目源代码的任何目录。 例如，名为的目录 `code` 在用户的主目录下：

   ```shell
   $ cd ~/code
   ```

1. 将以下内容粘贴到命令行中以 [以批处理模式生成项目](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=35 \
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
   > 如果定位AEM 6.5.10+，请替换 `aemVersion="cloud"` with `aemVersion="6.5.10"`.

   用于配置项目的可用属性的完整列表 [可在此处找到](https://github.com/adobe/aem-project-archetype#available-properties).

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

## 部署和构建项目 {#build}

生成项目代码并将其部署到AEM的本地实例。

1. 确保AEM的创作实例在端口上本地运行 **4502**.
1. 从命令行中，导航到 `aem-guides-wknd` 项目目录。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 运行以下命令以构建整个项目并将其部署到AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   内部版本将花费大约一分钟，且应以以下消息结束：

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

   Maven个人资料 `autoInstallSinglePackage` 编译项目的各个模块，并将单个包部署到AEM实例。 默认情况下，此包将部署到端口上本地运行的AEM实例 **4502** 并拥有 `admin:admin`.

1. 导航到本地AEM实例上的包管理器： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您应会看到 `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`和 `aem-guides-wknd.all`.

1. 导航到站点控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). WKND站点将是其中一个站点。 它将包括一个具有美国和语言母版层次结构的网站结构。 此网站层次结构基于 `language_country` 和 `isSingleCountryWebsite` 使用原型生成项目时。

1. 打开 **美国** `>` **英语** 页面，方法是选择页面并单击 **编辑** 按钮：

   ![站点控制台](assets/project-setup/aem-sites-console.png)

1. 已创建起始内容，可向页面添加多个组件。 试用这些组件，了解功能。 您将在下一章中学习组件的基础知识。

   ![主页入门内容](assets/project-setup/start-home-page.png)

   *原型生成的示例内容*

## Inspect项目 {#project-structure}

生成的AEM项目由各个Maven模块组成，每个模块具有不同的角色。 本教程和大部分开发工作都侧重于以下模块：

* [核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java代码，主要是后端开发人员。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)  — 包含CSS、JavaScript、Sass、Type Script的源代码，主要面向前端开发人员。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)  — 包含组件和对话框定义，将编译的CSS和JavaScript嵌入为客户端库。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 包含结构性内容和配置，如可编辑的模板、元数据架构(/content， /conf)。

* **全部**  — 这是一个空的Maven模块，将上述模块合并到一个可以部署到AEM环境的包中。

![Maven项目图](assets/project-setup/project-pom-structure.png)

请参阅 [AEM项目原型文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 要了解更多详细信息，请 **全部** Maven模块。

### 包括核心组件 {#core-components}

[AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans) 是一组适用于AEM的标准化Web内容管理(WCM)组件。 这些组件提供了一组功能的基线，旨在为各个项目设置样式、自定义和扩展。

AEMas a Cloud Service环境包括 [AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). 因此，为AEMas a Cloud Service生成的项目会执行 **not** 包括嵌入的AEM核心组件。

对于AEM 6.5/6.4生成的项目，原型会自动嵌入 [AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 中。 嵌入AEM核心组件是AEM 6.5/6.4的最佳实践，目的是确保在您的项目中部署最新版本。 有关核心组件的更多信息 [项目中包含的内容可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## 源代码管理 {#source-control}

最好使用某种形式的源代码控件来管理应用程序中的代码。 本教程使用git和GitHub。 Maven和/或所选IDE生成的多个文件应被SCM忽略。

无论您何时构建并安装代码包，Maven都会创建目标文件夹。 目标文件夹和内容应从SCM中排除。

下 `ui.apps` 请注意 `.content.xml` 文件。 这些XML文件映射在JCR中安装的内容的节点类型和属性。 这些文件至关重要，应该 **not** 被忽略。

AEM项目原型将生成一个示例 `.gitignore` 可用作文件安全忽略起点的文件。 文件在 `<src>/aem-guides-wknd/.gitignore`.

## 恭喜！ {#congratulations}

恭喜，您刚刚创建了第一个AEM项目！

### 后续步骤 {#next-steps}

通过简单的操作，了解Adobe Experience Manager(AEM)站点组件的基础技术 `HelloWorld` 示例 [组件基础知识](component-basics.md) 教程。

## 高级Maven命令（附加） {#advanced-maven-commands}

在开发过程中，您可能只使用其中一个模块，并且希望避免构建整个项目以节省时间。 您可能还希望直接部署到AEM发布实例，或者可能部署到未在端口4502上运行的AEM实例。

接下来，我们将查看一些额外的Maven配置文件和命令，在开发过程中，您可以使用这些配置文件和命令来提高灵活性。

### 核心模块 {#core-module}

的 **[核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** 模块包含与项目关联的所有Java代码。 构建后，它会将OSGi包部署到AEM。 要仅构建此模块，请执行以下操作：

1. 导航到 `core` 文件夹（下方） `aem-guides-wknd`):

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

1. 导航到 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). 这是OSGi Web控制台，其中包含有关在AEM实例上安装的所有包的信息。

1. 切换 **Id** 排序列，您应会看到WKND包已安装并处于活动状态。

   ![核心包](assets/project-setup/wknd-osgi-console.png)

1. 您可以在 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![CRXDE Jar的位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模块 {#apps-content-module}

的 **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven模块包含站点下方所需的所有渲染代码 `/apps`. 这包括将以名为的AEM格式存储的CSS/JS [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). 这还包括 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=zh-Hans) 用于呈现动态HTML的脚本。 您可以考虑 **ui.apps** 模块作为JCR中结构的映射，但格式可以存储在文件系统中并提交到源代码控制。 的 **ui.apps** 模块仅包含代码。

要构建仅此模块，请执行以下操作：

1. 从命令行中。 导航到 `ui.apps` 文件夹（下方） `aem-guides-wknd`):

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

1. 导航到 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您应会看到 `ui.apps` 包作为第一个安装的包，且其时间戳应比任何其他包的时间戳都更新。

   ![已安装Ui.apps包](assets/project-setup/ui-apps-package.png)

1. 返回到命令行并运行以下命令(在 `ui.apps` 文件夹):

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

   用户档案 `autoInstallPackagePublish` 用于将包部署到端口上运行的发布环境 **4503**. 如果找不到在http://localhost:4503上运行的AEM实例，则需要出现上述错误。

1. 最后，运行以下命令以部署 `ui.apps` 端口上的包 **4504**:

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

   如果端口上没有运行AEM实例，则预期会再次发生生成失败 **4504** 中。 参数 `aem.port` 在的POM文件中定义 `aem-guides-wknd/pom.xml`.

的 **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** 模块的结构与 **ui.apps** 模块。 唯一的区别是 **ui.content** 模块包含称为 **可变** 内容。 **可变** 内容主要是指存储在源代码管理中的模板、策略或文件夹结构等非代码配置 **但** 可以直接在AEM实例上修改。 有关详细信息，请参阅页面和模板一章。

用于构建 **ui.apps** 模块可用于构建 **ui.content** 模块。 您可以在 **ui.content** 文件夹。

## 疑难解答

如果您在使用AEM项目原型生成项目时遇到问题，请参阅 [已知问题](https://github.com/adobe/aem-project-archetype#known-issues) 和打开列表 [问题](https://github.com/adobe/aem-project-archetype/issues).
