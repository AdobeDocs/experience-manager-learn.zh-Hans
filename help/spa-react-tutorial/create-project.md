---
title: SPA编辑器项目 | AEM SPA编辑器和React入门
description: 了解如何将Adobe Experience Manager(AEM)Maven项目用作与AEM SPA Editor集成的React应用程序的起点。
sub-product: 站点
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 413
thumbnail: 413-spa-react.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '1128'
ht-degree: 2%

---


# SPA编辑器项目 {#spa-editor-project}

了解如何将Adobe Experience Manager(AEM)Maven项目用作与AEM SPA Editor集成的React应用程序的起点。

## 目标

1. 了解从Maven原型构建的新AEM SPA Editor项目的结构。
2. 将启动项目部署到AEM的本地实例。

## 您将构建的内容

在本章中，将根据AEM项目原型部署一 [个新的AEM项目](https://github.com/adobe/aem-project-archetype)。 AEM项目将通过React SPA的简单起点启动。 本章中使用的项目将作为执行WKND SPA的基础，并将在今后各章中加以建立。

![WKND SPA React Starter Project](./assets/create-project/wknd-spa-react.png)

*开始WKND SPA的站点层次结构。*

## 前提条件

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。 确保以作者模式启动的Adobe Experience Manager的 **新实例** 在本地运行。

## 获取项目

有几个选项可用于为AEM创建Maven多模块项目。 本教程使用最 [新的AEM Project](https://github.com/adobe/aem-project-archetype) Archetype作为教程代码的基础。 为了支持AEM的多个版本，对项目代码进行了修改。 请查看 [有关向后兼容性的说明](overview.md#compatibility)。

>[!CAUTION]
>
> 使用最新版本的原型 **是** ，最 [佳实践](https://github.com/adobe/aem-project-archetype) 是，为实际实施生成一个新项目。 AEM项目应使用archetype的属性目标AEM `aemVersion` 的单个版本。

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/create-project-start
   ```

2. 以下文件夹和文件结构表示由本地文件系统上的Maven原型生成的AEM Project:

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. 从AEM项目原型生成AEM项目时使用 [了以下属性](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | 属性 | 值 |
   |-----------------|-------------------------------------|
   | aemVersion | 云 |
   | appTitle | WKND SPA反应 |
   | appId | wknd-spa-react |
   | groupId | com.adobe.aem.guides |
   | frontendModule | 反应 |
   | 包装 | com.adobe.aem.guides.wknd.spa.react |
   | includeExamples | n |

   >[!NOTE]
   >
   > 注意属 `frontendModule=react` 性。 这告诉AEM Project Archetype使用启动程序代码库引导 [项目](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) ，以与AEM SPA Editor一起使用。

## 构建项目

然后，使用Maven编译、构建项目代码并将其部署到AEM的本地实例。

1. 确保AEM实例在端口4502上本地 **运行**。
2. 从命令行终端验证Maven是否已安装：

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 从目录运行以下Maven命 `aem-guides-wknd-spa` 令以构建项目并将其部署到AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   项目的多个模块应进行编译并部署到AEM。

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-react 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-react ..................................... SUCCESS [  0.523 s]
    [INFO] WKND SPA React - Core .............................. SUCCESS [  8.069 s]
    [INFO] wknd-spa-react.ui.frontend - UI Frontend ........... SUCCESS [01:23 min]
    [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  0.830 s]
    [INFO] WKND SPA React - UI apps ........................... SUCCESS [  4.654 s]
    [INFO] WKND SPA React - UI content ........................ SUCCESS [  1.607 s]
    [INFO] WKND SPA React - All ............................... SUCCESS [  0.384 s]
    [INFO] WKND SPA React - Integration Tests Bundles ......... SUCCESS [  0.770 s]
    [INFO] WKND SPA React - Integration Tests Launcher ........ SUCCESS [  1.407 s]
    [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.055 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  01:44 min
   ```

   Maven用户档案 ***autoInstallSinglePackage*** （自动安装单个包）编译项目的各个模块并将单个包部署到AEM实例。 默认情况下，此包将部署到AEM实例，该实例在端口4502 **上本地运** 行，凭 **据为admin:admin**。

4. 导航到 **[!UICONTROL 本地AEM]** 实例上的包管理器： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

5. 您应该看到三个 `wknd-spa-react.all`包， `wknd-spa-react.ui.apps` 和 `wknd-spa-react.ui.content`。

   ![WKND SPA包](./assets/create-project/package-manager.png)

   *AEM包管理器*

   项目所需的所有自定义代码都将打包到这些包中并安装在AEM运行时上。

6. 您还应看到和的几个 `spa.project.core` 包 `core.wcm.components`。 这些依赖项会自动包含在原型中。 有关AEM核心 [组件的更多信息，请访问](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)。

   `spa.project.core` 是生成SPA编辑器所需的JSON模型API所需的依赖项。

## 创作内容

接下来，打开由原型生成的起始SPA并更新一些内容。

1. 导航到站 **[!UICONTROL 点]** 控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND SPA包括一个基本的场地结构，包括国家、语言和主页。 此层次结构基于原型的默认值 `language_country` 和 `isSingleCountryWebsite`。 在生成项目时更新可用 [属性](https://github.com/adobe/aem-project-archetype#available-properties) ，可以覆盖这些值。

2. 选择页 **[!DNL us]** 面并单 **[!DNL en]** 击菜 **[!DNL WKND SPA React Home Page]** 单栏 **[!UICONTROL 中的]** “编辑”按钮，打开> >页：

   ![站点控制台](./assets/create-project/open-home-page.png)

3. 文 **[!UICONTROL 本组]** 件已添加到页面。 您可以像AEM中的任何其他组件一样编辑此组件。

   ![更新文本组件](./assets/create-project/update-text-component.gif)

4. 向页面添 **[!UICONTROL 加其]** 他文本组件。

   请注意，创作体验与传统的AEM Sites页面类似。 目前，可以使用的组件数量有限。 在本教程中将添加更多内容。

## Inspect单页应用程序

然后，使用浏览器的开发人员工具验证这是单页应用程序。

1. 在页面 **[!UICONTROL 编辑器中]**，单击“页面 **[!UICONTROL 信息”按钮]** >“ **[!UICONTROL 视图为已发布”]**:

   ![视图为已发布按钮](./assets/create-project/view-as-published.png)

   这将打开一个带有查询参数的新选项卡，该 `?wcmmode=disabled` 选项卡会有效地关闭AEM编辑器： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 视图页面的源，并注意未找到文 **[!DNL Hello World]** 本内容或任何其他内容。 相反，您应看到HTML，如下所示：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 是加载到页面上并负责呈现内容的React SPA。

   但是， *内容从何而来？*

3. 返回选项卡： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 打开浏览器的开发人员工具，并在刷新过程中检查页面的网络流量。 视图 **XHR** 请求：

   ![XHR请求](./assets/create-project/xhr-requests.png)

   应向http://localhost:4502/content/wknd-spa-react/us/en.model.json提 [出](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 它包含所有将驱动SPA的内容（格式为JSON）。

5. 在新选项卡中，打开 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   请求表 `en.model.json` 示将驱动应用程序的内容模型。 InspectJSON输出，您应该能够找到表示文 **[!UICONTROL 本]** 组件的片段。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   在下一章中，我们将检查如何将此JSON内容从AEM组件映射到SPA组件，以形成AEM SPA编辑器体验的基础。

   >[!NOTE]
   >
   > 安装浏览器扩展以自动设置JSON输出格式可能会很有帮助。

## 恭喜！ {#congratulations}

祝贺您，您刚刚创建了您的第一个AEM SPA编辑器项目！

SPA很简单。 在接下来的几章中将添加更多功能。

### 后续步骤 {#next-steps}

[集成SPA](integrate-spa.md) —— 了解SPA源代码如何与AEM项目集成，并了解快速开发SPA的可用工具。
