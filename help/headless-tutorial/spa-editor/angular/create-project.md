---
title: SPA编辑器项目 | AEM SPA Editor和Angular快速入门
description: 了解如何将Adobe Experience Manager(AEM)Maven项目用作与AEM SPA编辑器集成的Angular应用程序的起点。
sub-product: sites
feature: SPA Editor, AEM Project Archetype
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 3%

---

# SPA编辑器项目 {#create-project}

了解如何将Adobe Experience Manager(AEM)Maven项目用作与AEM SPA编辑器集成的Angular应用程序的起点。

## 目标

1. 了解基于Maven原型构建的新AEM SPA Editor项目的结构。
2. 将起始项目部署到AEM的本地实例。

## 将构建的内容

在本章中，将根据 [AEM项目原型](https://github.com/adobe/aem-project-archetype). AEM项目正在启动，其起点非常简单，只需AngularSPA。 本章中使用的项目将用作实施WKND SPA的基础，并在将来的章节中构建。

![WKND SPAAngular入门项目](./assets/create-project/what-you-will-build.png)

*经典的“你好世界”消息。*

## 前提条件

查看设置 [本地开发环境](overview.md#local-dev-environment). 确保以 **作者** 模式。

## 获取项目

有多个选项可用于为AEM创建Maven多模块项目。 本教程使用最新 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 作为教程代码的基础。 为了支持多个版本的AEM，已对项目代码进行了修改。 请查阅 [关于向后兼容性的说明](overview.md#compatibility).

>[!CAUTION]
>
>最佳做法是使用 **最新** 版本 [原型](https://github.com/adobe/aem-project-archetype) 为实际实施生成新项目。 AEM项目应使用 `aemVersion` 原型的属性。

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 以下文件夹和文件结构表示由本地文件系统上的Maven原型生成的AEM项目：

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

3. 从生成AEM项目时，使用以下属性 [AEM项目原型](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | 属性 | 值 |
   |-----------------|---------------------------------------|
   | aemVersion | 云 |
   | appTitle | WKND SPAAngular |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | 软件包 | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 请注意 `frontendModule=angular` 属性。 这可告知AEM项目原型使用启动程序引导项目 [Angular代码库](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) 与AEM SPA Editor一起使用时，不会将其标记为“隐藏”。

## 构建项目

接下来，使用Maven编译、构建项目代码并将其部署到AEM的本地实例。

1. 确保AEM实例在端口上本地运行 **4502**.
2. 从命令行终端验证Maven是否已安装：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 从 `aem-guides-wknd-spa` 要构建项目并将其部署到AEM的目录：

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   如果使用 [AEM 6.x](overview.md#compatibility):

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

   Maven个人资料 ***autoInstallSinglePackage*** 编译项目的各个模块，并将单个包部署到AEM实例。 默认情况下，此包将部署到端口上本地运行的AEM实例 **4502** 并拥有 **管理员：管理员**.

4. 导航到 **[!UICONTROL 包管理器]** 在本地AEM实例上： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. 您应会看到三个 `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` 和 `wknd-spa-angular.ui.content`.

   ![WKND SPA包](./assets/create-project/package-manager.png)

   项目所需的所有自定义代码都捆绑到这些包中，并安装在AEM运行时上。

6. 您还应会看到 `spa.project.core` 和 `core.wcm.components`. 这些是原型自动包含的依赖项。 有关 [AEM Core Components可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans).

## 创作内容

接下来，打开由原型生成的起始SPA，并更新一些内容。

1. 导航到 **[!UICONTROL 站点]** 控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA包括基本站点结构，其中包含国家/地区、语言和主页。 此层次结构基于的 `language_country` 和 `isSingleCountryWebsite`. 这些值可通过更新 [可用属性](https://github.com/adobe/aem-project-archetype#available-properties) 生成项目时。

2. 打开 **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** 页面，方法是选择页面并单击 **[!UICONTROL 编辑]** 按钮：

   ![站点控制台](./assets/create-project/open-home-page.png)

3. A **[!UICONTROL 文本]** 组件已添加到页面。 您可以像在AEM中编辑任何其他组件一样编辑此组件。

   ![更新文本组件](./assets/create-project/update-text-component.gif)

4. 添加其他 **[!UICONTROL 文本]** 组件。

   请注意，创作体验与传统AEM Sites页面的体验类似。 目前可使用的组件数量有限。 在教程中添加了更多内容。

## Inspect单页应用程序

接下来，使用浏览器的开发人员工具验证这是单页应用程序。

1. 在 **[!UICONTROL 页面编辑器]**，请单击 **[!UICONTROL 页面信息]** 菜单> **[!UICONTROL 查看已发布的项目]**:

   ![“查看已发布项”按钮](./assets/create-project/view-as-published.png)

   这将打开一个包含查询参数的新选项卡 `?wcmmode=disabled` 这会有效地关闭AEM编辑器： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 查看页面的源并注意文本内容 **[!DNL Hello World]** 或找不到任何其他内容。 您而应会看到HTML，如下所示：

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

   `clientlib-angular.min.js` 是加载到页面上并负责呈现内容的AngularSPA。

   *内容来自何处？*

3. 返回到选项卡： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 打开浏览器的开发人员工具，并在刷新期间检查页面的网络流量。 查看 **XHR** 请求：

   ![XHR请求](./assets/create-project/xhr-requests.png)

   应请求 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 其中包含将驱动SPA的所有内容（格式为JSON）。

5. 在新选项卡中，打开 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   请求 `en.model.json` 表示将驱动应用程序的内容模型。 Inspect输出，您应该能够找到表示 **[!UICONTROL 文本]** 组件。

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

   在下一章中，我们将检查如何将JSON内容从AEM组件映射到SPA组件，以构建AEM SPA编辑器体验的基础。

   >[!NOTE]
   >
   > 安装浏览器扩展以自动设置JSON输出格式可能会有所帮助。

## 恭喜！ {#congratulations}

恭喜，您刚刚创建了第一个AEM SPA Editor项目！

现在非常简单，但在接下来的几章中，添加了更多功能。

### 后续步骤 {#next-steps}

[集成SPA](integrate-spa.md)  — 了解SPA源代码如何与AEM项目集成，并了解可用于快速开发SPA的工具。
