---
title: React应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 提供了一个React应用程序，用于演示如何使用AEM的GraphQL API查询内容。 用于JavaScript的AEM无头客户端用于执行为应用程序提供动力的GraphQL查询。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 2%

---


# React App

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 提供了一个React应用程序，用于演示如何使用AEM的GraphQL API查询内容。 用于JavaScript的AEM无头客户端用于执行为应用程序提供动力的GraphQL查询。

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

![React Application](./assets/react-screenshot.png)

提供了完整的分步教程 [此处](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html).

## 前提条件 {#prerequisites}

应在本地安装以下工具：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;by.sort=desc&amp;list=0&amp;p.offset=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## AEM要求

应用程序旨在连接到AEM **作者** 或 **发布** 的 [WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest) 已安装。

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=zh-Hans)

我们建议 [将WKND引用站点部署到Cloud Service环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). 使用的本地设置 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 或 [AEM 6.5快速入门Jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) 也可使用。

## 使用方法

1. 克隆 `aem-guides-wknd-graphql` 存储库：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 编辑 `aem-guides-wknd/react-app/.env.development` 并确保 `REACT_APP_HOST_URI` 指向您的target AEM实例。 更新身份验证方法（如果连接到创作实例）。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. 打开终端并运行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. 应加载新的浏览器窗口 [http://localhost:3000](http://localhost:3000)
1. 应用程序上应显示来自WKND引用站点的历险列表。

## 代码

以下是用于为应用程序提供支持的重要文件和代码的简短摘要。 完整代码可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM Headless Client for JavaScript

的 [AEM Headless Client](https://github.com/adobe/aem-headless-client-js) 用于执行GraphQL查询。 AEM Headless Client提供两种执行查询的方法， [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) 和 [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` 对AEM内容执行标准GraphQL查询，这是最常见的查询运行类型。

[持久化查询](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) 是AEM中的一项功能，可缓存GraphQL查询的结果，然后通过GET使结果可用。 持久化查询应用于将反复执行的常见查询。 在此应用程序中，冒险列表是在主屏幕上执行的第一个查询。 这将是一个非常受欢迎的查询，因此应使用持久查询。 `runPersistedQuery` 对保留的查询端点执行请求。

`src/api/useGraphQL.js` 是 [反应效果挂钩](https://reactjs.org/docs/hooks-overview.html#effect-hook) 用于侦听对参数所做的更改 `query` 和 `path`. 如果 `query` 为空，则会根据 `path`. 以下是构建和用于获取数据的AEM Headless客户端的位置。

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### 冒险内容

该应用程序主要显示历险列表，并为用户提供一个单击进入冒险详细信息的选项。

`Adventures.js`  — 显示历险记的卡片列表。  初始状态使用 [持久化查询](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) 其中 [预包装](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) 和WKND引用站点。 端点为 `/wknd/adventures-all`. 用户可以使用以下几个按钮来根据活动试验筛选结果：

```javascript
function filterQuery(activity) {
  return `
    {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: "${activity}"
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
        adventurePrimaryImage {
          ... on ImageRef {
            _path
            mimeType
            width
            height
          }
        }
      }
    }
  }
  `;
}
```

`AdventureDetail.js`  — 显示冒险的详细视图。 根据探险项的路径（从url中解析）进行graphQL查询：

```javascript
//parse the content fragment from the url
const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
...
function adventureDetailQuery(_path) {
  return `{
    adventureByPath (_path: "${_path}") {
      item {
        _path
          adventureTitle
          adventureActivity
          adventureType
          adventurePrice
          adventureTripLength
          adventureGroupSize
          adventureDifficulty
          adventurePrice
          adventurePrimaryImage {
            ... on ImageRef {
              _path
              mimeType
              width
              height
            }
          }
          adventureDescription {
            html
          }
          adventureItinerary {
            html
          }
      }
    }
  }
  `;
}
```

### 环境变量

几个 [环境变量](https://create-react-app.dev/docs/adding-custom-environment-variables) 此项目使用来连接到AEM环境。 默认情况下会连接到运行在http://localhost:4502的AEM创作环境。 如果要更改此行为，请更新 `.env.development` 文件：

* `REACT_APP_HOST_URI=http://localhost:4502`  — 设置为AEM Target主机
* `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`  — 设置GraphQL端点路径
* `REACT_APP_AUTH_METHOD=`  — 首选的身份验证方法。 可选，默认情况下不使用身份验证。
   * `service-token`  — 为云环境PROD使用服务令牌交换
   * `dev-token`  — 将开发令牌用于云环境的本地开发
   * `basic`  — 将用户/传递用于本地开发和本地创作环境
   * 留空不使用验证方法
* `REACT_APP_AUTHORIZATION=admin:admin`  — 设置在连接到AEM创作环境时使用的基本身份验证凭据（仅限开发）。 如果连接到发布环境，则不需要此设置。
* `REACT_APP_DEV_TOKEN`  — 开发令牌字符串。 要连接到远程实例，除了基本身份验证(user:pass)之外，您还可以在云控制台中将载体身份验证与DEV令牌结合使用
* `REACT_APP_SERVICE_TOKEN`  — 服务令牌文件的路径。 要连接到远程实例，还可以使用服务令牌（从云控制台下载文件）完成身份验证

### 代理API请求

使用WebPack开发服务器(`npm start`)项目依赖于 [代理设置](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用 `http-proxy-middleware`. 文件配置在 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) 并依赖于 `.env` 和 `.env.development`.

如果连接到AEM创作环境，则需要配置相应的身份验证方法。

### CORS — 跨域资源共享

此项目依赖于在目标AEM环境中运行的CORS配置，并假定应用程序在开发模式下在http://localhost:3000上运行。 的 [COR配置](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) 是 [WKND参考站点](https://github.com/adobe/aem-guides-wknd).

![CORS配置](assets/cross-origin-resource-sharing-configuration.png)

*创作环境的CORS配置示例*