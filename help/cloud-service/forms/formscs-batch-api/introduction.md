---
title: 在AEM Forms CS中使用批处理API生成文档
description: 配置和触发批处理操作以生成文档。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 14%

---

# 简介

批量请求是指一次生成数十、数百或数千个类似文档的位置。 示例：财务公司可以生成信用卡对帐单以发送给其所有客户。
批处理API（异步API）适用于计划的高吞吐量多文档生成用例。 这些 API 会批量生成文档。例如，每月生成的电话帐单、信用卡对帐单和收益对帐单。

要使用AEM Forms CS批处理操作API，需要以下配置

1. 配置Azure存储帐户
1. 创建Azure存储支持的云配置
1. 创建批处理数据存储配置
1. 执行批处理API

建议您先熟悉[API文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en)，然后再继续使用此教程。
