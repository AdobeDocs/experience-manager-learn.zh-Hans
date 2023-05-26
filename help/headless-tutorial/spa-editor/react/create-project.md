---
title: 创建项目 | AEM SPA Editor和React快速入门
description: 了解如何生成一个Adobe Experience Manager (AEM) Maven项目，作为与AEM SPA编辑器集成的React应用程序的起点。
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: c489a033f34aecaa0af10e3868c258feba6aaae6
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 2%

---

# 创建项目 {#spa-editor-project}

了解如何生成一个Adobe Experience Manager (AEM) Maven项目，作为与AEM SPA编辑器集成的React应用程序的起点。

## 目标

1. 使用SPA项目原型生成启用AEM编辑器的项目。
2. 将起始项目部署到AEM的本地实例。

## 您将构建的内容 {#what-build}

AEM在本章中，将根据 [AEM项目原型](https://github.com/adobe/aem-project-archetype). AEM项目通过React SPA的一个非常简单的起点引导。

**什么是Maven项目？** - [Apache Maven](https://maven.apache.org/) 是用于构建项目的软件管理工具。 *所有Adobe Experience Manager* 实施使用Maven项目在AEM之上生成、管理和部署自定义代码。

**什么是Maven原型？** - A [Maven原型](https://maven.apache.org/archetype/index.html) 是用于生成新项目的模板或模式。 AEM项目原型允许我们生成一个具有自定义命名空间的新项目，并包含一个遵循最佳实践的项目结构，从而大大加快了我们的项目。

## 前提条件

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment). 确保已在中启动新的Adobe Experience Manager实例 **作者** 模式，正在本地运行。

## 创建项目 {#create}

>[!NOTE]
>
>本教程使用版本 **35** 原型的。

1. 打开命令行终端并输入以下Maven命令：

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=35 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 如果定位AEM 6.5.5+版本，则替换 `aemVersion="cloud"` 替换为 `aemVersion="6.5.5"`. 如果定位到6.4.8+，请使用 `aemVersion="6.4.8"`.

   请注意 `frontendModule=react` 属性。 这会告知AEM项目原型使用启动程序引导项目 [React代码库](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) 与AEM SPA编辑器一起使用。 类似属性 `appTitle`， `appId`， `artifactId`、和 `groupId` 用于标识项目和目的。

   用于配置项目的可用属性的完整列表 [可在此处找到](https://github.com/adobe/aem-project-archetype#available-properties).

1. 以下文件夹和文件结构由本地文件系统上的Maven原型生成：

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   每个文件夹表示一个单独的Maven模块。 在本教程中，我们将主要使用 `ui.frontend` 模块，即React应用程序。 有关各个模块的更多详细信息，请参阅 [AEM项目原型文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## 部署和生成项目

接下来，使用Maven编译、生成项目代码并将其部署到AEM的本地实例。

1. 确保AEM的实例在端口上本地运行 **4502**.
1. 从命令行导航到 `aem-guides-wknd-spa.react` 项目目录。

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 运行以下命令以生成整个项目并将其部署到AEM：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   构建过程将需要大约一分钟，并且应该以以下消息结束：

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven配置文件 `autoInstallSinglePackage` 编译项目的各个模块，并将单个包部署到AEM实例。 默认情况下，此包将部署到本地在端口上运行的AEM实例 **4502** 并且拥有 `admin:admin`.

1. 导航到 **包管理器** 在本地AEM实例上： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. 您应会看到多个以为前缀的包 `aem-guides-wknd-spa.react`.

   ![WKND SPA包](assets/create-project/package-manager.png)

   *AEM包管理器*

   项目所需的所有自定义代码都捆绑在这些包中，并安装在AEM环境中。

## 创作内容

接下来，打开原型生成的入门SPA并更新某些内容。

1. 导航到 **站点** 控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA包括基本站点结构，其中包含国家/地区、语言和主页。 此层次结构基于原型的默认值 `language_country` 和 `isSingleCountryWebsite`. 可以通过更新 [可用属性](https://github.com/adobe/aem-project-archetype#available-properties) 生成项目时。

2. 打开 **us** > **en** > **WKND SPA React主页** ，然后单击 **编辑** 菜单栏中的按钮：

   ![站点控制台](./assets/create-project/open-home-page.png)

3. A **文本** 组件已添加到页面中。 您可以像AEM中的任何其他组件一样编辑此组件。

   ![更新文本组件](./assets/create-project/update-text-component.gif)

4. 添加其他 **文本** 组件添加到页面。

   请注意，创作体验类似于传统AEM Sites页面的创作体验。 当前可用的组件数量有限。 在本教程中会添加更多内容。

## Inspect单页应用程序

接下来，使用浏览器的开发人员工具验证这是一个单页应用程序。

1. 在 **页面编辑器**，单击 **页面信息** 按钮> **查看已发布的项目**：

   ![“以发布的形式查看”按钮](./assets/create-project/view-as-published.png)

   这将使用查询参数打开一个新选项卡 `?wcmmode=disabled` 可以有效地关闭AEM编辑器： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 查看页面的源，并注意文本内容 **[!DNL Hello World]** 或找不到任何其他内容。 您应该会看到如下所示的HTML：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 是加载到页面上的React SPA，负责呈现内容。

   但是， *内容来自何处？*

3. 返回到选项卡： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 打开浏览器的开发人员工具，并在刷新期间检查页面的网络流量。 查看 **XHR** 请求：

   ![XHR请求](./assets/create-project/xhr-requests.png)

   应该有一个请求 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 该文档包含所有将驱动SPA的内容（格式为JSON）。

5. 在新标签页中打开 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   请求 `en.model.json` 表示将驱动应用程序的内容模型。 Inspect JSON输出，您应该能够找到表示 **[!UICONTROL 文本]** 组件。

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

   在下一章中，我们将检查此JSON内容如何从AEM组件映射到SPA组件，从而形成AEM SPA编辑器体验的基础。

   >[!NOTE]
   >
   > 安装浏览器扩展以自动格式化JSON输出可能很有帮助。

## 恭喜！ {#congratulations}

恭喜，您刚刚创建了第一个AEM SPA Editor项目！

SPA很简单。 在接下来的几个章节中，添加了更多功能。

### 后续步骤 {#next-steps}

[集成SPA](integrate-spa.md)  — 了解SPA源代码如何与AEM项目集成，并了解可用于快速开发SPA的工具。
