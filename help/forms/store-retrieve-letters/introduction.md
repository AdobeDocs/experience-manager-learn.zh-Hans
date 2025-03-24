---
title: 保存并恢复信件
description: 了解如何保存和检索草稿信件
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 160
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# 简介

交互式通信允许准备临时通信的代理保存部分完成的通信并检索该通信以继续工作。 AEM Forms为您提供[服务提供程序接口](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html)。 客户应实施此界面以获取保存和恢复功能。

本文使用MySQL数据库存储信件实例的元数据。 信件数据存储在文件系统中。

以下视频演示了用例：

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## 先决条件

您需要以下各项来实施解决方案，以满足您的需求

* 使用AEM Forms的工作体验
* 带Forms加载项的AEM Server 6.5
* 在构建OSGi包时应该熟悉
