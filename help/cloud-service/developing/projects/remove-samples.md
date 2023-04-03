---
title: 从AEM Maven项目中删除示例
description: 了解如何从AEM项目原型生成的AEM项目中清除和删除示例代码。
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

# 从AEM Maven项目中删除示例

了解如何从AEM项目原型生成的AEM项目中清除和删除生成的示例代码。

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

删除 `<div class="helloworld" ...></div>` 从：

```
ui.frontend/src/main/webpack/static/index.html
```

删除 `<helloworld>` 组件实例定义来自：

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
