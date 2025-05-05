---
title: Web组件/JS - AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的好方法。 此Web组件/JS应用程序演示了如何通过AEM的GraphQL API，使用持久化查询来查询内容。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 129
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---

# Web组件

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的好方法。 此Web组件应用程序演示了如何使用AEM的GraphQL API通过持久查询来查询内容，以及如何呈现部分UI(使用纯JavaScript代码完成)。

具有AEM Headless的![Web组件](./assets/web-component/web-component.png)

在GitHub[&#128279;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)上查看源代码

## 先决条件 {#prerequisites}

应在本地安装以下工具：

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM要求

Web组件可与以下AEM部署选项配合使用。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)进行本地设置
   + 需要[JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=list&amp;p.offset=0&amp;p.limit=14&amp;p.limit=144)&#x200B;(如果连接到本地AEM 6.5或AEM SDK)

此示例应用依赖于要安装[basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip)，并且所需的[部署配置](../deployment/web-component.md)已准备就绪。


>[!IMPORTANT]
>
>Web组件设计为连接到&#x200B;__AEM Publish__&#x200B;环境，但是，如果在Web组件的[`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11)文件中提供了身份验证，则它可以从AEM Author获取内容。

## 使用方法

1. 克隆`adobe/aem-guides-wknd-graphql`存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 导航到`web-component`子目录。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 编辑`.../src/person.js`文件以包含AEM连接详细信息：

   在`aemHeadlessService`对象中，更新`aemHost`以指向您的AEM发布服务。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   如果连接到AEM Author服务，请在`aemCredentials`对象中，提供本地AEM用户凭据。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 打开终端并运行来自`aem-guides-wknd-graphql/web-component`的命令：

   ```shell
   $ npm install
   $ npm start
   ```

1. 新的浏览器窗口将打开一个静态HTML页面，该页面嵌入了[http://localhost:8080](http://localhost:8080)处的Web组件。
1. _人员信息_ Web组件显示在网页上。

## 代码

以下摘要介绍了Web组件的构建方式、它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及这些数据的呈现方式。 完整的代码可在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)上找到。

### Web组件HTML标记

可重复使用的Web组件（又称自定义元素）`<person-info>`已添加到`../src/assets/aem-headless.html` HTML页面。 它支持`host`和`query-param-value`属性来驱动组件的行为。 `host`属性的值覆盖`person.js`中`aemHeadlessService`对象的`aemHost`值，并且`query-param-value`用于选择要呈现的人员。

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web组件实施

`person.js`定义了Web组件功能，下面是其中的关键亮点。

#### PersonInfo元素实施

`<person-info>`自定义元素的类对象通过使用`connectedCallback()`生命周期方法、附加影子根、获取GraphQL持久查询和DOM操作来创建自定义元素的内部影子DOM结构来定义功能。

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
        personImgElement.setAttribute('src',
            host + (person.profilePicture._dynamicUrl || person.profilePicture._path));
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### 注册`<person-info>`元素

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### 跨源资源共享(CORS)

此Web组件依赖于在目标AEM环境中运行的基于AEM的CORS配置，并假定主机页面在开发模式下运行于`http://localhost:8080`，以下是本地AEM Author服务的CORS OSGi配置示例。

请查看各个AEM服务的[部署配置](../deployment/web-component.md)。
