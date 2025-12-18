---
title: 使用GraphQL API构建查询AEM的React应用程序 — AEM Headless快速入门 — GraphQL
description: Adobe Experience Manager (AEM)和GraphQL快速入门。 构建从AEM GraphQL API获取内容/数据的React应用程序，并了解如何使用AEM Headless JS SDK。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 410
source-git-commit: e7f556737cdf6a92c0503d3b4a52eef1f71c8330
workflow-type: tm+mt
source-wordcount: '1479'
ht-degree: 2%

---


# 构建使用AEM的GraphQL API的React应用程序

在本章中，您将了解AEM的GraphQL API如何改善外部应用程序中的体验。

使用简单的React应用程序查询和显示AEM GraphQL API公开的&#x200B;**团队**&#x200B;和&#x200B;**人员**&#x200B;内容。 React的使用在很大程度上并不重要，消费的外部应用程序可以在任何平台的任何框架中编写。

## 先决条件

假定已完成此多部分教程前面部分中概述的步骤，或者[basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip)已安装到您的AEM创作和发布服务中。

本章中的&#x200B;_IDE屏幕截图来自[Visual Studio Code](https://code.visualstudio.com/)_

### AEM环境

要完成本教程，建议您拥有AEM管理员权限来访问以下任一项：

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=zh-Hans)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)进行本地设置
+ [AEM 6.5 LTS](https://experienceleague.adobe.com/docs/experience-manager-65/content/release-notes/release-notes.html?lang=zh-Hans)，已安装[GraphQL索引包1.0.5+](https://experienceleague.adobe.com/docs/experience-manager-65/content/headless/graphql-api/graphql-endpoint.html)

### 软件要求

必须安装以下软件：

+ [Node.js v18+](https://nodejs.org/en)
+ [Visual Studio代码](https://code.visualstudio.com/)
+ [Git](https://git-scm.com/)
+ [Java JDK](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-65/content/implementing/deploying/introduction/technical-requirements)&#x200B;(如果连接到本地AEM SDK或6.5实例)

## 目标

了解如何：

+ 下载并启动示例React应用程序
+ 使用[AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)查询AEM的GraphQL端点
+ 在AEM中查询团队及其引用的成员列表
+ 查询AEM以了解团队成员的详细信息

## 获取示例React应用程序

在本章中，我们使用与AEM的GraphQL API交互以及显示从这些API获得的团队和人员数据所需的代码实施了一个存根示例React应用程序。

Github.com上的<https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>提供了示例React应用程序源代码

获取React应用程序：

1. 从[Github.com](https://github.com/adobe/aem-guides-wknd-graphql)克隆示例WKND GraphQL React应用程序。 请记住，此Git存储库包含多个项目，因此请确保导航到`basic-tutorial`文件夹，如下一步骤中所述。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 导航到`basic-tutorial`文件夹并在IDE中将其打开。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   在VSCode中![React应用程序](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新`.env.development`以连接到您的AEM发布服务。

   有关示例项目的环境变量以及如何设置这些变量的更多详细信息，[查看README.md](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#update-environment-variables)。

   **AEM as a Cloud Service**

   将React应用程序连接到AEM as a Cloud Service Publish服务时，将`REACT_APP_HOST_URI`的值设置为您的AEM as a Cloud Service发布URL(例如 `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`)和`REACT_APP_AUTH_METHOD`的值更改为`none`

   **适用于AEM as a Cloud Service的AEM SDK（本地）**

   将React应用程序连接到本地AEM as a Cloud Service SDK环境时，将`REACT_APP_HOST_URI`的值设置为您的本地主机和发布端口(例如， `REACT_APP_HOST_URI=http://localhost:4503`)和`REACT_APP_AUTH_METHOD`的值更改为`none`

   **AEM 6.5 LTS托管环境**

   将React应用程序连接到AEM as a Cloud Service Publish服务时，将`REACT_APP_HOST_URI`的值设置为您的AEM 6.5发布URL(例如 `REACT_APP_HOST_URI=https://dev.mysite.com`)和`REACT_APP_AUTH_METHOD`的值更改为`none`

   **AEM 6.5 LTS快速入门（本地）**

   将React应用程序连接到本地AEM 6.5快速启动时，将`REACT_APP_HOST_URI`的值设置为您的本地主机和发布端口(例如 `REACT_APP_HOST_URI=http://localhost:4503`)和`REACT_APP_AUTH_METHOD`的值更改为`none`

   >[!NOTE]
   >
   > 确保您已发布项目配置、内容片段模型、创作的内容片段、GraphQL端点以及之前步骤中的持久查询。
   >
   > 如果您在本地AEM Author SDK上执行上述步骤，则可以指向`http://localhost:4502`和`REACT_APP_AUTH_METHOD`的`basic`值。


1. 从命令行转到`aem-guides-wknd-graphql/basic-tutorial`文件夹

1. 启动React应用程序

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React应用程序在[http://localhost:3000/](http://localhost:3000/)上以开发模式启动。 在整个教程中对React应用程序所做的更改会立即反映出来。

![部分实施的React应用程序](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   此React应用程序已部分实施。 按照本教程中的步骤完成实施。 需要实施工作的JavaScript文件具有下列注释，请确保使用本教程中指定的代码添加/更新这些文件中的代码。
>
>
> //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>
>  // TODO按照AEM Headless教程中的步骤实施此操作
>
>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>

## React应用程序剖析

示例React应用程序有三个主要部分：

1. `src/api`文件夹包含用于向AEM进行GraphQL查询的文件。
   + `src/api/aemHeadlessClient.js`初始化并导出用于与AEM通信的AEM Headless客户端
   + `src/api/usePersistedQueries.js`实现[自定义React挂接](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components)将数据从AEM GraphQL返回到`Teams.js`和`Person.js`视图组件。

1. `src/components/Teams.js`文件使用列表查询显示团队及其成员的列表。
1. `src/components/Person.js`文件使用参数化单结果查询显示单个人员的详细信息。

## 查看AEMHeadless对象

查看`aemHeadlessClient.js`文件，了解如何创建用于与AEM通信的`AEMHeadless`对象。

1. 打开 `src/api/aemHeadlessClient.js`。

1. 复查行1-40：

   + 从`AEMHeadless`AEM Headless Client for JavaScript[导入](https://github.com/adobe/aem-headless-client-js)声明，第11行。

   + 授权配置基于`.env.development`，第14-22行中定义的变量，以及箭头函数表达式`setAuthorization`，第31-40行。

   + 所包含`serviceUrl`开发代理[配置的](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#proxy-api-requests)设置，第27行。

1. 第42-49行是最为重要的，因为它们会实例化`AEMHeadless`客户端并将其导出以供在整个React应用程序中使用。

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

要实施通用`fetchPersistedQuery(..)`函数以运行AEM GraphQL持久查询，请打开`usePersistedQueries.js`文件。 `fetchPersistedQuery(..)`函数使用`aemHeadlessClient`对象的`runPersistedQuery()`函数来异步运行基于承诺的查询。

稍后，自定义React `useEffect`挂接调用此函数以从AEM中检索特定数据。

1. 在`src/api/usePersistedQueries.js` **中，使用下面的代码更新** `fetchPersistedQuery(..)`的第35行。

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

+ [中的新](https://react.dev/reference/react/useEffect#useeffect)自定义React useEffect挂钩`src/api/usePersistedQueries.js`调用`my-project/all-teams`持久查询，返回AEM中的Team内容片段列表。
+ 位于`src/components/Teams.js`的React组件调用新的自定义React `useEffect`挂接，并呈现Teams数据。

完成后，应用程序的主视图会填充AEM中的团队数据。

![团队视图](./assets/graphql-and-external-app/react-app__teams-view.png)

### 步骤

1. 打开 `src/api/usePersistedQueries.js`。

1. 找到函数`useAllTeams()`

1. 要创建通过`useEffect`调用持久查询`my-project/all-teams`的`fetchPersistedQuery(..)`挂接，请添加以下代码。 该挂接还仅从`data?.teamList?.items`处的AEM GraphQL响应返回相关数据，从而允许React视图组件独立于父JSON结构。

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

1. 打开`src/components/Teams.js`

1. 在`Teams` React组件中，使用`useAllTeams()`挂接从AEM获取团队列表。

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

1. 最后，渲染团队数据。 使用提供的`Team` React子组件呈现从GraphQL查询返回的每个团队。

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

完成[团队功能](#implement-teams-functionality)后，让我们实施该功能来处理团队成员或人员的详细信息上的显示。

此功能需要：

+ [中的新](https://react.dev/reference/react/useEffect#useeffect)自定义React useEffect挂接`src/api/usePersistedQueries.js`，它调用参数化的`my-project/person-by-name`持久查询并返回单个人员记录。

+ 位于`src/components/Person.js`的React组件使用人员的全名作为查询参数，调用新的自定义React `useEffect`挂接，并呈现人员数据。

完成后，在“团队”视图中选择人员姓名将渲染人员视图。

![人员](./assets/graphql-and-external-app/react-app__person-view.png)

1. 打开 `src/api/usePersistedQueries.js`。

1. 找到函数`usePersonByName(fullName)`

1. 要创建通过`useEffect`调用持久查询`my-project/all-teams`的`fetchPersistedQuery(..)`挂接，请添加以下代码。 该挂接还仅从`data?.teamList?.items`处的AEM GraphQL响应返回相关数据，从而允许React视图组件独立于父JSON结构。

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

1. 打开`src/components/Person.js`
1. 在`Person` React组件中，解析`fullName`路由参数，并使用`usePersonByName(fullName)`挂接从AEM获取人员数据。

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


## CORS 配置

此示例React应用程序在开发期间不需要跨源资源共享(CORS)，因为它使用本地代理连接到AEM。 使用本地代理仅用于促进快速开发，不适用于非开发用例。

但是，在生产场景中，通常最佳实践是客户端应用程序从用户的浏览器直接与AEM通信。 若要启用此功能，可能需要在AEM[中配置](../deployment/overview.md)CORS，以允许从Web应用获取请求。

## 尝试该应用程序

查看应用程序[http://localhost:3000/](http://localhost:3000/)，然后单击&#x200B;_成员_&#x200B;链接。 此外，您还可以通过在AEM中添加内容片段来向团队Alpha添加更多团队和/或成员。

>[!IMPORTANT]
>
>要验证您的实施更改，或者如果在进行上述更改后无法使应用程序正常工作，请参阅[basic-tutorial](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) `solution`分支。

## 在幕后工作

为&#x200B;**请求打开浏览器的**&#x200B;开发人员工具&#x200B;**>**&#x200B;网络&#x200B;_和_&#x200B;筛选器`all-teams`。 请注意，GraphQL API请求`/graphql/execute.json/my-project/all-teams`是针对`http://localhost:3000`而提出的，而&#x200B;**NOT**&#x200B;是针对`REACT_APP_HOST_URI`的值，例如`<https://publish-pxxx-exxx.adobeaemcloud.com`。 这些请求是针对React应用程序的域发出的，因为使用[模块启用了](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)代理设置`http-proxy-middleware`。


通过代理![GraphQL API请求](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


查看主`../setupProxy.js`文件并在`../proxy/setupProxy.auth.**.js`文件中注意到`/content`和`/graphql`路径是如何进行代理的，并指示它不是静态资源。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

使用本地代理不适合用于生产部署，可以在&#x200B;_生产部署_&#x200B;部分中找到更多详细信息。

## 恭喜！{#congratulations}

恭喜！作为基础教程的一部分，您已成功创建React应用程序以使用和显示AEM GraphQL API中的数据！
