---
title: 移动到AEMas a Cloud Service时配置Dispatcher
description: 了解对AEM Dispatcher for AEMas a Cloud Service、Dispatcher转换工具以及如何使用Dispatcher Tools SDK的显着更改。
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

了解AEM Dispatcher for AEMas a Cloud Service，重点介绍对Dispatcher for AEM 6、Dispatcher转换工具以及如何使用Dispatcher Tools SDK做出的显着更改。

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

在重构代码库时，请使用 [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 可将现有的内部部署或Adobe Managed Services Dispatcher配置重构为AEMas a Cloud Service兼容的Dispatcher配置。

### 关键活动

* 使用 [Adobe I/ODispatcher Converter工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 迁移现有Dispatcher配置。
* 从 [AEM项目原型](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) 作为最佳实践。
* [设置本地Dispatcher工具](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) 在Cloud Service环境中进行测试之前，验证调度程序。


