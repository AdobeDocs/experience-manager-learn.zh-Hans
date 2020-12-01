---
title: SPA Editor项目 | AEM SPA编辑器和Angular入门
description: 了解如何将Adobe Experience Manager(AEM)Maven项目用作与SPA Editor集成的Angular应用程序的起点。
sub-product: 站点
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 2%

---


# SPA Editor项目{#create-project}

了解如何将Adobe Experience Manager(AEM)Maven项目用作与SPA Editor集成的Angular应用程序的起点。

## 目标

1. 了解从Maven原型构建的新的AEM SPA Editor项目的结构。
2. 将启动项目部署到AEM的本地实例。

## 您将构建的内容

在本章中，将根据[AEM项目Archetype](https://github.com/adobe/aem-project-archetype)部署新的AEM项目。 AEM项目将由角度SPA的一个非常简单的起点启动。 本章中使用的项目将作为实施WKND SPA的基础，并将在今后各章中加以建立。

![WKND SPA角启动项目](./assets/create-project/what-you-will-build.png)

*经典的Hello World消息。*

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。 确保以&#x200B;**author**&#x200B;模式启动的Adobe Experience Manager的新实例在本地运行。

## 获取项目

有几个选项可用于为AEM创建Maven多模块项目。 本教程使用最新的[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)作为教程代码的基础。 为了支持AEM的多个版本，对项目代码进行了修改。 请查看[关于向后兼容性的说明](overview.md#compatibility)。

>[!CAUTION]
>
>最好使用[archetype](https://github.com/adobe/aem-project-archetype)的&#x200B;**latest**&#x200B;版本生成用于实际实施的新项目。 AEM项目应使用原型的`aemVersion`属性目标AEM的单个版本。

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
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

3. 从[AEM项目原型](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14)生成AEM项目时使用了以下属性：

   | 属性 | 值 |
   |-----------------|---------------------------------------|
   | aemVersion | 云 |
   | appTitle | WKND SPA角度 |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | 角 |
   | 包装 | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 注意`frontendModule=angular`属性。 这告诉AEM Project Archetype使用启动程序[角度代码库](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)引导项目，以便与SPA Editor一起使用。

## 构建项目

然后，使用Maven编译、构建项目代码并将其部署到AEM的本地实例。

1. 确保AEM实例在端口&#x200B;**4502**&#x200B;上本地运行。
2. 从命令行终端验证Maven是否已安装：

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 从`aem-guides-wknd-spa`目录运行以下Maven命令以构建项目并将其部署到AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   如果使用[AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   项目的多个模块应进行编译并部署到AEM。

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
    [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
    [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
    [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
    [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
    [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
    [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
    [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
    [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
    [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
   ```

   Maven用户档案&#x200B;***autoInstallSinglePackage***&#x200B;编译项目的各个模块并将单个包部署到AEM实例。 默认情况下，此包将部署到在端口&#x200B;**4502**&#x200B;上本地运行的AEM实例，凭据为&#x200B;**admin:admin**。

4. 导航到本地AEM实例上的&#x200B;**[!UICONTROL 包管理器]**:[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

5. 您应当看到`wknd-spa-angular.all`、`wknd-spa-angular.ui.apps`和`wknd-spa-angular.ui.content`的三个软件包。

   ![WKND SPA包](./assets/create-project/package-manager.png)

   项目所需的所有自定义代码都将打包到这些包中并安装在AEM运行时上。

6. 您还应当看到`spa.project.core`和`core.wcm.components`的几个包。 它们是原型自动包含的依赖项。 有关[AEM核心组件的更多信息，请访问](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)。

## 创作内容

接下来，打开原型生成的启动SPA并更新一些内容。

1. 导航到&#x200B;**[!UICONTROL 站点]**&#x200B;控制台：[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND SPA包括一个基本的站点结构，包括国家、语言和主页。 此层次结构基于`language_country`和`isSingleCountryWebsite`的原型默认值。 在生成项目时更新[可用属性](https://github.com/adobe/aem-project-archetype#available-properties)可覆盖这些值。

2. 选择&#x200B;**[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]**&#x200B;页面，然后单击菜单栏中的&#x200B;**[!UICONTROL 编辑]**&#x200B;按钮：

   ![站点控制台](./assets/create-project/open-home-page.png)

3. **[!UICONTROL 文本]**&#x200B;组件已添加到页面。 您可以像AEM中的任何其他组件一样编辑此组件。

   ![更新文本组件](./assets/create-project/update-text-component.gif)

4. 向页面添加额外的&#x200B;**[!UICONTROL Text]**&#x200B;组件。

   请注意，创作体验与传统的AEM Sites页面类似。 目前，可以使用的组件数量有限。 在本教程中将添加更多内容。

## Inspect单页应用程序

然后，使用浏览器的开发人员工具验证这是单页应用程序。

1. 在&#x200B;**[!UICONTROL 页面编辑器]**&#x200B;中，单击&#x200B;**[!UICONTROL 页面信息]**&#x200B;菜单> **[!UICONTROL 视图作为已发布]**:

   ![视图为已发布按钮](./assets/create-project/view-as-published.png)

   这将打开一个查询参数`?wcmmode=disabled`的新选项卡，该选项卡会有效地关闭AEM编辑器：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 视图页面的源，并注意未找到文本内容&#x200B;**[!DNL Hello World]**&#x200B;或任何其他内容。 相反，您应看到HTML，如下所示：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` 是加载到页面并负责呈现内容的角SPA。

   *内容从何而来？*

3. 返回选项卡：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 打开浏览器的开发人员工具，并在刷新过程中检查页面的网络流量。 视图&#x200B;**XHR**&#x200B;请求：

   ![XHR请求](./assets/create-project/xhr-requests.png)

   应该有对[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)的请求。 它包含所有将驱动SPA的内容（格式为JSON）。

5. 在新选项卡中，打开[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   请求`en.model.json`表示将驱动应用程序的内容模型。 InspectJSON输出，您应该能够找到代表&#x200B;**[!UICONTROL Text]**&#x200B;组件的片段。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   在下一章中，我们将检查JSON内容如何从AEM组件映射到SPA组件，以形成SPA AEM编辑器体验的基础。

   >[!NOTE]
   >
   > 安装浏览器扩展以自动设置JSON输出格式可能会很有帮助。

## 恭喜！{#congratulations}

祝贺您，您刚刚创建了您的第一个AEM SPA编辑项目！

它现在非常简单，但在接下来的几章中将添加更多功能。

### 后续步骤{#next-steps}

[集成SPA](integrate-spa.md)  —— 了解SPA源代码如何与AEM项目集成，并了解快速开发SPA的可用工具。
