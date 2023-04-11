---
title: 构建一个React应用程序，该应用程序使用GraphQL API查询AEM -AEM Headless快速入门 — GraphQL
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 构建从AEM GraphQL API获取内容/数据的React应用程序，另请参阅如何使用AEM Headless JS SDK。
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1182'
ht-degree: 2%

---


# 构建使用AEM GraphQL API的React应用程序

在本章中，您将了解AEM GraphQL API如何在外部应用程序中提供体验。

简单的React应用程序用于查询和显示 **团队** 和 **人员** 由AEM GraphQL API公开的内容。 React的使用基本上不重要，任何平台的任何框架都可以编写消耗性外部应用程序。

## 前提条件

我们假定已完成本多部分教程前几部分中概述的步骤，或者 [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) 安装在AEMas a Cloud Service创作和发布服务上。

_本章中的IDE屏幕截图来自 [Visual Studio代码](https://code.visualstudio.com/)_

必须安装以下软件：

- [Node.js v18](https://nodejs.org/)
- [Visual Studio代码](https://code.visualstudio.com/)

## 目标

了解如何：

- 下载并启动示例React应用程序
- 使用查询AEM GraphQL端点 [AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)
- 查询AEM以获取团队及其引用的成员列表
- 查询AEM成员的详细信息

## 获取示例React应用程序

在本章中，实施了一个简化的React示例应用程序，该应用程序使用与AEM GraphQL API交互所需的代码，以及从这些应用程序获取的显示团队和人员数据。

Github.com上提供了示例React应用程序源代码，网址为 <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

要获取React应用程序，请执行以下操作：

1. 从中克隆示例WKND GraphQL React应用程序 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 导航到 `basic-tutorial` 文件夹，并在IDE中打开它。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![VSCode中的React App](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新 `.env.development` 连接到AEMas a Cloud Service发布服务。

   - 已设置 `REACT_APP_HOST_URI`值作为AEMas a Cloud Service的发布URL(例如 `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`)和 `REACT_APP_AUTH_METHOD`&#39;s值到 `none`
   >[!NOTE]
   >
   > 确保您已发布了前面步骤中的项目配置、内容片段模型、创作了内容片段、GraphQL端点和保留查询。
   >
   > 如果您对本地AEM创作SDK执行了上述步骤，则可以指向 `http://localhost:4502` 和 `REACT_APP_AUTH_METHOD`&#39;s值到 `basic`.


1. 从命令行中，转到 `aem-guides-wknd-graphql/basic-tutorial` 文件夹

1. 启动React应用程序

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React应用程序在 [http://localhost:3000/](http://localhost:3000/). 在整个教程中对React应用程序所做的更改会立即反映出来。

![部分实施的React应用程序](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   此React应用程序已部分实施。 按照本教程中的步骤完成实施。 需要实施的JavaScript文件具有以下注释，请确保使用本教程中指定的代码在这些文件中添加/更新代码。
>
>
> //*********************************
>
>  // TODO :按照AEM Headless教程中的步骤实施此步骤
>
>  //*********************************

## React应用程序剖析

示例React应用程序有三个主要部分：

1. 的 `src/api` 文件夹包含用于向AEM进行GraphQL查询的文件。
   - `src/api/aemHeadlessClient.js` 初始化并导出用于与AEM通信的AEM Headless Client
   - `src/api/usePersistedQueries.js` 实施 [自定义React挂钩](https://react.dev/docs/hooks-custom.html) 将数据从AEM GraphQL返回到 `Teams.js` 和 `Person.js` 查看组件。

1. 的 `src/components/Teams.js` 文件会使用列表查询显示团队及其成员的列表。
1. 的 `src/components/Person.js` 文件使用参数化的单结果查询来显示单个人员的详细信息。

## 查看AEMHeadless对象

查看 `aemHeadlessClient.js` 文件，以了解如何创建 `AEMHeadless` 用于与AEM通信的对象。

1. 打开 `src/api/aemHeadlessClient.js`.

1. 查看第1-40行：

   - 导入 `AEMHeadless` 来自 [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js)，第11行。

   - 基于 `.env.development`、行14-22和箭头函数表达式 `setAuthorization`31-40号线。

   - 的 `serviceUrl` 已包含的设置 [开发代理](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) 27号线。

1. 第42-49行在实例化 `AEMHeadless` 客户端，并将其导出以在整个React应用程序中使用。

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## 实施以运行AEM GraphQL持久查询

实施通用 `fetchPersistedQuery(..)` 运行AEM GraphQL的函数保留查询打开 `usePersistedQueries.js` 文件。 的 `fetchPersistedQuery(..)` 函数使用 `aemHeadlessClient` 对象 `runPersistedQuery()` 函数以异步方式运行基于承诺的行为。

稍后，自定义React `useEffect` 挂接调用此函数以从AEM中检索特定数据。

1. 在 `src/api/usePersistedQueries.js` **更新** `fetchPersistedQuery(..)`，第35行，并使用以下代码。

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## 实施团队功能

接下来，构建在React应用程序主视图上显示团队及其成员的功能。 此功能需要：

- 新 [自定义React useEffect挂接](https://react.dev/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` 调用 `my-project/all-teams` 保留查询，在AEM中返回团队内容片段列表。
- React组件位于 `src/components/Teams.js` 调用新自定义React的 `useEffect` 挂接并呈现团队数据。

完成后，应用程序的主视图中会填充来自AEM的团队数据。

![“团队”视图](./assets/graphql-and-external-app/react-app__teams-view.png)

### 步骤

1. 打开 `src/api/usePersistedQueries.js`.

1. 找到函数 `useAllTeams()`

1. 创建 `useEffect` 用于调用保留查询的挂接 `my-project/all-teams` 通过 `fetchPersistedQuery(..)`，添加以下代码。 此挂接还仅返回AEM GraphQL响应(位于 `data?.teamList?.items`，允许React视图组件与父JSON结构不相关。

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. 打开 `src/components/Teams.js`

1. 在 `Teams` React组件中，使用 `useAllTeams()` 挂。

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. 执行基于视图的数据验证，根据返回的数据显示错误消息或加载指示器。

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. 最后，渲染团队数据。 从GraphQL查询返回的每个团队都使用提供的 `Team` React子组件。

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## 实施人员功能

使用 [团队功能](#implement-teams-functionality) 完成后，我们将实施该功能以处理团队成员或人员详细信息上的显示。

此功能需要：

- 新 [自定义React useEffect挂接](https://react.dev/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` 调用参数化的 `my-project/person-by-name` 保留查询，并返回单个人员记录。

- React组件位于 `src/components/Person.js` 将人员的全名用作查询参数，调用新的自定义React `useEffect` 挂接并呈现人员数据。

完成后，在“团队”视图中选择人员姓名，即会呈现人员视图。

![人员](./assets/graphql-and-external-app/react-app__person-view.png)

1. 打开 `src/api/usePersistedQueries.js`.

1. 找到函数 `usePersonByName(fullName)`

1. 创建 `useEffect` 用于调用保留查询的挂接 `my-project/all-teams` 通过 `fetchPersistedQuery(..)`，添加以下代码。 此挂接还仅返回AEM GraphQL响应(位于 `data?.teamList?.items`，允许React视图组件与父JSON结构不相关。

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. 打开 `src/components/Person.js`
1. 在 `Person` React组件，解析 `fullName` 路由参数，并使用从AEM中获取人员数据 `usePersonByName(fullName)` 挂。

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. 执行基于视图的数据验证，根据返回的数据显示错误消息或加载指示器。

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. 最后，渲染人员数据。

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## 尝试应用程序

查看应用程序 [http://localhost:3000/](http://localhost:3000/) 单击 _成员_ 链接。 此外，您还可以通过在AEM中添加内容片段，向Team Alpha中添加更多团队和/或成员。

## 在引擎罩下

打开浏览器的 **开发人员工具** > **网络** 和 _过滤器_ 表示 `all-teams` 请求。 请注意GraphQL API请求 `/graphql/execute.json/my-project/all-teams` 针对 `http://localhost:3000` 和 **NOT** 值 `REACT_APP_HOST_URI` (例如， <https://publish-p123-e456.adobeaemcloud.com>)。 请求针对React应用程序的域发出，因为 [代理设置](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 启用 `http-proxy-middleware` 模块。


![GraphQL API通过代理请求](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


查看主 `../setupProxy.js` 文件和内部 `../proxy/setupProxy.auth.**.js` 文件注意如何 `/content` 和 `/graphql` 路径已代理，并指示它不是静态资产。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

使用本地代理不适合进行生产部署，有关更多详细信息，请参阅 _生产部署_ 中。

## 恭喜！{#congratulations}

恭喜！您已成功创建React应用程序，以便在基本教程中使用和显示AEM GraphQL API中的数据！
