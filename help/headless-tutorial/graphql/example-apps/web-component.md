---
title: Web元件/JS - AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此Web元件/JS應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 5%

---

# 網頁元件

範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此Web元件應用程式示範了如何使用AEM GraphQL API透過持續查詢來查詢內容，以及如何演算部分UI，使用純JavaScript程式碼完成。

![具有AEM Headless的網頁元件](./assets/web-component/web-component.png)

檢視 [GitHub上的原始程式碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 前提条件 {#prerequisites}

下列工具應安裝在本機：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=limit&amp;p.limit=0&amp;p.limit=144) (如果連線到本機AEM 6.5或AEM SDK)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM需求

Web元件可與下列AEM部署選項搭配使用。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本機設定，使用 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)
+ [AEM 6.5 SP13+快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans？lang=en#install-local-aem-instances)

所有部署都需要 `tutorial-solution-content.zip` 從 [解決方案檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) 安裝及必要 [部署設定](../deployment/web-component.md) 都會執行。


>[!IMPORTANT]
>
>Web元件的設計是要連線至 __AEM發佈__ 不過，如果網頁元件的中提供驗證，則它可以從AEM Author取得內容 [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 檔案。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導覽至 `web-component` 子目錄。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 編輯 `.../src/person.js` 檔案以包含AEM連線詳細資料：

   在 `aemHeadlessService` 物件，更新 `aemHost` 指向您的AEM Publish服務。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   如果連線到AEM Author服務，請在 `aemCredentials` 物件，提供本機AEM使用者認證。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 開啟終端機並執行命令 `aem-guides-wknd-graphql/web-component`：

   ```shell
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器視窗會開啟靜態HTML頁面，其中嵌入Web元件： [http://localhost:8080](http://localhost:8080).
1. 此 _個人資訊_ 網頁元件會顯示在網頁上。

## 程式碼

以下摘要說明如何建立Web元件、如何連線至AEM Headless以使用GraphQL持續查詢擷取內容，以及資料如何呈現。 完整的程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### 網頁元件HTML標籤

可重複使用的Web元件（亦稱為自訂元素） `<person-info>` 新增至 `../src/assets/aem-headless.html` HTML頁面。 它支援 `host` 和 `query-param-value` 屬性來驅動元件的行為。 此 `host` 屬性的值覆寫 `aemHost` 值自 `aemHeadlessService` 中的物件 `person.js`、和 `query-param-value` 用於選取要呈現的人員。

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web元件實施

此 `person.js` 會定義網頁元件功能，以下是其中的重要焦點。

#### PersonInfo元素實作

此 `<person-info>` 自訂元素的類別物件會使用 `connectedCallback()` 生命週期方法、附加陰影根目錄、擷取GraphQL持續查詢，以及使用DOM操作來建立自訂元素的內部陰影DOM結構。

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### 註冊 `<person-info>` 元素

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### 跨原始資源共用(CORS)

此Web元件仰賴在目標AEM環境中執行的AEM型CORS設定，並假設主機頁面執行於 `http://localhost:8080` 在開發模式及以下為本機AEM Author服務的CORS OSGi設定範例。

請檢閱 [部署設定](../deployment/web-component.md) 適用於個別AEM服務。

![CORS設定](assets/react-app/cross-origin-resource-sharing-configuration.png)
