---
title: 将优化的图像与AEM Headless结合使用
description: 了解如何使用AEM Headless请求优化的图像URL。
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 300
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 4%

---

# 用 AEM Headless 优化的图像 {#images-with-aem-headless}

图像是[开发丰富、引人注目的AEM Headless体验的关键方面](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans)。 AEM Headless支持管理图像资源及其优化交付。

AEM Headless内容建模中使用的内容片段，通常引用要在Headless体验中显示的图像资源。 可以写入AEM GraphQL查询，以根据引用图像的位置向图像提供URL。

`ImageRef`类型有四个URL选项用于内容引用：

+ `_path`是AEM中的引用路径，不包含AEM源（主机名）
+ `_dynamicUrl`是用于图像资产的Web优化投放的URL。
   + `_dynamicUrl`不包含AEM源，因此域(AEM Author或AEM Publish服务)必须由客户端应用程序提供。
+ `_authorUrl`是AEM创作实例上图像资源的完整URL
   + [AEM Author](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html)可用于提供Headless应用程序的预览体验。
+ `_publishUrl`是AEM Publish上图像资源的完整URL
   + [AEM Publish](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html)通常是Headless应用程序的生产部署显示图像的位置。

`_dynamicUrl`是建议用于图像资产投放的URL，应尽可能替换`_path`、`_authorUrl`和`_publishUrl`的使用。

|                                | AEM as a Cloud Service | AEM AS A CLOUD SERVICE RDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 支持Web优化图像？ | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="AEM Headless 图像"
>abstract="了解 AEM Headless 如何支持图像资产的管理及其优化交付。"

## 内容片段模型

确保包含图像引用的内容片段字段为&#x200B;__内容引用__&#x200B;数据类型。

通过选择字段并检查右侧的&#x200B;__属性__&#x200B;选项卡，可在[内容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)中审阅字段类型。

![内容引用了图像的内容片段模型](./assets/images/content-fragment-model.jpeg)

## GraphQL持久查询

在GraphQL查询中，将字段作为`ImageRef`类型返回，并请求`_dynamicUrl`字段。 例如，在[WKND站点项目](https://github.com/adobe/aem-guides-wknd)中查询冒险并在其`primaryImage`字段中包含图像资产引用的图像URL，可以使用新持久查询`wknd-shared/adventure-image-by-path`完成，该查询定义为：

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

`_path`筛选器中使用的`$path`变量需要内容片段的完整路径（例如`/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`）。

`_assetTransform`定义如何构造`_dynamicUrl`以优化提供的图像演绎版。 Web优化图像URL也可通过更改URL的查询参数在客户端进行调整。

| GraphQL参数 | 描述 | 必填 | GraphQL变量值 |
|-------------------|------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------|
| `format` | 图像资源的格式。 | ✔ | `GIF`，`PNG`，`PNG8`，`JPG`，`PJPG`，`BJPG`，`WEBP`，`WEBPLL`，`WEBPLY` |
| `seoName` | URL中文件段的名称。 如果未提供，则使用图像资产名称。 | ✘ | 字母数字、`-`或`_` |
| `crop` | 裁切框架从图像中删除，必须在图像大小范围内 | ✘ | 正整数，定义原始图像尺寸范围内的裁切区域 |
| `size` | 输出图像的大小（高度和宽度），以像素为单位。 | ✘ | 正整数 |
| `rotation` | 图像的旋转（以度为单位）。 | ✘ | `R90`、`R180`、`R270` |
| `flip` | 翻转图像。 | ✘ | `HORIZONTAL`、`VERTICAL`、`HORIZONTAL_AND_VERTICAL` |
| `quality` | 图像质量占原始质量的百分比。 | ✘ | 1-100 |
| `width` | 输出图像的宽度（像素）。 提供`size`时`width`被忽略。 | ✘ | 正整数 |
| `preferWebP` | 如果`true`和AEM在浏览器支持WebP的情况下提供它，而不考虑`format`。 | ✘ | `true`、`false` |


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

要在应用程序中加载所引用图像的Web优化图像，请使用`primaryImage`的`_dynamicUrl`作为图像的源URL。

在React中，显示AEM Publish中的Web优化图像如下所示：

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

请记住，`_dynamicUrl`不包括AEM域，因此您必须提供所需的源位置以便解析图像URL。

## 响应式URL

上例显示了使用单大小图像，但在Web体验中，通常需要响应式图像集。 可以使用[img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)或[图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)实现响应式图像。 以下代码段显示了如何使用`_dynamicUrl`作为基础。 `width`是一个URL参数，您可以将其附加到`_dynamicUrl`以向不同的响应视图供电。

```javascript
// The AEM host is usually read from a environment variable of the SPA.
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

让我们创建一个简单的React应用程序，该应用程序按照[响应图像模式](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/)显示Web优化图像。 响应式图像有两种主要模式：

+ 使用srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)的[Img元素可提高性能
+ 设计控件的[图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture)

### 带有srcset的Img元素

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

带有srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)的[Img元素与`sizes`属性一起使用，为不同的屏幕大小提供不同的图像资源。 为不同屏幕大小提供不同的图像资源时，图像源非常有用。

### 图片元素

[图片元素](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture)与多个`source`元素一起使用，以便为不同的屏幕大小提供不同的图像资源。 当为不同屏幕大小提供不同的图像呈现时，图片元素很有用。

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### 示例代码

这个简单的React应用程序使用[AEM Headless SDK](./aem-headless-sdk.md)查询AEM Headless API以获取冒险内容，并使用带有srcset](#img-element-with-srcset)和[picture element](#picture-element)的[img元素显示Web优化图像。 `srcset`和`sources`使用自定义`setParams`函数将Web优化投放查询参数附加到图像的`_dynamicUrl`，因此请更改根据Web客户端需求投放的图像演绎版。

在使用了AEM Headless SDK](./aem-headless-sdk.md#graphql-persisted-queries)的自定义React挂接[useAdventureByPath中执行针对AEM的查询。

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
