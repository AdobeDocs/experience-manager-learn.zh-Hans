---
title: 从AEM Maven项目中删除示例
description: 了解如何从AEM项目原型生成的AEM项目中清理和删除示例代码。
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 3%

---

# 从AEM Maven项目中删除示例

了解如何从AEM项目原型生成的AEM项目中清理和删除生成的示例代码。

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## 资源

+ [AEM Maven项目原型](https://github.com/adobe/aem-project-archetype)

## 命令

可以执行以下命令以从AEM Maven项目中删除生成的示例文件：

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

## 编辑

从：`<div class="helloworld" ...></div>`删除

```
ui.frontend/src/main/webpack/static/index.html
```

从以下位置删除`<helloworld>`组件实例定义：

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
