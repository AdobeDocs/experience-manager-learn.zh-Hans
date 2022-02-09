---
title: 在AEM Forms CS中使用批处理API生成文档
description: 配置并触发批处理操作以生成文档。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Communications API

批量请求是指一次生成数以十、百或千个类似文档的位置。 示例：金融公司可以生成信用卡报表，发送给其所有客户。
批处理API（异步API）适用于计划的高吞吐量多文档生成用例。 这些API可批量生成文档。 例如，每月生成的电话账单、信用卡报表和福利报表。

要使用AEM Forms CS批处理操作API，需要以下配置

1. 配置Azure存储帐户
1. 创建Azure存储备份的云配置
1. 创建批量数据存储配置
1. 执行批处理API

建议您熟悉 [API文档](https://experienceleague.corp.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) ，然后再继续使用本教程。




