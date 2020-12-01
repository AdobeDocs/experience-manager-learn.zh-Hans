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


# 项目设置{#project-setup}

本教程介绍如何创建Maven多模块项目，以管理Adobe Experience Manager站点的代码和配置。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。 确保您在本地有一个新的Adobe Experience Manager实例，并且未安装任何其他示例／演示包（必需的服务包除外）。

## 目标 {#objective}

1. 了解如何使用Maven原型生成新的AEM项目。
1. 了解AEM Project Archetype生成的不同模块以及它们如何协同工作。
1. 了解AEM核心组件如何包含在AEM项目中。

## 您将构建的{#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

在本章中，您将使用[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)生成一个新的Adobe Experience Manager项目。 您的AEM项目包含用于站点实施的所有代码、内容和配置。 本章中生成的项目将作为执行WKND网站的基础，并将在今后各章中加以建立。

## 背景 {#background}

**什么是Maven项目？** -  [Apache ](https://maven.apache.org/) Mavenis是用于构建项目的软件管理工具。*所有Adobe Experience Manager* 实施都使用Maven项目在AEM之上构建、管理和部署自定义代码。

**什么是马文原型？** - Maven原 [型](https://maven.apache.org/archetype/index.html) 是用于生成新项目的模板或模式。AEM Project原型允许我们使用自定义命名空间生成新项目，并包含遵循最佳实践的项目结构，从而大大加快了我们的项目。

## 创建项目{#create}

为AEM创建Maven多模块项目有几个选项。 本教程将利用[Maven AEM Project Archetype **22**](https://github.com/adobe/aem-project-archetype)。 云管理器还[提供UI向导](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html)以启动AEM应用程序项目的创建。 云管理器UI生成的基础项目与直接使用原型的结构相同。

>[!NOTE]
>
>为了完成本教程的学习，请使用原型版本&#x200B;**22**。 但是，使用&#x200B;**最新**&#x200B;版本的原型生成新项目始终是最佳做法。

接下来的一系列步骤将使用基于UNIX的命令行终端进行，但在使用Windows终端时应类似。

1. 打开命令行终端并验证Maven是否已安装并添加到命令行路径：

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

   如果&#x200B;**不**&#x200B;看到&#x200B;**adobe-public**，则表明您的`~/.m2/settings.xml`文件中未正确引用Adobe回购。 请重新访问在[本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven)中安装和配置Apache Maven的步骤。

1. 导航到要在其中生成AEM项目的目录。 这可以是要维护项目源代码的任何目录。 例如，在用户主目录下名为`code`的目录：

   ```shell
   $ cd ~/code
   ```

1. 将以下内容粘贴到命令行中，以在批处理模式[中生成项目：](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)

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
   >默认情况下，从Maven原型生成项目时使用交互模式。 为避免对我们使用批处理模式生成的任何值进行脂肪分配。 还可以使用[用于Eclipse的AEM开发人员工具插件](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/aem-eclipse.html)创建Maven AEM项目。

   >[!CAUTION]
   >
   >如果您收到如下错误：*无法对project standalone-pom执行goal org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate(default-cli):所需的原型不存在*。 这表示您的`~/.m2/settings.xml`文件中未正确引用Adobe回购。 请重新访问前面的步骤，并验证settings.xml文件是否引用了Adobe回购。

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

## 构建项目{#build}

现在，我们已生成了一个新项目，可以将项目代码部署到AEM的本地实例。

1. 确保在端口&#x200B;**4502**&#x200B;上运行AEM的实例。
1. 从命令行导航到`aem-guides-wknd`项目目录。

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

   Maven用户档案`autoInstallSinglePackage`编译项目的各个模块，并将单个包部署到AEM实例。 默认情况下，此包将部署到在端口&#x200B;**4502**&#x200B;上本地运行且凭据为`admin:admin`的AEM实例。

1. 导航到本地AEM实例上的包管理器：[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应当看到`aem-guides-wknd.ui.apps`、`aem-guides-wknd.ui.content`和`aem-guides-wknd.all`的三个软件包。

   您还应看到[AEM核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)的多个包，它们按原型包含在项目中。 本教程的稍后部分将介绍这一内容。

1. 导航到站点控制台：[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。 WKND站点将是其中一个站点。 它将包括具有美国和语言母版层次结构的站点结构。 当使用原型生成项目时，此站点层次结构基于`language_country`和`isSingleCountryWebsite`的值。

1. 选择&#x200B;**US** `>` **English**&#x200B;页面，然后单击菜单栏中的&#x200B;**编辑**&#x200B;按钮：

   ![站点控制台](assets/project-setup/aem-sites-console.png)

1. 已创建某些内容，并且可以向页面添加多个组件。 尝试这些组件，了解其功能。 本教程稍后将详细探讨如何配置此页面和组件。

## Inspect项目{#project-structure}

AEM原型由单个Maven模块组成：

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - Java捆绑包包含所有核心功能(如OSGi服务、监听器或调度程序)，以及组件相关的Java代码(如servlet或请求过滤器)。
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) -包含项目的/apps部分，即JS&amp;CSS客户端、组件和OSGi配置
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) -包含结构内容和配置，如可编辑的模板、元数据模式(/content、/conf)
* ui.tests —— 包含服务器端执行的JUnit测试的Java捆绑包。 此捆绑包不会部署到生产上。
* ui.launcher —— 包含将ui.tests捆绑包（和依赖捆绑包）部署到服务器并触发远程JUnit执行的粘胶代码
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) -（可选）包含使用基于Webpack的前端构建模块所需的对象。
* 全部——这是一个空的Maven模块，它将上述模块组合到单个包中，并可部署到AEM环境。

![Maven项目图](assets/project-setup/project-pom-structure.png)

请参阅[AEM项目原型文档](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)以详细了解Maven模块。

## 高级Maven命令{#advanced-maven-commands}

在开发过程中，您可能只使用其中一个模块，并希望避免构建整个项目以节省时间。 您可能还希望直接部署到AEM发布实例，或者部署到不在端口4502上运行的AEM实例。

接下来，我们将看到一些Maven用户档案和命令，您可以在开发过程中使用它们来实现更大的灵活性。

### 核心模块{#core-module}

**[核心](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)**&#x200B;模块包含与项目相关的所有Java代码。 构建后，它将OSGi捆绑包部署到AEM。 仅构建此模块：

1. 导航到`core`文件夹（位于`aem-guides-wknd`下）:

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

1. 导航到[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)。 这是OSGi Web控制台，包含有关安装在AEM实例上的所有捆绑包的信息。

1. 切换&#x200B;**Id**&#x200B;排序列，您应看到WKND包已安装并处于活动状态。

   ![核心捆绑](assets/project-setup/wknd-osgi-console.png)

1. 您可以在[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar)中看到jar的“physical”位置：

   ![CRXDE Location of Jar](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模块{#apps-content-module}

**[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)**&#x200B;主模块包含`/apps`下站点所需的所有渲染代码。 这包括将以AEM格式存储的CSS/JS，该格式名为[clientlibs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)。 这还包括用于呈现动态HTML的[HTL](https://docs.adobe.com/docs/en/htl/overview.html)脚本。 您可以将&#x200B;**ui.apps**&#x200B;模块视为JCR中结构的映射，但格式可以存储在文件系统上并提交到源代码控制。 **ui.apps**&#x200B;模块只包含代码。

要构建仅此模块，请执行以下操作：

1. 从命令行。 导航到`ui.apps`文件夹（位于`aem-guides-wknd`下）:

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

1. 导航到[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您应将`ui.apps`包看作第一个安装的包，它应具有比任何其他包更新的时间戳。

   ![已安装UI.apps包](assets/project-setup/ui-apps-package.png)

1. 返回命令行并运行以下命令（在`ui.apps`文件夹中）:

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

   用户档案`autoInstallPackagePublish`用于将包部署到在端口&#x200B;**4503**&#x200B;上运行的发布环境。 如果找不到在http://localhost:4503上运行的AEM实例，则会出现上述错误。

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

   如果端口&#x200B;**4504**&#x200B;上没有运行的AEM实例可用，则同样会发生生成失败。 参数`aem.port`在POM文件`aem-guides-wknd/pom.xml`中定义。

**[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)**&#x200B;模块的结构与&#x200B;**ui.apps**&#x200B;模块相同。 唯一的区别是&#x200B;**ui.content**&#x200B;模块包含所谓的&#x200B;**mutable**&#x200B;内容。 **多** 表集内容主要指非代码配置，如存储在源代码控制中的模板、策略或文件夹结构，但 **** 可以直接在AEM实例上修改。这将在页面和模板的一章中进行更详细的探讨。 目前，重要的一点是，用于构建&#x200B;**ui.apps**&#x200B;模块的相同Maven命令可用于构建&#x200B;**ui.content**&#x200B;模块。 您可以从&#x200B;**ui.content**&#x200B;文件夹中重复上述步骤。

### Ui.frontend模块{#ui-frontend-module}

**[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;模块是Maven模块，实际上是[webpack](https://webpack.js.org/)项目。 此模块设置为一个专用的前端构建系统，它输出JavaScript和CSS文件，这些文件又部署到AEM。 **ui.frontend**&#x200B;模块允许开发人员使用[Sass](https://sass-lang.com/)、[TypeScript](https://www.typescriptlang.org/)等语言进行编码，使用[npm](https://www.npmjs.com/)模块并将输出直接集成到AEM中。

**ui.frontend**&#x200B;模块将在有关客户端库和前端开发的章节中详细介绍。 现在，我们来看一下如何将它集成到项目中。

1. 从命令行。 导航到`ui.frontend`文件夹（位于`aem-guides-wknd`下）:

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

   请注意以下行：`copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`。 这表示编译的CSS和JS正被复制到`ui.apps`文件夹中。

1. 视图文件`aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`的修改时间戳。 它应该在最近更新，然后更新`ui.apps`模块中的其他文件。

   与我们查看的其他模块不同，**ui.frontend**&#x200B;模块不直接部署到AEM。 而是将CSS和JS复制到&#x200B;**ui.apps**&#x200B;模块中，然后将&#x200B;**ui.apps**&#x200B;模块部署到AEM。 如果从第一个Maven命令查看构建顺序，您将看到&#x200B;**ui.frontend**&#x200B;始终在&#x200B;***ui.apps**之前构建。*

   在本教程的稍后部分，我们将介绍&#x200B;**ui.frontend**&#x200B;模块和嵌入式webpack开发服务器的高级功能，以实现快速开发。

## 包含核心组件{#core-components}

原型自动嵌入项目中的[AEM核心组件](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)。 以前，在检查部署到AEM的包时，会包括多个与核心组件相关的包。 核心组件是一套旨在加快AEM Sites项目开发的基础组件。 核心组件是开放源代码，可在[GitHub](https://github.com/adobe/aem-core-wcm-components)上使用。 有关如何将核心组件[包含在项目中的更多信息，请参阅](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components)。

1. 使用您最喜爱的文本编辑器打开`aem-guides-wknd/pom.xml`。

1. 搜索 `core.wcm.components.version`. 这将向您显示包含的核心组件版本：

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > AEM Project Archetype将包含AEM核心组件的某个版本，但这些项目具有不同的发布周期，因此包含的核心组件版本可能不是最新版本。 作为最佳实践，您应始终寻求利用最新版核心组件。 新功能和错误修复会频繁更新。 在GitHub](https://github.com/adobe/aem-core-wcm-components/releases)上可以找到最新的[发行信息。

1. 如果向下滚动到`dependencies`部分，您应看到单个核心组件依赖关系：

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

## 源控制管理{#source-control}

使用某种形式的源代码控件管理应用程序中的代码总是一个好主意。 本教程使用git和GitHub。 有几个文件由Maven和／或所选IDE生成，SCM应忽略这些文件。

Maven将在您构建和安装代码包时创建目标文件夹。 目标文件夹和内容应从SCM中排除。

在ui.apps下，您还会注意到许多已创建的。content.xml文件。 这些XML文件映射JCR中安装的内容的节点类型和属性。 这些文件是关键文件，应&#x200B;**不应**&#x200B;被忽略。

AEM项目原型将生成一个示例`.gitignore`文件，该文件可用作可安全忽略文件的起点。 文件在`<src>/aem-guides-wknd/.gitignore`处生成。

## 审核 {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## 恭喜！{#congratulations}

祝贺您，您刚刚创建了您的第一个AEM项目！

### 后续步骤{#next-steps}

通过[组件基础知识](component-basics.md)教程的简单`HelloWorld`示例，了解Adobe Experience Manager(AEM)站点组件的底层技术。
