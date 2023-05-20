---
title: 從AEM Maven專案移除範例
description: 瞭解如何從AEM專案原型產生的AEM專案中清除和移除範常式式碼。
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 6%

---

# 從AEM Maven專案中移除範例

瞭解如何從AEM專案原型產生的AEM專案中清理及移除產生的範常式式碼。

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## 资源

+ [AEM Maven專案原型](https://github.com/adobe/aem-project-archetype)

## 命令

可以執行以下命令以從AEM Maven專案中移除產生的範例檔案：

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## 編輯

移除 `<div class="helloworld" ...></div>` 從：

```
ui.frontend/src/main/webpack/static/index.html
```

移除 `<helloworld>` 元件實體定義來源：

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
