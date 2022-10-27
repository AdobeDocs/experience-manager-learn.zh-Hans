---
title: 基本的Next.js应用程序
description: 一个基本的Next.js应用程序，可显示WKND冒险列表及其详细信息
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
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---


# 基本的Next.js应用程序

此 [Next.js](https://nextjs.org/) 应用程序演示了如何使用AEM GraphQL API使用持久查询来查询内容。 此应用程序会呈现WKND冒险的可过滤内容，并在选择冒险时显示冒险的完整详细信息。

此代码：

+ 连接到AEM发布服务，且不需要身份验证
+ 使用WKND的持久查询： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

要深入查看此Next.js应用程序的构建方式，请查看 [示例Next.js应用程序文档](../example-apps/next-js.md).
