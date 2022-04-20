---
title: 使用AEM Headless SDK
description: 了解如何使用AEM无头SDK进行GraphQL查询。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 5%

---


# AEM Headless SDK

AEM Headless SDK是一组库，客户可以使用这些库通过HTTP快速轻松地与AEM Headless API交互。

AEM Headless SDK可用于各种平台：

+ [适用于客户端浏览器的AEM Headless SDK(JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [AEM Headless SDK for server-side/Node.js(JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [AEM Headless SDK for Java™](https://github.com/adobe/aem-headless-client-java)

## GraphQL查询

使用GraphQL使用查询查询AEM(而不是 [持久GraphQL查询](#persisted-graphql-queries))允许开发人员在代码中定义查询，准确指定要从AEM请求的内容。

GraphQL查询的性能往往比持久查询低，因为它们使用HTTPPOST执行，这在CDN和AEM Dispatcher层的缓存能力较差。

### 代码示例{#graphql-queries-code-examples}

以下是如何对AEM执行GraphQL查询的代码示例。

+++ JavaScript示例

安装 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 运行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代码示例显示如何使用 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm模块使用 `async/await` 语法。 适用于JavaScript的AEM Headless SDK还支持 [Promise语法](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
```

+++


+++ React useEffect(..) 示例

安装 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 运行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代码示例显示如何使用 [React useEffect(..) 钩钩](https://reactjs.org/docs/hooks-effect.html) 执行对AEM GraphQL的异步调用。

使用 `useEffect` 在React中进行异步GraphQL调用非常有用，因为它：

1. 为对AEM的异步调用提供同步包装器。
1. 减少不必要的请求AEM。

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

导入和使用 `useGraphQL` 挂接以查询AEM。

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## 持久 GraphQL 查询

使用GraphQL使用持久查询查询查询AEM(而不是 [常规GraphQL查询](#graphl-queries))允许开发人员在AEM中保留查询（但不保留其结果），然后按名称请求执行查询。 持久化查询与SQL数据库中存储过程的概念类似。

持久化查询往往比常规GraphQL查询更具性能，因为持久化查询是使用HTTPGET执行的，这在CDN和AEM Dispatcher层中更具有缓存能力。 持久化查询也有效，可定义API，并让开发人员不再需要了解每个内容片段模型的详细信息。

### 代码示例{#persisted-graphql-queries-code-examples}

以下是如何针对AEM执行GraphQL持久查询的代码示例。

+++ JavaScript示例

安装 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 运行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代码示例显示如何使用 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm模块使用 `async/await` 语法。 适用于JavaScript的AEM Headless SDK还支持 [Promise语法](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

此代码假定具有名称的持久查询 `wknd/adventureNames` 已在AEM作者上创建并发布到AEM发布。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
```

+++

+++ React useEffect(..) 示例

安装 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 运行 `npm install` 命令。

```
$ npm i @adobe/aem-headless-client-js
```

此代码示例显示如何使用 [React useEffect(..) 钩钩](https://reactjs.org/docs/hooks-effect.html) 执行对AEM GraphQL的异步调用。

使用 `useEffect` 在React中进行异步GraphQL调用非常有用，因为：

1. 它为对AEM的异步调用提供同步包装器。
1. 它减少了不必要的AEM请求。

此代码假定具有名称的持久查询 `wknd/adventureNames` 已在AEM作者上创建并发布到AEM发布。

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

并从React代码的其他位置调用此代码。

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
