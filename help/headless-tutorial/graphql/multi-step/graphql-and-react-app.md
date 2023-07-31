---
title: 使用GraphQL API构建查询AEM的React应用程序 — AEM Headless快速入门 — GraphQL
description: Adobe Experience Manager (AEM)和GraphQL入门。 构建从AEM GraphQL API获取内容/数据的React应用程序，并了解如何使用AEM Headless JS SDK。
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 7938325427b6becb38ac230a3bc4b031353ca8b1
workflow-type: tm+mt
source-wordcount: '1188'
ht-degree: 2%

---


# 构建使用AEM GraphQL API的React应用程序

在本章中，您将探索AEM GraphQL API如何提升外部应用程序中的体验。

使用简单的React应用程序进行查询和显示 **团队** 和 **人员** AEM GraphQL API公开的内容。 React的使用在很大程度上并不重要，消费的外部应用程序可以在任何平台的任何框架中编写。

## 前提条件

假定已完成本多部分教程前面部分中概述的步骤，或者 [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip) 安装在您的AEMas a Cloud Service创作和发布服务上。

_本章中的IDE屏幕截图来自 [Visual Studio代码](https://code.visualstudio.com/)_

必须安装以下软件：

- [Node.js v18](https://nodejs.org/en)
- [Visual Studio代码](https://code.visualstudio.com/)

## 目标

了解如何：

- 下载并启动示例React应用程序
- 使用查询AEM GraphQL端点 [AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)
- 查询AEM以获取团队及其引用的成员的列表
- 查询AEM以了解团队成员的详细信息

## 获取示例React应用程序

在本章中，我们使用与AEM GraphQL API交互所需的代码实施了一个截短的示例React应用程序，并显示从这些应用程序获得的团队和人员数据。

Github.com上提供了示例React应用程序源代码，网址为 <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

获取React应用程序：

1. 从克隆示例WKND GraphQL React应用程序 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 导航到 `basic-tutorial` 文件夹并在IDE中将其打开。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![VSCode中的React应用程序](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新 `.env.development` 以连接到AEMas a Cloud Service发布服务。

   - 设置 `REACT_APP_HOST_URI`的值设为您的AEMas a Cloud Service的发布URL(例如 `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`)和 `REACT_APP_AUTH_METHOD`的值到 `none`

   >[!NOTE]
   >
   > 确保您已发布项目配置、内容片段模型、创作的内容片段、GraphQL端点以及之前步骤中的持久查询。
   >
   > 如果您在本地AEM Author SDK上执行了上述步骤，则可以指向 `http://localhost:4502` 和 `REACT_APP_AUTH_METHOD`的值到 `basic`.


1. 从命令行转到 `aem-guides-wknd-graphql/basic-tutorial` 文件夹

1. 启动React应用程序

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React应用程序在开发模式下启动 [http://localhost:3000/](http://localhost:3000/). 在整个教程中，都会立即反映对React应用程序所做的更改。

![部分实施的React应用程序](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   此React应用程序已部分实施。 按照本教程中的步骤完成实施。 需要实施工作的JavaScript文件包含以下注释，请确保使用本教程中指定的代码添加/更新这些文件中的代码。
>
>
> //*********************************
>
>  // TODO ：按照AEM Headless教程中的步骤实施此功能
>
>  //*********************************
>

## React应用程序剖析

示例React应用程序有三个主要部分：

1. 此 `src/api` 文件夹包含用于向AEM进行GraphQL查询的文件。
   - `src/api/aemHeadlessClient.js` 初始化并导出用于与AEM通信的AEM Headless客户端
   - `src/api/usePersistedQueries.js` 实施 [自定义React挂钩](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) 将数据从AEM GraphQL返回到 `Teams.js` 和 `Person.js` 查看组件。

1. 此 `src/components/Teams.js` 文件使用列表查询显示团队及其成员的列表。
1. 此 `src/components/Person.js` 文件使用参数化单结果查询显示单个人员的详细信息。

## 查看AEMHeadless对象

查看 `aemHeadlessClient.js` 有关如何创建 `AEMHeadless` 用于与AEM通信的对象。

1. 打开 `src/api/aemHeadlessClient.js`.

1. 复查行1-40：

   - 导入 `AEMHeadless` 声明来自 [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js)，第11行。

   - 根据中定义的变量配置授权 `.env.development`，第14-22行，以及，箭头函数表达式 `setAuthorization`，第31-40行。

   - 此 `serviceUrl` 包含的设置 [开发代理](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) 配置，27行。

1. 第42-49行是最为重要的，因为它们将 `AEMHeadless` 客户端并将其导出，以在整个React应用程序中使用。

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

实施通用 `fetchPersistedQuery(..)` 用于运行AEM GraphQL持久查询的函数打开 `usePersistedQueries.js` 文件。 此 `fetchPersistedQuery(..)` 函数使用 `aemHeadlessClient` 对象的 `runPersistedQuery()` 函数以异步运行查询、基于承诺的行为。

稍后，自定义React `useEffect` hook调用此函数以从AEM检索特定数据。

1. 在 `src/api/usePersistedQueries.js` **更新** `fetchPersistedQuery(..)`，第35行，代码如下。

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

接下来，构建在React应用程序的主视图上显示团队及其成员的功能。 此功能需要：

- 新 [自定义React useEffect挂接](https://react.dev/reference/react/useEffect#useeffect) 在 `src/api/usePersistedQueries.js` 调用 `my-project/all-teams` 持久查询，返回AEM中的团队内容片段列表。
- 位于的React组件 `src/components/Teams.js` 调用新的自定义React `useEffect` 挂接，并呈现团队数据。

完成后，应用程序的主视图将使用来自AEM的团队数据填充。

![团队视图](./assets/graphql-and-external-app/react-app__teams-view.png)

### 步骤

1. 打开 `src/api/usePersistedQueries.js`.

1. 找到函数 `useAllTeams()`

1. 创建 `useEffect` 调用持久查询的挂接 `my-project/all-teams` via `fetchPersistedQuery(..)`，添加以下代码。 此挂接还仅返回来自AEM GraphQL响应的相关数据，其位置 `data?.teamList?.items`，从而使React视图组件与父JSON结构无关。

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

1. 在 `Teams` React组件，使用从AEM获取团队列表 `useAllTeams()` 钩子。

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

1. 最后，渲染团队数据。 从GraphQL查询返回的每个团队都会使用提供的 `Team` React子组件。

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

使用 [团队功能](#implement-teams-functionality) 完成，我们实施相应的功能来处理团队成员或人员的详细信息显示。

此功能需要：

- 新 [自定义React useEffect挂接](https://react.dev/reference/react/useEffect#useeffect) 在 `src/api/usePersistedQueries.js` 调用参数化 `my-project/person-by-name` 持久查询，并返回单个人员记录。

- 位于的React组件 `src/components/Person.js` 将人员的全名用作查询参数的活动，会调用新的自定义React `useEffect` 挂接，并呈现人员数据。

完成后，在“团队”视图中选择人员姓名将渲染人员视图。

![人员](./assets/graphql-and-external-app/react-app__person-view.png)

1. 打开 `src/api/usePersistedQueries.js`.

1. 找到函数 `usePersonByName(fullName)`

1. 创建 `useEffect` 调用持久查询的挂接 `my-project/all-teams` via `fetchPersistedQuery(..)`，添加以下代码。 此挂接还仅返回来自AEM GraphQL响应的相关数据，其位置 `data?.teamList?.items`，从而使React视图组件与父JSON结构无关。

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
1. 在 `Person` React组件，解析 `fullName` route参数，并使用从AEM获取人员数据 `usePersonByName(fullName)` 钩子。

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

1. 最后，呈现人员数据。

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

## 尝试该应用程序

查看应用程序 [http://localhost:3000/](http://localhost:3000/) 并单击 _成员_ 链接。 此外，您还可以通过在AEM中添加内容片段来向团队Alpha添加更多团队和/或成员。

## 在幕后工作

打开浏览器的 **开发人员工具** > **网络** 和 _筛选_ 对象 `all-teams` 请求。 请注意GraphQL API请求 `/graphql/execute.json/my-project/all-teams` 制造反对 `http://localhost:3000` 和 **NOT** 相对于的值 `REACT_APP_HOST_URI` (例如， <https://publish-p123-e456.adobeaemcloud.com>)。 这些请求是针对React应用程序的域发起的，因为 [代理设置](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用以下方式启用 `http-proxy-middleware` 模块。


![通过代理的GraphQL API请求](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


查看主要 `../setupProxy.js` 文件和 `../proxy/setupProxy.auth.**.js` 文件注意如何 `/content` 和 `/graphql` 已代理路径并指示它不是静态资源。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

使用本地代理不适合用于生产部署，有关详细信息，请访问 _生产部署_ 部分。

## 恭喜！{#congratulations}

恭喜！作为基础教程的一部分，您已成功创建React应用程序以使用和显示AEM GraphQL API中的数据！
