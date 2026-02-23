---
title: 为IC文档创建表单数据模型
description: 了解如何在AEM Forms中创建表单数据模型，以动态检索交互式通信文档的数据。
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# 为IC文档创建表单数据模型

创建Forms数据模型以将外部数据源与Adobe AEM中的交互式通信集成。 此过程包括设置RESTful服务、上传Swagger文件以及配置服务端点以动态检索和绑定数据。 了解如何安全地连接到外部服务并测试模型以确保成功的数据检索。

实现了一个模拟API服务器，用于模拟Orders服务以用于开发和测试目的。 它公开端点以提取给定用户的订单（例如，通过用户ID），在与生产API相同的架构中返回预定义或动态生成的订单数据。

创建表单数据模型时使用的swagger文件可以从此处[下载](assets/UsersAndOrders.json)

>[!VIDEO](https://video.tv.adobe.com/v/3480005/?learn=on&enablevpops)

## 后续步骤

[创建模板](./create-template.md)
