---
title: Web组件/JS - AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此Web组件/JS应用程序演示了如何使用AEM GraphQL API通过持久化查询来查询内容。
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

# Web组件

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此Web组件应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容并渲染UI的一部分，该操作使用纯JavaScript代码完成。

![带有AEM Headless的Web组件](./assets/web-component/web-component.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=limit&amp;p.limit=0&amp;p.limit=144) (如果连接到本地AEM 6.5或AEM SDK)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM要求

Web组件可与以下AEM部署选项配合使用。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用进行本地设置 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)
+ [AEM 6.5 SP13+快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans？lang=en#install-local-aem-instances)

所有部署都需要 `tutorial-solution-content.zip` 从 [解决方案文件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) 安装和必需 [部署配置](../deployment/web-component.md) 执行。


>[!IMPORTANT]
>
>Web组件设计为连接到 __AEM发布__ 但是，如果在Web组件的中提供身份验证，则它可以从AEM Author获取内容 [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 文件。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 导航到 `web-component` 子目录。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 编辑 `.../src/person.js` 文件以包含AEM连接详细信息：

   在 `aemHeadlessService` 对象，更新 `aemHost` 以指向您的AEM Publish服务。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   如果连接到AEM Author服务，则在 `aemCredentials` 对象，提供本地AEM用户凭据。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 打开终端并运行以下命令： `aem-guides-wknd-graphql/web-component`：

   ```shell
   $ npm install
   $ npm start
   ```

1. 新的浏览器窗口将打开嵌入Web组件的静态HTML页 [http://localhost:8080](http://localhost:8080).
1. 此 _人员信息_ Web组件显示在网页上。

## 代码

以下概要介绍了Web组件的构建方式、它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及数据如何呈现。 完整的代码可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Web组件HTML标记

可重复使用的Web组件（又称自定义元素） `<person-info>` 已添加到 `../src/assets/aem-headless.html` HTML页面。 它支持 `host` 和 `query-param-value` 属性来驱动组件的行为。 此 `host` 属性的值覆盖 `aemHost` 值自 `aemHeadlessService` 中的对象 `person.js`、和 `query-param-value` 用于选择要呈现的人员。

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web组件实施

此 `person.js` 定义Web组件功能，下面是其中的关键亮点。

#### PersonInfo元素实施

此 `<person-info>` 自定义元素的类对象通过使用 `connectedCallback()` 生命周期方法、附加影子根、获取GraphQL持久查询，以及DOM操作以创建自定义元素的内部影子DOM结构。

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

#### 注册 `<person-info>` 元素

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### 跨源资源共享(CORS)

此Web组件依赖于在目标AEM环境中运行的基于AEM的CORS配置，并假定主机页面运行于 `http://localhost:8080` 在开发模式及以下模式中，是本地AEM Author服务的CORS OSGi配置示例。

请查阅 [部署配置](../deployment/web-component.md) AEM的URL位置。

![CORS配置](assets/react-app/cross-origin-resource-sharing-configuration.png)
