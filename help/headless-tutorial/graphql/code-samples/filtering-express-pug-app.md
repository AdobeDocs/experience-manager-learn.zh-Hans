---
title: 筛选ExpressJS和Pug应用程序
description: 一个简单的ExpressJS/Pug应用程序，可过滤使用内容片段建模的WKND冒险。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---


# 筛选ExpressJS和Pug应用程序

探索AEM无头GraphQL API使用 [ExpressJS](https://expressjs.com/)/[普格](https://pugjs.org/) 应用程序。 此ExpressJS/Pug应用程序会创建一个可按活动类型过滤的WKND冒险列表。

此代码可演示如何使用Adobe [AEM Headless Client for NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) 使用基于Node.js的JavaScript调用持久化GraphQL查询。 此应用程序使用 `wknd-shared/adventures-all` 保留查询以收集所有历险，并派生可用活动类型列表。 当用户选择活动类型时，所选类型将传递到 `wknd-shared/adventures-by-activity` 保留查询，并仅检索指定活动类型的历程的冒险详细信息。

此代码：

+ 连接到AEM发布服务，且不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
