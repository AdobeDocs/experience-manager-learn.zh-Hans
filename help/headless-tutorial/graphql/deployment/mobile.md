---
title: AEM无头移动部署
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# AEM无头移动部署

AEM无外设移动部署是适用于iOS、Android等的本机移动设备应用程序。 以无头方式使用AEM中的内容并与其进行交互。

由于未在浏览器上下文中启动与AEM无头API的HTTP连接，因此移动部署需要极少的配置。

## 部署配置

| 移动设备应用程序连接到 | AEM Author | AEM 发布 | AEM预览 |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher过滤器](./dispatcher-fitlers.md) | ✘ | ✔ | ✔ |
| [CORS配置](./cors.md) | ✘ | ✘ | ✘ |
| 图像URL主机名 | ✔ | ✔ | ✔ |

## 移动设备应用程序示例

Adobe提供了iOS和Android移动设备应用程序的示例。


