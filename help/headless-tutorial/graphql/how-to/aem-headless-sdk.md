---
title: 使用AEM Headless SDK
description: 瞭解如何使用AEM Headless SDK進行GraphQL查詢。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 31948793786a2c430533d433ae2b9df149ec5fc0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 10%

---

# AEM Headless SDK

AEM Headless SDK是一組程式庫，使用者端可使用這些程式庫，透過HTTP快速輕鬆地與AEM Headless API互動。

AEM Headless SDK適用於各種平台：

+ [适用于客户端浏览器的 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [适用于服务器端/Node.js 的 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [适用于 Java™ 的 AEM Headless SDK](https://github.com/adobe/aem-headless-client-java)

## 持久 GraphQL 查询

使用GraphQL透過持續查詢查詢AEM (與 [使用者端定義的GraphQL查詢](#graphl-queries))可讓開發人員在AEM中保留查詢（但不保留其結果），然後要求依名稱執行查詢。 持久查詢與SQL資料庫中預存程式的概念類似。

持續查詢的效能比使用者端定義的GraphQL查詢更高，因為持續查詢是使用HTTPGET執行的，這可在CDN和AEM Dispatcher層級快取。 持續查詢也會生效，定義API，並解除開發人員瞭解每個內容片段模式詳細資訊的需求。

### 程式碼範例{#persisted-graphql-queries-code-examples}

以下是如何對AEM執行GraphQL持續查詢的程式碼範例。

+++ JavaScript範例

安裝 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 藉由執行 `npm install` Node.js專案根目錄的命令。

```
$ npm i @adobe/aem-headless-client-js
```

此程式碼範例說明如何使用查詢AEM [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm模組使用 `async/await` 語法。 適用於JavaScript的AEM Headless SDK也支援 [Promise語法](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

此程式碼會假設一個名為的持久查詢 `wknd/adventureNames` 已在AEM Author上建立並發佈至AEM Publish。

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

+++ React useEffect(..) 範例

安裝 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 藉由執行 `npm install` React專案根目錄的命令。

```
$ npm i @adobe/aem-headless-client-js
```

此程式碼範例說明如何使用 [React useEffect(..) 鉤點](https://reactjs.org/docs/hooks-effect.html) 執行非同步呼叫AEM GraphQL。

使用 `useEffect` 在React中進行非同步GraphQL呼叫很有用，因為：

1. 它為AEM的非同步呼叫提供同步包裝函式。
1. 它減少了不必要的AEM請求。

此程式碼會假設一個名為的持久查詢 `wknd-shared/adventure-by-slug` 已在AEM Author上建立並使用GraphiQL發佈至AEM Publish。

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

叫用自訂React `useEffect` 從React元件的其他地方進行連結。

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

新增 `useEffect` 可為React應用程式使用的每個持續查詢建立鉤點。

+++

<p> </p>

## GraphQL查詢

AEM支援使用者端定義的GraphQL查詢，不過使用AEM是最佳實務 [持續的GraphQL查詢](#persisted-graphql-queries).

## Webpack 5+

AEM Headless JS SDK有相依於 `util` Webpack 5+預設未包含。 如果您使用Webpack 5+，並收到以下錯誤：

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

新增下列專案 `devDependencies` 至您的 `package.json` 檔案：

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

然後執行 `npm install` 以安裝相依性。
