---
title: 使用GraphQL API建置React應用程式以查詢AEM - AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 建置從AEM GraphQL API擷取內容/資料的React應用程式，也瞭解AEM Headless JS SDK的使用方式。
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


# 建立使用AEM GraphQL API的React應用程式

在本章中，您將探索AEM GraphQL API如何推動外部應用程式中的體驗。

使用簡單的React應用程式來查詢和顯示 **團隊** 和 **個人** AEM GraphQL API公開的內容。 React的使用基本上不重要，而且消費型外部應用程式可以寫入任何平台的任何框架中。

## 前提条件

假設已完成此多部分教學課程先前部分中概述的步驟，或 [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) 安裝在您的AEMas a Cloud Service製作和發佈服務上。

_本章中的IDE熒幕擷取畫面來自 [Visual Studio Code](https://code.visualstudio.com/)_

必須安裝下列軟體：

- [Node.js v18](https://nodejs.org/)
- [Visual Studio Code](https://code.visualstudio.com/)

## 目標

瞭解如何：

- 下載並啟動範例React應用程式
- 使用查詢AEM GraphQL端點 [AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)
- 查詢AEM以取得團隊及其參考成員的清單
- 查詢AEM以取得團隊成員的詳細資訊

## 取得範例React應用程式

在本章中，將以與AEM GraphQL API互動，並顯示從這些API取得的團隊和人員資料所需的程式碼來實作截圖的React應用程式範例。

React應用程式原始碼範例位於Github.com <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

若要取得React應用程式：

1. 複製範例WKND GraphQL React應用程式，從 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導覽至 `basic-tutorial` 資料夾，並在IDE中開啟。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![VSCode中的React應用程式](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新 `.env.development` 以連線至AEMas a Cloud Service發佈服務。

   - 設定 `REACT_APP_HOST_URI`的值設為您的AEMas a Cloud Service的發佈URL (例如 `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`)和 `REACT_APP_AUTH_METHOD`的值至 `none`
   >[!NOTE]
   >
   > 請確定您已發佈專案設定、內容片段模型、編寫的內容片段、GraphQL端點，並保留先前步驟中的查詢。
   >
   > 如果您在本機AEM Author SDK上執行上述步驟，您可以指向 `http://localhost:4502` 和 `REACT_APP_AUTH_METHOD`的值至 `basic`.


1. 從命令列，前往 `aem-guides-wknd-graphql/basic-tutorial` 資料夾

1. 啟動React應用程式

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React應用程式於以下日期以開發模式啟動： [http://localhost:3000/](http://localhost:3000/). 在教學課程中對React應用程式所做的變更會立即反映出來。

![部分實作的React應用程式](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   此React應用程式已部分實作。 依照本教學課程中的步驟完成實施。 需要實施作業的JavaScript檔案有下列註解，請務必使用本教學課程中指定的程式碼，新增/更新這些檔案中的程式碼。
>
>
> //*********************************
>
>  // TODO ：：依照AEM Headless教學課程中的步驟來實作此專案
>
>  //*********************************

## React應用程式的剖析

範例React應用程式有三個主要部分：

1. 此 `src/api` 資料夾包含用來向AEM提出GraphQL查詢的檔案。
   - `src/api/aemHeadlessClient.js` 初始化並匯出用於與AEM通訊的AEM Headless使用者端
   - `src/api/usePersistedQueries.js` 實作 [自訂React鉤點](https://react.dev/docs/hooks-custom.html) 將資料從AEM GraphQL傳回至 `Teams.js` 和 `Person.js` 檢視元件。

1. 此 `src/components/Teams.js` 檔案會使用清單查詢來顯示團隊及其成員的清單。
1. 此 `src/components/Person.js` 檔案會使用引數化的單一結果查詢，顯示單一人員的詳細資訊。

## 檢閱AEMHeadless物件

檢閱 `aemHeadlessClient.js` 檔案以瞭解如何建立 `AEMHeadless` 用來與AEM通訊的物件。

1. 打开 `src/api/aemHeadlessClient.js`.

1. 複查明細行1-40：

   - 匯入 `AEMHeadless` 來自的宣告 [適用於JavaScript的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js)，第11行。

   - 根據中定義的變數設定授權 `.env.development`，第14-22行，以及，箭頭函式運算式 `setAuthorization`，第31-40行。

   - 此 `serviceUrl` 包含的設定 [開發proxy](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configuration （設定），第27行。

1. 第42到49行是最重要的字元，因為它們會將 `AEMHeadless` 使用者端並匯出，以便在整個React應用程式中使用。

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## 實作以執行AEM GraphQL持續查詢

實作一般模型 `fetchPersistedQuery(..)` 執行AEM GraphQL持續查詢的函式開啟 `usePersistedQueries.js` 檔案。 此 `fetchPersistedQuery(..)` 函式使用 `aemHeadlessClient` 物件的 `runPersistedQuery()` 函式以非同步方式執行查詢、承諾型行為。

稍後，自訂React `useEffect` hook呼叫此函式以從AEM擷取特定資料。

1. 在 `src/api/usePersistedQueries.js` **更新** `fetchPersistedQuery(..)`，第35行，程式碼如下。

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

## 實作Teams功能

接下來，建立在React應用程式的主要檢視上顯示「團隊」及其成員的功能。 此功能需要：

- 新 [自訂React useEffect鉤點](https://react.dev/docs/hooks-custom.html) 在 `src/api/usePersistedQueries.js` 會叫用 `my-project/all-teams` 持久查詢，傳回AEM中的團隊內容片段清單。
- 位於的React元件 `src/components/Teams.js` 會叫用新的自訂React `useEffect` 鉤點，並轉譯團隊資料。

完成後，應用程式的主要檢視會填入來自AEM的Teams資料。

![團隊檢視](./assets/graphql-and-external-app/react-app__teams-view.png)

### 步骤

1. 打开 `src/api/usePersistedQueries.js`.

1. 找到函式 `useAllTeams()`

1. 若要建立 `useEffect` 叫用持久查詢的連結 `my-project/all-teams` 透過 `fetchPersistedQuery(..)`，新增下列程式碼。 此連結也只會傳回位於AEM GraphQL回應中的相關資料 `data?.teamList?.items`，可讓React檢視元件與父JSON結構無關。

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

1. 在 `Teams` AEM React元件，使用 `useAllTeams()` 勾點。

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. 執行檢視型資料驗證，根據傳回的資料顯示錯誤訊息或載入指標。

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

1. 最後，呈現Teams資料。 從GraphQL查詢傳回的每個團隊都會使用提供的呈現 `Team` React子元件。

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


## 實作人員功能

使用 [Teams功能](#implement-teams-functionality) 完成，讓我們實作功能以處理團隊成員或個人詳細資訊上的顯示。

此功能需要：

- 新 [自訂React useEffect鉤點](https://react.dev/docs/hooks-custom.html) 在 `src/api/usePersistedQueries.js` 會叫用引數化 `my-project/person-by-name` 持久查詢，並傳回單一人員記錄。

- 位於的React元件 `src/components/Person.js` 以個人全名作為查詢引數的系統，會叫用新的自訂React `useEffect` 鉤點，並呈現個人資料。

完成之後，在「團隊」檢視中選取人員名稱會呈現人員檢視。

![人员](./assets/graphql-and-external-app/react-app__person-view.png)

1. 打开 `src/api/usePersistedQueries.js`.

1. 找到函式 `usePersonByName(fullName)`

1. 若要建立 `useEffect` 叫用持久查詢的連結 `my-project/all-teams` 透過 `fetchPersistedQuery(..)`，新增下列程式碼。 此連結也只會傳回位於AEM GraphQL回應中的相關資料 `data?.teamList?.items`，可讓React檢視元件與父JSON結構無關。

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
1. 在 `Person` React元件，剖析 `fullName` AEM route引數，並使用 `usePersonByName(fullName)` 勾點。

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

1. 執行檢視型資料驗證，根據傳回的資料顯示錯誤訊息或載入指標。

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

1. 最後，轉譯人員資料。

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

## 試用應用程式

檢閱應用程式 [http://localhost:3000/](http://localhost:3000/) 並按一下 _成員_ 連結。 您也可以在AEM中新增內容片段，以新增更多團隊和/或成員至Team Alpha。

## 潛藏於引擎蓋之下

開啟瀏覽器的 **開發人員工具** > **網路** 和 _篩選_ 的 `all-teams` 要求。 注意GraphQL API請求 `/graphql/execute.json/my-project/all-teams` 針對 `http://localhost:3000` 和 **NOT** 與的值比較 `REACT_APP_HOST_URI` (例如， <https://publish-p123-e456.adobeaemcloud.com>)。 系統會針對React應用程式的網域提出要求，原因如下 [Proxy設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 已啟用，使用 `http-proxy-middleware` 模組。


![透過Proxy的GraphQL API請求](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


檢閱主要 `../setupProxy.js` 檔案和範圍 `../proxy/setupProxy.auth.**.js` 檔案注意如何 `/content` 和 `/graphql` 已代理路徑並指示它不是靜態資產。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

使用本機Proxy不適合用於生產部署，如需更多詳細資訊，請參閱 _生產部署_ 區段。

## 恭喜！{#congratulations}

恭喜！您已成功建立React應用程式，以便在基本教學課程中使用和顯示AEM GraphQL API的資料！
