---
title: 客户端应用程序集成 — AEM无头 — GraphQL的高级概念
description: 实施持久化查询并将其集成到WKND应用程序中。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# 客户端应用程序集成

在上一章中，您使用GraphiQL资源管理器创建并更新了持久化查询。

本章将指导您完成使用现有HTTPGET请求将持久查询与WKND客户端应用程序（又称WKND应用程序）集成的步骤 **React组件**. 它还为应用AEM Headless学习知识、编码专业知识以增强WKND客户端应用程序提供了可选挑战。

## 前提条件 {#prerequisites}

本文档是多部分教程的一部分。 在继续处理本章之前，请确保已完成前几章。 WKND客户端应用程序连接到AEM发布服务，因此务必要 **将以下内容发布到AEM发布服务**.

* 项目配置
* GraphQL 端点
* 内容片段模型
* 创作内容片段
* GraphQL持久查询

的 _本章中的IDE屏幕截图来自 [Visual Studio代码](https://code.visualstudio.com/)_

### 第1-4章解决方案包（可选） {#solution-package}

可以安装一个解决方案包，以完成AEM UI中章节1-4中的步骤。 此包是 **不需要** 如果前几章已经完成。

1. 下载 [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. 在AEM中，导航到 **工具** > **部署** > **包** 访问 **包管理器**.
1. 上载并安装在上一步中下载的包（zip文件）。
1. 将包复制到AEM发布服务

## 目标 {#objectives}

在本教程中，您将了解如何使用 [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js).

## 克隆并运行示例客户端应用程序 {#clone-client-app}

为了加快教程的进度，我们提供了一个React JS入门应用程序。

1. 克隆 [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 编辑 `aem-guides-wknd-graphql/advanced-tutorial/.env.development` 文件和设置 `REACT_APP_HOST_URI` 以指向target AEM发布服务。

   如果连接到创作实例，请更新身份验证方法。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![React应用程序开发环境](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > 上述说明是将React应用程序连接到 **AEM发布服务**，但要连接到 **AEM创作服务** 获取target AEMas a Cloud Service环境的本地开发令牌。
   >
   > 也可以将应用程序连接到 [使用AEMaaCS SDK的本地创作实例](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 使用基本身份验证。


1. 打开终端并运行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. 应加载新的浏览器窗口 [http://localhost:3000](http://localhost:3000)


1. 点按 **露营** > **约塞米蒂背包** 查看Yosemite Backpacking探险详情。

   ![Yosemite背包屏幕](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 打开浏览器的开发人员工具并检查 `XHR` 请求

   ![POSTGraphQL](assets/client-application-integration/graphql-persisted-query.png)

   您应会看到 `GET` 具有项目配置名称(`wknd-shared`)，保留的查询名称(`adventure-by-slug`)，变量名称(`slug`)，值(`yosemite-backpacking`)和特殊字符编码。

>[!IMPORTANT]
>
>    如果您想知道为什么针对 `http://localhost:3000` 和NOT针对AEM发布服务域，请查看 [在引擎罩下](../multi-step/graphql-and-react-app.md#under-the-hood) 从基本教程。


## 查看代码

在 [基本教程 — 构建使用AEM GraphQL API的React应用程序](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) 我们已审查并改进了几个关键文件，以获得实际操作的专业知识。 在增强WKND应用程序之前，请查看关键文件。

* [查看AEMHeadless对象](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [实施以运行AEM GraphQL持久查询](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### 审阅 `Adventures` React组件

WKND React应用程序的主视图是所有历险的列表，您可以根据诸如之类的活动类型过滤这些历险 _野营、骑自行车_. 此视图由 `Adventures` 组件。 以下是主要实施详细信息：

* 的 `src/components/Adventures.js` 调用 `useAllAdventures(adventureActivity)` 钩子 `adventureActivity` 参数是活动类型。

* 的 `useAllAdventures(adventureActivity)` 挂钩在 `src/api/usePersistedQueries.js` 文件。 基于 `adventureActivity` 值时，它会确定要调用的持久查询。 如果不为null值，则会调用 `wknd-shared/adventures-by-activity`，否则将获得所有可用的冒险 `wknd-shared/adventures-all`.

* 钩子使用主 `fetchPersistedQuery(..)` 将查询执行委派到的函数 `AEMHeadless` 通过 `aemHeadlessClient.js`.

* 此挂接还仅返回AEM GraphQL响应(位于 `response.data?.adventureList?.items`，允许 `Adventures` React视图组件与父JSON结构无关。

* 成功执行查询后， `AdventureListItem(..)` 从渲染函数 `Adventures.js` 添加HTML元素以显示 _图像、行程时长、价格和标题_ 信息。

### 审阅 `AdventureDetail` React组件

的 `AdventureDetail` React组件可渲染探险的详细信息。 以下是主要实施详细信息：

* 的 `src/components/AdventureDetail.js` 调用 `useAdventureBySlug(slug)` 钩子 `slug` 参数是查询参数。

* 如上所示， `useAdventureBySlug(slug)` 挂钩在 `src/api/usePersistedQueries.js` 文件。 它调用 `wknd-shared/adventure-by-slug` 将委派到 `AEMHeadless` 通过 `aemHeadlessClient.js`.

* 成功执行查询后， `AdventureDetailRender(..)` 从渲染函数 `AdventureDetail.js` 添加HTML元素以显示“冒险”详细信息。


## 增强代码

### 使用 `adventure-details-by-slug` 持久化查询

在上一章中，我们创建了 `adventure-details-by-slug` 持久查询，它提供其他冒险信息，例如 _位置、instructorTeam和管理员_. 让我们替换 `adventure-by-slug` with `adventure-details-by-slug` 保留查询以呈现此附加信息。

1. 打开 `src/api/usePersistedQueries.js`.

1. 找到函数 `useAdventureBySlug()` 将查询更新为

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### 显示其他信息

1. 要显示其他冒险信息，请打开 `src/components/AdventureDetail.js`

1. 找到函数 `AdventureDetailRender(..)` 将返回函数更新为

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. 还定义相应的渲染函数：

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **位置**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstructorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **管理员**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### 定义新样式

1. 打开 `src/components/AdventureDetail.scss` 添加以下类定义

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>更新的文件位于 **AEM指南WKND - GraphQL** 项目，请参阅 [高级教程](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) 中。


完成上述增强功能后，WKND应用程序如下所示，浏览器的开发人员工具显示 `adventure-details-by-slug` 持久查询调用。

![增强的WKND应用程序](assets/client-application-integration/Enhanced-WKND-APP.gif)

## 增强功能挑战（可选）

WKND React应用程序的主视图允许您根据诸如之类的活动类型过滤这些冒险 _野营、骑自行车_. 但WKND业务团队希望 _位置_ 基于过滤功能。 要求包括

* 在WKND应用程序的主视图中，左上角或右上角添加 _位置_ 过滤图标。
* 单击 _位置_ 过滤图标应显示位置列表。
* 从列表中单击所需位置选项应仅显示匹配的冒险。
* 如果只有一个匹配的“冒险”，则会显示“冒险详细信息”视图。

## 恭喜

恭喜！现在，您已完成将持久查询集成并实施到示例WKND应用程序中。
