---
title: Web组件/JS - AEM无头示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此Web组件/JS应用程序演示了如何使用持久化查询来使用AEM GraphQL API查询内容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 4%

---


# Web组件

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此Web组件应用程序演示了如何使用AEM GraphQL API来查询内容，它使用持久化查询来呈现部分UI，这些操作是使用纯JavaScript代码完成的。

![具有AEM Headless的Web组件](./assets/web-component/web-component.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;by.sort=desc&amp;list=0&amp;p.offset=14) (如果连接到本地AEM 6.5或AEM SDK)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM要求

Web组件可与以下AEM部署选项配合使用。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用进行本地设置 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

所有部署都需要 `tutorial-solution-content.zip` 从 [解决方案文件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) 安装和必要 [部署配置](../deployment/web-component.md) 执行。


>[!IMPORTANT]
>
>Web组件旨在连接到 __AEM发布__ 环境中，但是，如果Web组件中提供了身份验证，则可以从AEM创作中源内容 [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 文件。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 导航到 `web-component` 子目录。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 编辑 `.../src/person.js` 要包含AEM连接详细信息的文件：

   在 `aemHeadlessService` 对象，更新 `aemHost` 以指向您的AEM发布服务。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   如果连接到AEM创作服务，请在 `aemCredentials` 对象，提供本地AEM用户凭据。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 打开终端并从 `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. 新的浏览器窗口会打开静态HTML页面，该页面嵌入Web组件的位置为 [http://localhost:8080](http://localhost:8080).
1. 的 _人员信息_ Web组件显示在网页上。

## 代码

以下是Web组件的构建方式、它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及该数据如何呈现的摘要。 完整代码可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Web组件HTML标记

可重用的Web组件（即自定义元素） `<person-info>` 添加到 `../src/assets/aem-headless.html` HTML。 它支持 `host` 和 `query-param-value` 属性来驱动组件的行为。 的 `host` 属性的值覆盖 `aemHost` 值 `aemHeadlessService` 对象 `person.js`和 `query-param-value` 用于选择要呈现的人员。

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web组件实施

的 `person.js` 定义Web组件功能，下面是其中的主要亮点。

#### PersonInfo元素实施

的 `<person-info>` 自定义元素的类对象通过使用 `connectedCallback()` 生命周期方法、附加阴影根、获取GraphQL持久查询和DOM操作以创建自定义元素的内部阴影DOM结构。

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

此Web组件依赖于在目标AEM环境中运行的基于AEM的CORS配置，并假定主机页面在上运行 `http://localhost:8080` 在开发模式下，下面是本地AEM创作服务的CORS OSGi配置示例。

请查阅 [部署配置](../deployment/web-component.md) 对应的AEM服务。

![CORS配置](assets/react-app/cross-origin-resource-sharing-configuration.png)
