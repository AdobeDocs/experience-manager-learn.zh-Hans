---
title: AEM Sites的页面差异开发
description: 此视频演示如何为AEM Sites的“页面差异”功能提供自定义样式。
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 2%

---


# 针对页面差异进行开发 {#developing-for-page-difference}

此视频演示如何为AEM Sites的“页面差异”功能提供自定义样式。

## 自定义页面差异样式 {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>此视频将自定义CSS添加到we.Retail客户端库中，因为这些更改应该对客户的AEM Sites项目进行；在下面的示例代码中： `my-project`.

AEM页面差异通过直接加载获得OOTB CSS `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`。

由于CSS的直接加载而不是使用客户端库类别，因此我们必须为自定义样式找到另一个注入点，而此自定义注入点是项目的创作clientlib。

这样做的好处是，允许这些自定义样式重写特定于租户。

### 准备创作clientlib {#prepare-the-authoring-clientlib}

确保项目存 `authoring` 在clientlib，位于 `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### 提供自定义CSS {#provide-the-custom-css}

添加到项目的 `authoring` clientlib, `css.txt` 它指向提供覆盖样式的较少文件。 [由于](https://lesscss.org/) 它有许多便捷的功能，包括本例中利用的类包装，因此更少是首选。

```shell
base=./css

htmldiff.less
```

在创建 `less` 包含样式覆盖的文件， `/apps/my-project/clientlibs/authoring/css/htmldiff.less`并根据需要提供覆盖样式。

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### 通过页面组件包含创作clientlib CSS {#include-the-authoring-clientlib-css-via-the-page-component}

将创作客户端类别直接包含在项目基页的标 `/apps/my-project/components/structure/page/customheaderlibs.html` 签之前， `</head>` 以确保加载样式。

这些样式应限于“编 [!UICONTROL 辑] ” [!UICONTROL 和“] 预览WCM”模式。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

应用了上述样式的差异页面的最终结果将如此（已添加HTML并更改了组件）。

![页面差异](assets/page-diff.png)

## 其他资源 {#additional-resources}

* [下载we.Retail示例站点](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [使用AEM客户端库](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [更少的CSS文档](https://lesscss.org/)
