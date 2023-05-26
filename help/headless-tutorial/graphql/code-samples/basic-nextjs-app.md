---
title: 基本Next.js应用程序
description: 一个基本Next.js应用程序，可显示WKND冒险列表及其详细信息
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---

# 基本Next.js应用程序

此 [Next.js](https://nextjs.org/) 应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。 此应用程序呈现WKND冒险的可筛选内容，并在选择冒险后显示该冒险的完整详细信息。

此代码：

+ 连接到AEM Publish服务，无需身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

要更深入地了解如何构建此Next.js应用程序，请查看 [示例Next.js应用程序文档](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io不支持在嵌入式IDE中编辑Next.js应用程序。 要编辑此代码示例， [直接在codesandbox.io上打开Next.js应用程序](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
