---
title: 在AEMas a Cloud Service中搜索和索引
description: 了解AEMas a Cloud Service的搜索索引、如何转换AEM 6索引定义以及如何部署索引。
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 3%

---

# 搜索和索引

了解AEMas a Cloud Service的搜索索引、如何将AEM 6索引定义转换为与AEMas a Cloud Service兼容，以及如何将索引部署到AEMas a Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## 索引转换工具

![索引转换工具](./assets/index-converter.png)

在重构代码库时，请使用 [索引转换工具](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 将自定义Oak索引定义转换为AEMas a Cloud Service兼容的索引定义。

### 关键活动

* 使用 [Adobe I/O工作流迁移器](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) 用于迁移资产处理工作流以使用Asset compute微服务的工具。
* 设置 [本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 并部署自定义索引。 确保更新的索引是最新的。
* 将更新的代码库部署到AEMas a Cloud Service开发环境，并继续验证。
* 如果修改现成的索引 **始终** 从最新版本上运行的AEMas a Cloud Service环境中复制最新索引定义。 根据需要修改复制的索引定义。