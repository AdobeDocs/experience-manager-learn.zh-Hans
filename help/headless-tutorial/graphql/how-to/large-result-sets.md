---
title: 如何在AEM Headless中使用大型结果集
description: 了解如何使用AEM Headless处理大型结果集。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
duration: 439
source-git-commit: d7f3c5193cc53f050d24dd66705a3979fb710c36
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 1%

---

# AEM Headless中的大型结果集

AEM Headless GraphQL查询可能会返回大量结果。 本文介绍了如何在AEM Headless中使用大型结果以确保应用程序的最佳性能。

AEM Headless支持 [offset/limit](#list-query) 和 [基于光标的分页](#paginated-query) 查询到较大结果集的较小子集。 可以提出多个请求来收集所需数量的结果。

下面的示例使用结果的小子集（每个请求四个记录）来演示这些技术。 在实际的应用程序中，您将为每个请求使用更多的记录以提高性能。 很好的基线是每个请求50条记录。

## 内容片段模型

分页和排序可用于任何内容片段模型。

## GraphQL持久查询

处理大型数据集时，可使用偏移和限制以及基于游标的分页来检索数据的特定子集。 但是，这两种技术之间存在一些差异，使得其中一种在某些情况下可能比另一种更合适。

### 偏移/限制

列出查询，使用 `limit` 和 `offset` 提供直接的方法，指定起始点(`offset`)和要检索的记录数(`limit`)。 这种方法允许从完整结果集中的任何位置选择结果的子集，例如跳转到结果的特定页面。 虽然它易于实施，但在处理大量结果时可能会变得缓慢而低效，因为检索许多记录需要扫描所有以前的记录。 当偏移值较高时，此方法还可能导致性能问题，因为它可能需要检索和丢弃许多结果。

#### GraphQL查询

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### 查询变量

```json
{
  "offset": 1,
  "limit": 4
}
```

#### GraphQL响应

生成的JSON响应包含第2、第3、第4和第5最昂贵的冒险。 结果中的前两次冒险具有相同的价格(`4500` 所以 [列表查询](#list-queries) 指定具有相同价格的冒险，然后按标题以升序排序。)

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### 分页查询

基于游标的分页（在分页查询中可用）涉及使用游标（对特定记录的引用）检索下一组结果。 这种方法效率更高，因为它无需扫描所有先前的记录来检索所需的数据子集。 分页查询非常适合迭代从头到中间的某个点或到结尾的大型结果集。 列出查询，使用 `limit` 和 `offset` 提供直接的方法，指定起始点(`offset`)和要检索的记录数(`limit`)。 这种方法允许从完整结果集中的任何位置选择结果的子集，例如跳转到结果的特定页面。 虽然它易于实施，但在处理大量结果时可能会变得缓慢而低效，因为检索许多记录需要扫描所有以前的记录。 当偏移值较高时，此方法还可能导致性能问题，因为它可能需要检索和丢弃许多结果。

#### GraphQL查询

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### 查询变量

```json
{
  "first": 3
}
```

#### GraphQL响应

生成的JSON响应包含第2、第3、第4和第5最昂贵的冒险。 结果中的前两次冒险具有相同的价格(`4500` 所以 [列表查询](#list-queries) 指定具有相同价格的冒险，然后按标题以升序排序。)

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### 下一组分页结果

可以使用获取下一组结果 `after` 参数和 `endCursor` 值。 如果没有更多要获取的结果， `hasNextPage` 是 `false`.

##### 查询变量

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## React示例

以下是演示如何使用的React示例 [偏移和限制](#offset-and-limit) 和 [基于光标的分页](#cursor-based-pagination) 方法。 通常，每个请求的结果数会更多，但就这些示例而言，限制设置为5。

### 偏移和限制示例

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

利用偏移和限制，可以方便地检索和显示结果子集。

#### useEffect挂钩

此 `useEffect` 挂接调用持久查询(`adventures-by-offset-and-limit`)以检索Adventures列表。 查询使用 `offset` 和 `limit` 用于指定起点和要检索的结果数的参数。 此 `useEffect` 在以下情况下调用挂接： `page` 值更改。


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### 组件

组件使用 `useOffsetLimitAdventures` 挂接以检索冒险列表。 此 `page` 值会递增或递减以获取下一组或上一组结果。 此 `hasMore` value用于确定是否应启用“下一页”按钮。

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### 分页示例

![分页示例](./assets/large-results/paginated-example.png)

_每个红色框表示一个离散的分页HTTP GraphQL查询。_

使用基于光标的分页，通过增量收集结果并将其连接到现有结果，可以轻松检索和显示大型结果集。


#### UseEffect挂钩

此 `useEffect` 挂接调用持久查询(`adventures-by-paginated`)以检索Adventures列表。 查询使用 `first` 和 `after` 指定要检索的结果数和游标起始位置的参数。 `fetchData` 不断循环，收集下一组分页结果，直到没有更多可提取的结果为止。

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### 组件

组件使用 `usePaginatedAdventures` 挂接以检索冒险列表。 此 `queryCount` value用于显示为检索Adventures列表而发出的HTTP请求数。

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
