---
title: 使用AEM Headless SDK
description: 了解如何使用AEM Headless SDK进行GraphQL查询。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
duration: 186
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 6%

---

# AEM Headless SDK

AEM Headless SDK是一组库，客户端可以使用它通过HTTP与AEM Headless API快速轻松地交互。

AEM Headless SDK适用于各种平台：

+ [适用于客户端浏览器的 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [适用于服务器端/Node.js 的 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [适用于 Java™ 的 AEM Headless SDK](https://github.com/adobe/aem-headless-client-java)

## 持久 GraphQL 查询

使用GraphQL通过持久查询查询AEM(与 [客户端定义的GraphQL查询](#graphl-queries))允许开发人员在AEM中保留查询（但不保留其结果），然后请求按名称执行查询。 持久查询与SQL数据库中存储过程的概念类似。

持久查询比客户端定义的GraphQL查询性能更高，因为持久查询使用HTTPGET执行，该HTTP查询可在CDN和AEM Dispatcher层缓存。 持久查询也有效，定义了API，并降低了开发人员了解每个内容片段模型详细信息的需要。

### 代码示例{#persisted-graphql-queries-code-examples}

以下代码示例介绍了如何对AEM执行GraphQL持久查询。

+++ JavaScript示例

安装 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 通过运行 `npm install` Node.js项目的根目录中的命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代码示例说明如何使用查询AEM [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm模块使用 `async/await` 语法。 适用于JavaScript的AEM Headless SDK还支持 [Promise语法](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

此代码假定一个名为的持久查询 `wknd/adventureNames` 已在AEM Author上创建，并已发布到AEM Publish。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(..) 示例

安装 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 通过运行 `npm install` React项目的根目录中的命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代码示例说明如何使用 [React useEffect(..) 挂钩](https://reactjs.org/docs/hooks-effect.html) 以执行AEM GraphQL的异步调用。

使用 `useEffect` 在React中进行异步GraphQL调用很有用，因为：

1. 它为AEM的异步调用提供同步包装器。
1. 它减少了不必要的请求AEM。

此代码假定一个名为的持久查询 `wknd-shared/adventure-by-slug` 已在AEM Author上创建，并使用GraphiQL发布到AEM Publish。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
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

  return { data, err };
}

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

调用自定义React `useEffect` 从React组件的其他位置挂接。

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

新建 `useEffect` 可以为React应用程序使用的每个持久查询创建挂接。

+++

<p> </p>

## GraphQL查询

AEM支持客户端定义的GraphQL查询，但使用AEM是最佳实践 [持久GraphQL查询](#persisted-graphql-queries).

## Webpack 5+

AEM Headless JS SDK依赖于 `util` 默认情况下，Webpack 5+中未包含。 如果您使用的是Webpack 5+，则会收到以下错误：

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

添加以下内容 `devDependencies` 敬您的 `package.json` 文件：

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

然后运行 `npm install` 以安装依赖项。
