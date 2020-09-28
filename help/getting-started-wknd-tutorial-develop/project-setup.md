---
title: AEM Sites入门——项目设置
seo-title: AEM Sites入门——项目设置
description: 涵盖创建Maven多模块项目以管理AEM站点的代码和配置。
sub-product: 站点
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 3%

---


# 项目设置 {#project-setup}

本教程介绍如何创建Maven多模块项目，以管理Adobe Experience Manager站点的代码和配置。

## 前提条件 {#prerequisites}

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。 确保您在本地有一个新的Adobe Experience Manager实例，并且未安装任何其他示例／演示包（必需的服务包除外）。

## 目标 {#objective}

1. 了解如何使用Maven原型生成新的AEM项目。
1. 了解AEM Project Archetype生成的不同模块以及它们如何协同工作。
1. 了解AEM核心组件如何包含在AEM项目中。

## 您将构建的内容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

在本章中，您将使用AEM Project Archetype生成一个新的 [Adobe Experience Manager项目](https://github.com/adobe/aem-project-archetype)。 您的AEM项目包含用于站点实施的所有代码、内容和配置。 本章中生成的项目将作为执行WKND网站的基础，并将在今后各章中加以建立。

## 背景 {#background}

**什么是Maven项目？** - [Apache Maven](https://maven.apache.org/) 是用于构建项目的软件管理工具。 *所有Adobe Experience Manager* 实施都使用Maven项目在AEM之上构建、管理和部署自定义代码。

**什么是马文原型？** - Maven [原型](https://maven.apache.org/archetype/index.html) 是生成新项目的模板或模式。 AEM Project原型允许我们使用自定义命名空间生成新项目，并包含遵循最佳实践的项目结构，从而大大加快了我们的项目。

## 创建项目 {#create}

为AEM创建Maven多模块项目有几个选项。 本教程将利用 [Maven AEM Project Archetype **22**](https://github.com/adobe/aem-project-archetype)。 云管理器还 [提供UI向导](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) ，用于启动AEM应用程序项目的创建。 云管理器UI生成的基础项目与直接使用原型的结构相同。

>[!NOTE]
>
>为了完成本教程的学习，请使 **用** 原型版本22。 但是，使用最新版原型生成 **新项目** ，始终是一种最佳做法。

接下来的一系列步骤将使用基于UNIX的命令行终端进行，但在使用Windows终端时应类似。

1. 打开命令行终端并验证Maven是否已安装并添加到命令行路径：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 通过运 **行以下命令** ，验证adobe-public用户档案是否处于活动状态：

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

   如果看 **不到** adobe- **public** ，则表明您的文件中未正确引用Adobe回 `~/.m2/settings.xml` 购协议。 请重新访问在本地开发环境中安装和 [配置Apache Maven的步骤](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven)。

1. 导航到要在其中生成AEM项目的目录。 这可以是要维护项目源代码的任何目录。 例如，在用户主 `code` 目录下命名的目录：

   ```shell
   $ cd ~/code
   ```

1. 将以下内容粘贴到命令行 [中以批处理模式生成项目](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >默认情况下，从Maven原型生成项目时使用交互模式。 为避免对我们使用批处理模式生成的任何值进行脂肪分配。 还可以使用AEM Developer Tools插件为Eclipse创 [建Maven AEM项目](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/aem-eclipse.html)。

   >[!CAUTION]
   >
   >如果您收到如下错误： *无法对project standalone-pom执行goal org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate(default-cli):所需的原型不存在*。 这表示您的文件中未正确引用Adobe回 `~/.m2/settings.xml` 购。 请重新访问前面的步骤，并验证settings.xml文件是否引用了Adobe回购。

   下表列表了本教程使用的值：

   | 名称 | 值 | 描述 |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | 基本Maven groupId |
   | artifactId | aem-guides-wknd | 基Maven ArtifactId |
   | 版本 | 0.0.1-快照 | 版本 |
   | 包装 | com.adobe.aem.guides.wknd | Java源包 |
   | appsFolderName | wknd | /apps文件夹名称 |
   | artifactName | WKND Sites项目 | Maven项目名称 |
   | componentGroupName | WKND | AEM组件组名称 |
   | contentFolderName | wknd | /content文件夹名称 |
   | confFolderName | wknd | /conf文件夹名称 |
   | cssId | wknd | 生成的css中使用的前缀 |
   | packageGroup | wknd | 内容包组名称 |
   | siteName | WKND站点 | AEM站点名称 |
   | optionAemVersion | 6.5.0 | 目标AEM版本 |
   | language_country | en_us | 语言／国家／地区代码，用于创建内容结构（例如en_us） |
   | optionIncludeExamples | y | 包括组件库示例站点 |
   | optionIncludeErrorHandler | n | 包含自定义404响应页 |
   | optionIncludeFrontendModule | y | 包括专用前端模块 |
   | isSingleCountryWebsite | n | 在示例内容中创建语言主控结构 |
   | optionDispatcherConfig | 无 | 生成调度程序配置模块 |

1. 以下文件夹和文件结构将由本地文件系统上的Maven原型生成：

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 构建项目 {#build}

现在，我们已生成了一个新项目，可以将项目代码部署到AEM的本地实例。

1. 确保您有一个AEM实例在端口4502上本地 **运行**。
1. 从命令行导航到项目 `aem-guides-wknd` 目录。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 运行以下命令以构建整个项目并将其部署到AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven用户档案 `autoInstallSinglePackage` 编译项目的各个模块并将单个包部署到AEM实例。 默认情况下，此包将部署到AEM实例，该实例在端口4502 **上本地运** 行，凭据为 `admin:admin`。

1. 导航到本地AEM实例上的包管理器： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应该看到三个 `aem-guides-wknd.ui.apps`包， `aem-guides-wknd.ui.content`分别为、和 `aem-guides-wknd.all`。

   您还应当看到AEM核心 [组件的多个包](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html) ，它们按原型包含在项目中。 本教程的稍后部分将介绍这一内容。

1. 导航到站点控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。 WKND站点将是其中一个站点。 它将包括具有美国和语言母版层次结构的站点结构。 此站点层次结构基于使用原型生成 `language_country` 项目 `isSingleCountryWebsite` 时的相应值。

1. 选择“美 **国** 英 `>` 语 **”页面，然后单** 击菜单栏 **中的“编辑** ”按钮：

   ![站点控制台](assets/project-setup/aem-sites-console.png)

1. 已创建某些内容，并且可以向页面添加多个组件。 尝试这些组件，了解其功能。 本教程稍后将详细探讨如何配置此页面和组件。

## Inspect项目 {#project-structure}

AEM原型由单个Maven模块组成：

* [核心](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - Java捆绑包，包含所有核心功能(如OSGi服务、监听器或调度程序)，以及与组件相关的Java代码(如servlet或请求过滤器)。
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) —— 包含项目的/apps部分，即JS&amp;CSS客户端库、组件和OSGi配置
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) —— 包含结构内容和配置，如可编辑的模板、元数据模式(/content、/conf)
* ui.tests —— 包含服务器端执行的JUnit测试的Java捆绑包。 此捆绑包不会部署到生产上。
* ui.launcher —— 包含将ui.tests捆绑包（和依赖捆绑包）部署到服务器并触发远程JUnit执行的粘胶代码
* [ui.frontend](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/uifrontend.html) -（可选）包含使用基于Webpack的前端构建模块所需的对象。
* 全部——这是一个空的Maven模块，它将上述模块组合到单个包中，并可部署到AEM环境。

![Maven项目图](assets/project-setup/project-pom-structure.png)

请参阅AEM [Project Archetype文档](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html) ，了解Maven模块的更多详细信息。

## 高级Maven命令 {#advanced-maven-commands}

在开发过程中，您可能只使用其中一个模块，并希望避免构建整个项目以节省时间。 您可能还希望直接部署到AEM发布实例，或者部署到不在端口4502上运行的AEM实例。

接下来，我们将看到一些Maven用户档案和命令，您可以在开发过程中使用它们来实现更大的灵活性。

### 核心模块 {#core-module}

核 **[心模块](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** ，包含与项目相关的所有Java代码。 构建后，它将OSGi捆绑包部署到AEM。 仅构建此模块：

1. 导览至文 `core` 件夹(下方 `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. 运行以下命令：

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 导航到 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)。 这是OSGi Web控制台，包含有关安装在AEM实例上的所有捆绑包的信息。

1. 切换Id **排序** 列，您应看到WKND捆绑已安装并处于活动状态。

   ![核心捆绑](assets/project-setup/wknd-osgi-console.png)

1. 您可以在CRXDE-Lite中看到jar的“物 [理”位置](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar):

   ![CRXDE Location of Jar](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模块 {#apps-content-module}

ui. **[apps授权模块](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** ，包含下方站点所需的所有渲染代码 `/apps`。 这包括将以AEM格式存储的CSS/JS，该格式名为 [clientlibs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)。 这还包括用 [于呈现](https://docs.adobe.com/docs/en/htl/overview.html) 动态HTML的HTL脚本。 您可以将ui. **apps模块视为** JCR中结构的映射，但其格式可以存储在文件系统上并提交到源代码控制。 ui. **apps模块仅** 包含代码。

要构建仅此模块，请执行以下操作：

1. 从命令行。 导览至文 `ui.apps` 件夹(下方 `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. 运行以下命令：

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 导航到 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应将该包视 `ui.apps` 为第一个安装的包，它应具有比任何其他包更新的时间戳。

   ![已安装UI.apps包](assets/project-setup/ui-apps-package.png)

1. 返回命令行并运行以下命令(在文件夹 `ui.apps` 中):

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

   该用户档案 `autoInstallPackagePublish` 用于将包部署到在端口4503上运行的发布环境 **中**。 如果找不到在http://localhost:4503上运行的AEM实例，则会出现上述错误。

1. 最后，运行以下命令在端口 `ui.apps` 4504上部署 **包**:

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

   如果端口4504上没有运行的AEM实例可用，则预计会发 **生生成** 失败。 该参 `aem.port` 数在POM文件中定义 `aem-guides-wknd/pom.xml`。

ui. **[content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** 模块的结构与ui.apps模 **块相同** 。 唯一的区别是ui. **content** 模块包含所谓的 **可变内容** 。 **可变** 内容本质上是指非代码配置，如存储在源代码控件中但可以直接在AEM实例上修 **改的** “模板”、“策略”或“文件夹结构”。 这将在页面和模板的一章中进行更详细的探讨。 目前，重要的一点是，用于构建ui.apps模块的 **相同Maven命令** ，可用于构 **建ui.content** 模块。 您可以从ui.content文件夹中重复 **上述步骤** 。

### Ui.frontend模块 {#ui-frontend-module}

ui. **[frontend](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模块是Maven模块，实际上是 [Webpack项目](https://webpack.js.org/) 。 此模块设置为一个专用的前端构建系统，它输出JavaScript和CSS文件，这些文件又部署到AEM。 开 **发人员** ui.front模块允许使用Sass、TypeScript [、npm模](https://sass-lang.com/)块等语言编 [码，并](https://www.typescriptlang.org/)[](https://www.npmjs.com/) 直接将输出集成到AEM中。

在 **客户端库** 和前端开发的章节中将更详细地介绍ui.frontend模块。 现在，我们来看一下如何将它集成到项目中。

1. 从命令行。 导览至文 `ui.frontend` 件夹(下方 `aem-guides-wknd`):

   ```shell
   $ cd ../ui.frontend
   ```

1. 运行以下命令：

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   注意这些行 `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`。 这表示已编译的CSS和JS正被复制到文件 `ui.apps` 夹中。

1. 视图文件的修改时间戳 `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`。 它应该比模块中的其他文件更新得 `ui.apps` 更新。

   与我们查看的其他模块不同， **ui.frontend** 模块不直接部署到AEM。 而是将CSS和JS复制到 **ui.apps** 模块，然后 **将ui.apps模块** 部署到AEM。 如果您从第一个Maven命令查看构建顺序，您会看到 **ui.frontend** 始终在ui.apps *之* 前构建 **。**

   在本教程的稍后部分，我们将介绍ui. **frontend模块和嵌入式** webpack开发服务器的高级功能，以实现快速开发。

## 包含核心组件 {#core-components}

原型自动嵌 [入项目中的AEM](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html) Core Components。 以前，在检查部署到AEM的包时，会包括多个与核心组件相关的包。 核心组件是一套旨在加快AEM Sites项目开发的基础组件。 核心组件是开放源代码，可在 [GitHub上使用](https://github.com/adobe/aem-core-wcm-components)。 有关如何在项目中包含核 [心组件的更多信息，请参阅此处](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components)。

1. 使用您最喜爱的文本编辑器打开 `aem-guides-wknd/pom.xml`。

1. 搜索 `core.wcm.components.version`. 这将向您显示包含的核心组件版本：

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > AEM Project Archetype将包含AEM核心组件的某个版本，但这些项目具有不同的发布周期，因此包含的核心组件版本可能不是最新版本。 作为最佳实践，您应始终寻求利用最新版核心组件。 新功能和错误修复会频繁更新。 可在GitHub [上找到最新版本信息](https://github.com/adobe/aem-core-wcm-components/releases)。

1. 如果向下滚动到该部 `dependencies` 分，您应看到单个核心组件依赖关系：

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## 源代码控制管理 {#source-control}

使用某种形式的源代码控件管理应用程序中的代码总是一个好主意。 本教程使用git和GitHub。 有几个文件由Maven和／或所选IDE生成，SCM应忽略这些文件。

Maven将在您构建和安装代码包时创建目标文件夹。 目标文件夹和内容应从SCM中排除。

在ui.apps下，您还会注意到许多已创建的。content.xml文件。 这些XML文件映射JCR中安装的内容的节点类型和属性。 这些文件很重要， **不应** 忽略它们。

AEM项目原型将生成一个样 `.gitignore` 例文件，该文件可用作可安全忽略文件的起点。 文件在上生成 `<src>/aem-guides-wknd/.gitignore`。

## 审核 {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## 恭喜！ {#congratulations}

祝贺您，您刚刚创建了您的第一个AEM项目！

### 后续步骤 {#next-steps}

通过组件基础知识教程的简单示例，了解Adobe Experience Manager(AEM)站 `HelloWorld` 点组件的 [基础技术](component-basics.md) 。
