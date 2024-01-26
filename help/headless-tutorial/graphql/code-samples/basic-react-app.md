---
title: 基本React应用程序
description: 一个基本React应用程序，可显示WKND冒险列表及其详细信息
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 21
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# 基本React应用程序

此 [React](https://reactjs.org/) 应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。 此应用程序呈现WKND冒险的可筛选内容，并在选择一个冒险后显示该冒险的完整详细信息。

此代码：

+ 连接到AEM Publish服务，不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

要更深入地了解如何构建此Next.js应用程序，请查看 [React应用程序示例文档](../example-apps/react-app.md).
