---
title: 使用AEM OpenAPI构建React应用程序 | Headless教程第4部分
description: Adobe Experience Manager (AEM)和OpenAPI快速入门。 构建可从AEM基于OpenAPI的内容片段投放API中获取内容/数据的React应用程序。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 900
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 1%

---


# 使用AEM的内容片段投放OpenAPI构建React应用程序

在本章中，您将探索如何使用OpenAPI投放AEM内容片段来改善外部应用程序中的体验。

使用简单的React应用程序来请求和显示通过OpenAPI的AEM内容片段投放公开的&#x200B;**团队**&#x200B;和&#x200B;**人员**&#x200B;内容。 React的使用在很大程度上不重要，而且消费的外部应用程序可以在任何框架中写入任何平台，只要它可以向AEM as a Cloud Service发出HTTP请求即可。

## 先决条件

假定已完成本多部分教程前面部分中概述的步骤。

必须安装以下软件：

* [Node.js v22+](https://nodejs.org/en)
* [Visual Studio代码](https://code.visualstudio.com/)

## 目标

了解如何：

* 下载并启动示例React应用程序。
* 使用OpenAPI调用AEM内容片段交付，以获取团队及其引用的成员列表。
* 使用OpenAPI调用AEM内容片段投放，以检索团队成员的详细信息。

## 在AEM as a Cloud Service上设置CORS

此示例React应用程序在本地运行（在`http://localhost:3000`上），并使用OpenAPI API连接到AEM Publish服务的AEM内容片段交付。 要允许此连接，必须在AEM Publish（或Preview）服务上配置CORS（跨源资源共享）。

按照有关设置在[上运行的SPA的`http://localhost:3000`说明操作，以允许向AEM发布服务](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#different-domains)发送CORS请求。

### 本地CORS代理

或者，为了进行开发，请运行一个[本地CORS代理](https://www.npmjs.com/package/local-cors-proxy)，该代理可提供与AEM的CORS友好连接。

```bash
$ npm install --global lcp
$ lcp --proxyUrl https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com
```

将`--proxyUrl`值更新到AEM发布（或预览） URL。

运行本地CORS代理后，在`http://localhost:8010/proxy`上访问AEM内容片段投放API以避免CORS问题。

## 克隆示例React应用程序

使用与OpenAPI的AEM内容片段投放交互以及显示从这些API获得的团队和人员数据所需的代码实施一个截尾的示例React应用程序。

Github.com[上提供了示例React应用程序源代码](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic)。

获取React应用程序：

1. 从[Github.com](https://github.com/adobe/aem-tutorials)从[`headless_open-api_basic`标记](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic)克隆示例WKND OpenAPI React应用程序。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-tutorials.git
   $ cd aem-tutorials  
   $ git fetch --tags
   $ git tag
   $ git checkout tags/headless_open-api_basic
   ```

1. 导航到`headless/open-api/basic`文件夹并在IDE中将其打开。

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ code .
   ```

1. 更新`.env`以连接到AEM as a Cloud Service发布服务，因为我们的内容片段将在此发布。 如果您希望使用AEM预览服务测试应用程序（并且内容片段发布在该处），则可以指向AEM预览服务。

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   ```

   使用[本地CORS代理](#local-cors-proxy)时，将`REACT_APP_HOST_URI`设置为`http://localhost:8010/proxy`。

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=http://localhost:8010/proxy
   ```

1. 启动React应用程序

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ npm install
   $ npm start
   ```

1. React应用程序在[http://localhost:3000/](http://localhost:3000/)上以开发模式启动。 在整个教程中对React应用程序所做的更改会立即在Web浏览器中反映出来。

>[!IMPORTANT]
>
>   此React应用程序已部分实施。 按照本教程中的步骤完成实施。 需要实施工作的JavaScript文件具有下列注释，请确保使用本教程中指定的代码添加/更新这些文件中的代码。
>
>
>  //*********************************
>  >  // TODO：按照AEM Headless教程中的步骤实施此设置
>  >  //*********************************
>

## React应用程序剖析

示例React应用程序有三个主要部分需要更新。

1. `.env`文件包含AEM发布（或预览）服务URL。
1. `src/components/Teams.js`显示团队及其成员的列表。
1. `src/components/Person.js`显示单个团队成员的详细信息。

## 实施团队功能

构建在React应用程序的主视图上显示团队及其成员的功能。 此功能需要：

* 新的[自定义React useEffect挂钩](https://react.dev/reference/react/useEffect#useeffect)通过获取请求调用&#x200B;**列出所有内容片段API**，然后获取每个`fullName`的`teamMember`值以进行显示。

完成后，应用程序的主视图会填充AEM中的团队数据。

![团队视图](./assets/4/teams.png)

1. 打开 `src/components/Teams.js`。

1. 实施&#x200B;**团队**&#x200B;组件以从[列出所有内容片段API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragments)获取团队列表，并渲染团队内容。 这分为以下步骤：

1. 创建一个`useEffect`挂接，该挂接将调用AEM的&#x200B;**List all Content Fragments** API，并将数据存储到React组件的状态。
1. 对于返回的每个&#x200B;**团队**&#x200B;内容片段，调用&#x200B;**获取内容片段** API以获取团队的完全水合详细信息，包括其成员及其`fullNames`。
1. 使用`Team`函数渲染团队数据。

   ```javascript
   import { useEffect, useState } from "react";
   import { Link } from "react-router-dom";
   import "./Teams.scss";
   
   function Teams() {
   
     // The teams folder is the only folder-tree that is allowed to contain Team Content Fragments.
     const TEAMS_FOLDER = '/content/dam/my-project/en/teams';
   
     // State to store the teams data
     const [teams, setTeams] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches all teams and their associated member details
       * This is a two-step process:
       * 1. First, get all team content fragments from the specified folder
       * 2. Then, for each team, fetch the full details including hydrated references to get the team member names
       */
       const fetchData = async () => {
         try {
           // Step 1: Fetch all teams from the teams folder
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments?path=${TEAMS_FOLDER}`
           );
           const allTeams = (await response.json()).items || [];
   
           // Step 2: Fetch detailed information for each team with hydrated references
           const hydratedTeams = [];
           for (const team of allTeams) {
             const hydratedTeamResponse = await fetch(
               `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${team.id}?references=direct-hydrated`
             );
             hydratedTeams.push(await hydratedTeamResponse.json());
           }
   
           setTeams(hydratedTeams);
         } catch (error) {
           console.error("Error fetching content fragments:", error);
         }
       };
   
       fetchData();
     }, [TEAMS_FOLDER]);
   
     // Show loading state while teams data is being fetched
     if (!teams) {
       return <div>Loading teams...</div>;
     }
   
     // Render the teams
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return (
             <Team 
               key={index} 
               {..team}
             />
           );
         })}
       </div>
     );
   }
   
   /**
   * Team component - renders a single team with its details and members
   * @param {string} fields - The authorable fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   */
   function Team({ fields, references, path }) {
     if (!fields.title || !fields.teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{fields.title}</h2>
         {/* Render description as HTML using dangerouslySetInnerHTML */}
         <p 
           className="team__description" 
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
         />
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render each team member as a link to their detail page */}
             {fields.teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                     {/* Display the full name from the hydrated reference */}
                     {references[teamMember].value.fields.fullName}
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

完成[团队功能](#implement-teams-functionality)后，实施功能以处理团队成员或人员的详细信息上的显示。

![人员视图](./assets/4/person.png)

要执行此操作：

1. 打开`src/components/Person.js`
1. 在`Person` React组件中，解析`id`路由参数。 请注意，之前已将React应用程序的路由设置为接受`id` URL参数（请参阅`/src/App.js`）。
1. 从AEM获取[获取内容片段API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragment)的人员数据。

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
     // Get the person ID from the URL parameter
     const { id } = useParams();
   
     // State to store the person data
     const [person, setPerson] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches person data from AEM Content Fragment Delivery API
       * Uses the ID from URL parameters to get the specific person's details
       */
       const fetchData = async () => {
         try {
           /* Hydrate references for access to profilePicture asset path */
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${id}?references=direct-hydrated`
           );
           const json = await response.json();
           setPerson(json || null);
         } catch (error) {
           console.error("Error fetching person data:", error);
         }
       };
       fetchData();
     }, [id]); // Re-fetch when ID changes
   
     // Show loading state while person data is being fetched
     if (!person) {
       return <div>Loading person...</div>;
     }
   
     return (
       <div className="person">
         {/* Person profile image - Look up the profilePicture reference in the references object */}
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
           alt={person.fields.fullName}
         />
         {/* Display person's occupations */}
         <div className="person__occupations">
           {person.fields.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
   
         {/* Person's main content: name and biography */}
         <div className="person__content">
           <h1 className="person__full-name">{person.fields.fullName}</h1>
           {/* Render biography as HTML content */}
           <div
             className="person__biography"
             dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
           />
         </div>
       </div>
     );  
   }
   
   export default Person;
   ```

### 获取完成的代码

本章的完整源代码可在Github.com[上找到](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end)。

```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_4-end
```

## 尝试该应用程序

查看应用程序[http://localhost:3000/](http://localhost:3000/)，然后单击&#x200B;_团队成员_&#x200B;链接。 此外，您还可以通过在AEM创作服务中添加内容片段并发布它们，将更多团队和/或成员添加到Alpha团队。

## 深入了解

在与React应用程序交互时，打开浏览器的&#x200B;**开发人员工具>网络**&#x200B;控制台和&#x200B;**筛选**&#x200B;以获取`/adobe/contentFragments`获取请求。

## 恭喜！

恭喜！您已成功创建了React应用程序，以便通过OpenAPI使用和显示AEM内容片段投放中的内容片段。
