---
title: 建立歡迎套件元件
description: 建立AEM網站頁面，包含根據提交的表單資料下載資產的連結。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 66496f0e-c121-4b6d-b371-084393ece3ca
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '74'
ht-degree: 0%

---

# 歡迎套件元件

已建立頁面元件，以列出頁面中可由一般使用者下載的資產。 個別資產的路徑會儲存在名為的屬性中 **路徑**. 提交的表單資料會決定要包含的資產。

下列程式碼會列出頁面上的資產：

```html
   <p class="cmp-press-kit__press-kit-size">
        Welcome kit contains ${pressKit.assets.size} assets.
    </p>
<ul class="cmp-press-kit__asset-list" data-sly-list.asset="${pressKit.assets}">
    <li class="cmp-press-kit__asset-item">
        <div class="cmp-press-kit__asset " >
            <div class="cmp-press-kit__asset-content">
                <img class="cmp-press-kit__asset-image" src="${asset.path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png" alt="${asset.name}"/>
                <p class="cmp-press-kit__asset-title">${asset.title}</p>
            </div>
            <div class="cmp-press-kit__asset-actions">
                <a class="cmp-press-kit__asset-download-button" href="${asset.path}">Download</a>
            </div>
        </div>
    </li>
</ul>
</sly>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!ready}"></sly>
```
