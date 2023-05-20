---
title: 針對AEM Sites中的頁面差異開發
description: 本影片說明如何為AEM Sites的頁面差異功能提供自訂樣式。
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 3%

---

# 針對頁面差異開發 {#developing-for-page-difference}

本影片說明如何為AEM Sites的頁面差異功能提供自訂樣式。

## 自訂頁面差異樣式 {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>本影片將自訂CSS新增至we.Retail使用者端資料庫，而其中的這些變更應該對自訂者的AEM Sites專案進行；在以下範常式式碼中： `my-project`.

AEM頁面差異可透過直接載入OOTB CSS `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

由於這會直接載入CSS而不使用使用者端程式庫類別，因此我們必須找到另一個自訂樣式的插入點，而且此自訂插入點是專案的編寫clientlib。

這樣做的好處是允許這些自訂樣式覆寫為租使用者專用。

### 準備編寫clientlib {#prepare-the-authoring-clientlib}

確儲存在 `authoring` 您專案的clientlib，位於 `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### 提供自訂CSS {#provide-the-custom-css}

新增至專案的 `authoring` clientlib a `css.txt` 會指向將提供覆寫樣式的較少檔案。 [更少](https://lesscss.org/) 由於其許多方便的功能（包括此範例中使用的類別包裝），所以較偏好使用。

```shell
base=./css

htmldiff.less
```

建立 `less` 包含樣式覆蓋的檔案 `/apps/my-project/clientlibs/authoring/css/htmldiff.less`，並視需要提供覆蓋樣式。

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

### 透過頁面元件包含編寫clientlib CSS {#include-the-authoring-clientlib-css-via-the-page-component}

在專案的基本頁面中包含編寫clientlibs類別 `/apps/my-project/components/structure/page/customheaderlibs.html` 直接位於 `</head>` 標籤來確保樣式已載入。

這些樣式應限製為 [!UICONTROL 編輯] 和 [!UICONTROL 預覽] wcm模式。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

套用上述樣式的差異頁面的最終結果看起來像這樣(新增HTML並變更元件)。

![頁面差異](assets/page-diff.png)

## 其他资源 {#additional-resources}

* [下載we.Retail範例網站](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [使用AEM使用者端資料庫](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [較少CSS檔案](https://lesscss.org/)
