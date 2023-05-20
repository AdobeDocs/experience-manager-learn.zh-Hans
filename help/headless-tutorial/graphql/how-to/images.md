---
title: 搭配AEM Headless使用最佳化的影像
description: 瞭解如何使用AEM Headless請求最佳化的影像URL。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 97a311e043d3903070cd249d993036b5d88a21dd
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 6%

---

# 使用AEM Headless最佳化影像 {#images-with-aem-headless}

影像是 [開發豐富、極具吸引力的AEM Headless體驗](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans). AEM Headless支援管理影像資產及其最佳化傳送。

AEM Headless內容模型中使用的內容片段，通常會參考要在Headless體驗中顯示的影像資產。 可以寫入AEM GraphQL查詢，以根據影像的參考來源提供URL給影像。

此 `ImageRef` type有四個URL選項供內容參照使用：

+ `_path` 是AEM中的參照路徑，不包含AEM來源（主機名稱）
+ `_dynamicUrl` 是偏好、網頁最佳化影像資產的完整URL。
   + 此 `_dynamicUrl` 不包含AEM來源，因此網域（AEM作者或AEM發佈服務）必須由使用者端應用程式提供。
+ `_authorUrl` 是AEM作者上影像資產的完整URL
   + [AEM作者](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 可用來提供headless應用程式的預覽體驗。
+ `_publishUrl` 是AEM發佈上影像資產的完整URL
   + [AEM發佈](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 通常是Headless應用程式的生產部署顯示影像的位置。

此 `_dynamicUrl` 是用於影像資產的慣用URL，應取代使用 `_path`， `_authorUrl`、和 `_publishUrl` 儘可能使用。

|  | AEM as a Cloud Service | AEMAS A CLOUD SERVICERDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 支援Web最佳化的影像？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="AEM Headless 图像"
>abstract="了解 AEM Headless 如何支持图像资产的管理及其优化交付。"

## 内容片段模型

確保包含影像參考的內容片段欄位屬於 __內容參考__ 資料型別。

您可在下列欄位型別中進行檢閱： [內容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)，方法是選取欄位，然後檢查 __屬性__ 標籤在右側。

![含有影像內容參照的內容片段模型](./assets/images/content-fragment-model.jpeg)

## GraphQL持續查詢

在GraphQL查詢中，將欄位傳回為 `ImageRef` 型別並要求 `_dynamicUrl` 欄位。 例如，查詢以下位置中的冒險活動： [wknd網站專案](https://github.com/adobe/aem-guides-wknd) 並將影像資產參考的影像URL加入其 `primaryImage` 欄位，可使用新的持續查詢完成 `wknd-shared/adventure-image-by-path` 定義為：

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### 查询变量

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

此 `$path` 變數用於 `_path` 篩選器需要內容片段的完整路徑(例如 `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`)。

此 `_assetTransform` 定義如何 `_dynamicUrl` 旨在最佳化提供的影像轉譯。 您也可以變更URL的查詢引數，在使用者端上調整Web最佳化的影像URL。

| GraphQL引數 | URL 参数 | 描述 | 必填 | GraphQL變數值 | URL引數值 | 範例URL引數 |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | 不适用 | 影像資產的格式。 | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | 不适用 | 不适用 |
| `seoName` | 不适用 | URL中的檔案區段名稱。 若未提供，則會使用影像資產名稱。 | ✘ | 英數字元， `-`，或 `_` | 不适用 | 不适用 |
| `crop` | `crop` | 裁切框架從影像中取出，必須在影像大小範圍內 | ✘ | 定義原始影像尺寸範圍內裁切區域的正整數 | 數字座標的逗號分隔字串 `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | 輸出影像的大小（高度和寬度），以畫素為單位。 | ✘ | 正整數 | 以逗號分隔的正整數，順序為 `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | 影像的旋轉（以度為單位）。 | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | 翻轉影像。 | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | 影像品質，以原始品質的百分比表示。 | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | 輸出影像的寬度（畫素）。 時間 `size` 已提供 `width` 會忽略。 | ✘ | 正整數 | 正整數 | `?width=1600` |
| `preferWebP` | `preferwebp` | 若 `true` 和AEM會在瀏覽器支援的情況下提供WebP，不論 `format`. | ✘ | `true`、`false` | `true`、`false` | `?preferwebp=true` |

## GraphQL回應

產生的JSON回應包含要求的欄位，其中包含影像資產的網頁最佳化URL。

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

若要載入應用程式中參照影像的Web最佳化影像，請使用 `_dynamicUrl` 的 `primaryImage` 做為影像的來源URL。

在React中，從AEM Publish顯示網頁最佳化影像的外觀如下：

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

請記住， `_dynamicUrl` 不包含AEM網域，因此您必須提供所需的原點供影像URL解析。

## 回應式URL

上述範例顯示使用單一大小的影像，不過在網頁體驗中，通常需要回應式影像集。 回應式影像可透過以下方式實作： [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 或 [圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). 下列程式碼片段會示範如何使用 `_dynamicUrl` 為基礎，並附加不同的寬度引數，以支援不同的回應式檢視。 不只可以 `width` 查詢引數可供使用，但使用者端可新增其他查詢引數，以根據其需求進一步最佳化影像資產。

```javascript
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

## React範例

讓我們建立簡單的React應用程式，以下顯示網頁最佳化的影像 [回應式影像模式](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). 回應式影像有兩種主要模式：

+ [具有srcset的Img元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 提升效能
+ [圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 設計控制項

### 具有srcset的Img元素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[具有srcset的Img元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 搭配 `sizes` 屬性以為不同的熒幕大小提供不同的影像資產。 針對不同的熒幕大小提供不同的影像資產時，影像畫面集相當實用。

### 圖片元素

[圖片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 與多個一起使用 `source` 元素，針對不同的熒幕大小提供不同的影像資產。 為不同的熒幕大小提供不同的影像轉譯時，圖片元素會很有用。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### 範常式式碼

這個簡單的React應用程式會使用 [AEM Headless SDK](./aem-headless-sdk.md) 查詢AEM Headless API以取得Adventure內容，並使用以下方法顯示網頁最佳化的影像： [具有srcset的img元素](#img-element-with-srcset) 和 [圖片元素](#picture-element). 此 `srcset` 和 `sources` 使用自訂 `setParams` 函式以將Web最佳化的傳遞查詢引數附加至 `_dynamicUrl` 的影像轉譯，因此請根據Web使用者端的需求變更傳送的影像轉譯。

在自訂React勾點中執行針對AEM的查詢 [使用AEM Headless SDK的useAdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries).

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
