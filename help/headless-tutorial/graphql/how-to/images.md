---
title: 在AEM Headless中使用优化的图像
description: 了解如何使用AEM Headless请求优化的图像URL。
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

# 使用AEM Headless优化图像 {#images-with-aem-headless}

图像是 [开发丰富、引人注目的AEM headless体验](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans). AEM Headless支持管理图像资源及其优化交付。

AEM Headless内容建模中使用的内容片段，通常引用要在Headless体验中显示的图像资产。 可以编写AEM GraphQL查询，以根据从何处引用图像的位置向图像提供URL。

此 `ImageRef` 对于内容引用，类型有四个URL选项：

+ `_path` 是AEM中的引用路径，不包含AEM原点（主机名）
+ `_dynamicUrl` 是首选的Web优化图像资产的完整URL。
   + 此 `_dynamicUrl` 不包含AEM源，因此域（AEM创作或AEM发布服务）必须由客户端应用程序提供。
+ `_authorUrl` 是AEM作者上图像资产的完整URL
   + [AEM创作](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 可用于提供headless应用程序的预览体验。
+ `_publishUrl` 是AEM发布中图像资产的完整URL
   + [AEM发布](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 通常是Headless应用程序的生产部署从中显示图像的地方。

此 `_dynamicUrl` 是用于图像资产的首选URL，应当取代的 `_path`， `_authorUrl`、和 `_publishUrl` 尽可能地。

|  | AEM as a Cloud Service | AEMAS A CLOUD SERVICERDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 支持Web优化图像？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="AEM Headless 图像"
>abstract="了解 AEM Headless 如何支持图像资产的管理及其优化交付。"

## 内容片段模型

确保包含图像引用的内容片段字段属于 __内容引用__ 数据类型。

字段类型可在 [内容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)，通过选择字段，并检查 __属性__ 在右边按Tab键。

![包含对图像的内容引用的内容片段模型](./assets/images/content-fragment-model.jpeg)

## GraphQL持久查询

在GraphQL查询中，将字段返回为 `ImageRef` 类型，并请求 `_dynamicUrl` 字段。 例如，在 [WKND站点项目](https://github.com/adobe/aem-guides-wknd) 并将图像资产引用的图像URL包含在其中 `primaryImage` 字段，可使用新的持久查询完成 `wknd-shared/adventure-image-by-path` 定义为：

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

此 `$path` 中使用的变量 `_path` 过滤器需要内容片段的完整路径(例如 `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`)。

此 `_assetTransform` 定义 `_dynamicUrl` 构建用于优化提供的图像演绎版。 Web优化图像URL也可以通过更改URL的查询参数在客户端进行调整。

| GraphQL参数 | URL 参数 | 描述 | 必填 | GraphQL变量值 | URL参数值 | 示例URL参数 |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | 不适用 | 图像资源的格式。 | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | 不适用 | 不适用 |
| `seoName` | 不适用 | URL中文件段的名称。 如果未提供，则使用图像资源名称。 | ✘ | 字母数字， `-`，或 `_` | 不适用 | 不适用 |
| `crop` | `crop` | 裁切框架从图像中删除，必须在图像大小范围内 | ✘ | 定义原始图像尺寸范围内的裁切区域的正整数 | 数字坐标的逗号分隔字符串 `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | 输出图像的大小（高度和宽度），以像素为单位。 | ✘ | 正整数 | 顺序中以逗号分隔的正整数 `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | 图像的旋转（度）。 | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | 翻转图像。 | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | 图像质量，以原始质量的百分比表示。 | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | 输出图像的宽度（像素）。 时间 `size` 提供 `width` 将被忽略。 | ✘ | 正整数 | 正整数 | `?width=1600` |
| `preferWebP` | `preferwebp` | 如果 `true` 如果浏览器支持WebP，则AEM将为WebP提供服务，而不考虑 `format`. | ✘ | `true`、`false` | `true`、`false` | `?preferwebp=true` |

## GraphQL响应

生成的JSON响应包含所请求的字段，这些字段包含图像资产的Web优化URL。

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

要加载应用程序中引用图像的Web优化图像，请使用 `_dynamicUrl` 的 `primaryImage` 作为图像的源URL。

在React中，从AEM Publish显示Web优化图像的方式如下所示：

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

记住， `_dynamicUrl` 不包括AEM域，因此您必须提供所需的源才能解析图像URL。

## 响应式URL

上例显示了使用单一大小的图像，但在Web体验中，通常需要响应式图像集。 可以使用实现响应式图像 [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 或 [图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). 以下代码片段显示了如何使用 `_dynamicUrl` 作为基础，并附加不同的宽度参数，以支持不同的响应视图。 不仅可以 `width` 查询参数可以使用，但客户端可以添加其他查询参数，以根据其需求进一步优化图像资产。

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

## React示例

让我们创建一个简单的React应用程序，它按以下步骤显示Web优化图像 [响应式图像模式](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). 响应式图像有两种主要模式：

+ [带有srcset的Img元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 提高性能
+ [图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 用于设计控制

### 带有srcset的Img元素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[带有srcset的Img元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 与 `sizes` 属性以为不同的屏幕大小提供不同的图像资源。 为不同屏幕大小提供不同的图像资源时，图像源非常有用。

### 图片元素

[图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 与多个一起使用 `source` 元素，为不同的屏幕大小提供不同的图像资源。 当为不同屏幕大小提供不同的图像呈现时，图片元素很有用。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### 示例代码

这个简单的React应用程序使用 [AEM Headless SDK](./aem-headless-sdk.md) 查询AEM Headless API以获取冒险内容，并使用以下方法显示Web优化图像 [带有srcset的img元素](#img-element-with-srcset) 和 [图片元素](#picture-element). 此 `srcset` 和 `sources` 使用自定义 `setParams` 用于将Web优化投放查询参数附加到 `_dynamicUrl` 因此，请根据Web客户端的需求更改交付的图像演绎版。

在自定义React挂接中执行针对AEM的查询 [使用AEM Headless SDK的useAdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries).

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
