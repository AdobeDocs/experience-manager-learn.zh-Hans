---
title: 将优化的图像与AEM Headless结合使用
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
source-git-commit: 09f9530cab0ec651b7c37c8c078631c79e8cfe4a
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 6%

---

# 使用AEM Headless优化图像 {#images-with-aem-headless}

图像是 [开发丰富而引人入胜的AEM无头体验](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans). AEM Headless支持管理图像资产及其优化交付。

AEM无头内容建模中使用的内容片段，通常引用要用于在无头体验中显示的图像资产。 可以编写AEM GraphQL查询，以根据引用图像的位置为图像提供URL。

的 `ImageRef` 类型具有四个用于内容引用的URL选项：

+ `_path` 是AEM中的引用路径，且不包括AEM源（主机名）
+ `_dynamicUrl` 是优化了web的首选图像资产的完整URL。
   + 的 `_dynamicUrl` 不包括AEM源，因此域（AEM创作或AEM发布服务）必须由客户端应用程序提供。
+ `_authorUrl` 是AEM作者上图像资产的完整URL
   + [AEM作者](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 可用于提供无头应用程序的预览体验。
+ `_publishUrl` 是AEM发布中图像资产的完整URL
   + [AEM发布](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) 通常是无头应用程序的生产部署显示图像的位置。

的 `_dynamicUrl` 是用于图像资产的首选URL，应该替换了 `_path`, `_authorUrl`和 `_publishUrl` 尽可能。

|  | AEM as a Cloud Service | AEMas a Cloud ServiceRDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 是否支持Web优化的图像？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="AEM Headless 图像"
>abstract="了解 AEM Headless 如何支持图像资产的管理及其优化交付。"

## 内容片段模型

确保包含图像引用的内容片段字段为 __内容引用__ 数据类型。

在 [内容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)，方法是选择字段并检查 __属性__ 选项卡。

![引用图像的内容片段模型](./assets/images/content-fragment-model.jpeg)

## GraphQL持久查询

在GraphQL查询中，将字段返回为 `ImageRef` 类型，并请求 `_dynamicUrl` 字段。 例如，查询 [WKND站点项目](https://github.com/adobe/aem-guides-wknd) ，并在其中包含图像资产引用的图像URL `primaryImage` 字段，可以使用新的保留查询完成 `wknd-shared/adventure-image-by-path` 定义为：

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

的 `$path` 变量 `_path` 过滤器需要内容片段的完整路径(例如， `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`)。

的 `_assetTransform` 定义如何 `_dynamicUrl` 用于优化提供的图像呈现。 还可以通过更改URL的查询参数，在客户端上调整Web优化图像URL。

| GraphQL参数 | URL 参数 | 描述 | 必填 | GraphQL变量值 | URL参数值 | URL参数示例 |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | 不适用 | 图像资产的格式。 | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | 不适用 | 不适用 |
| `seoName` | 不适用 | URL中文件区段的名称。 如果未提供，则使用图像资产名称。 | ✘ | 字母数字， `-`或 `_` | 不适用 | 不适用 |
| `crop` | `crop` | 从图像中取出的裁剪帧必须在图像大小内 | ✘ | 在原始图像尺寸范围内定义裁剪区域的正整数 | 以逗号分隔的数字坐标字符串 `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | 输出图像的大小（包括高度和宽度）（以像素为单位）。 | ✘ | 正整数 | 以逗号分隔的正整数 `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | 图像的旋转（以度为单位）。 | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | 翻转图像。 | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | 图像质量（以原始质量的百分比表示）。 | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | 输出图像的宽度（以像素为单位）。 When `size` 提供 `width` 将被忽略。 | ✘ | 正整数 | 正整数 | `?width=1600` |
| `preferWebP` | `preferwebp` | 如果 `true` 和AEM在浏览器支持时提供WebP，而不考虑 `format`. | ✘ | `true`、`false` | `true`、`false` | `?preferwebp=true` |

## GraphQL响应

生成的JSON响应包含请求的字段，其中包含到图像资产的Web优化URL。

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

在React中，显示AEM Publish中的Web优化图像如下所示：

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

记住， `_dynamicUrl` 不包括AEM域，因此您必须为要解析的图像URL提供所需的源。

### 响应式URL

上例显示了如何使用单一大小的图像，但在Web体验中，通常需要响应式图像集。 响应式图像可以使用 [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 或 [图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). 以下代码片段显示了如何使用 `_dynamicUrl` 作为基础，并附加不同的宽度参数，以便为不同的响应视图提供动力。 不仅可以 `width` 可使用查询参数，但客户端可以添加其他查询参数，以根据需要进一步优化图像资产。

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
    src="${${dynamicUrl}&width=1000}"
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

### React示例

让我们创建一个简单的React应用程序，在之后显示Web优化的图像 [响应式图像模式](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). 响应式图像有两种主要模式：

+ [包含srcset的IMG元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 提高性能
+ [图像元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 用于设计控制

#### 包含srcset的IMG元素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[包含srcset的Img元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 与 `sizes` 属性，以便为不同的屏幕大小提供不同的图像资产。 当为不同的屏幕大小提供不同的图像资产时，Img Srcsets非常有用。

#### 图像元素

[图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) 与多个 `source` 元素，以便为不同的屏幕大小提供不同的图像资产。 当为不同的屏幕大小提供不同的图像呈现时，图片元素非常有用。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

#### 示例代码

这个简单的React应用程序使用 [AEM Headless SDK](./aem-headless-sdk.md) 查询AEM无头API以获取Adventure内容，并使用 [srcset中的img元素](#img-element-with-srcset) 和 [图像元素](#picture-element). 的 `srcset` 和 `sources` 使用自定义 `setParams` 函数将web优化投放查询参数附加到 `_dynamicUrl` ，因此请根据web客户端的需求更改交付的图像呈现版本。

在自定义React挂接中执行AEM查询 [使用AEM Headless SDK的useAdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries).

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
