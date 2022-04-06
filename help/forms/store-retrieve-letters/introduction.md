---
title: 保存并恢复信件
seo-title: Save and resume letters
description: 了解如何保存和检索草稿信件
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# 简介

交互式通信允许代理准备临时通信以保存部分完成的通信并检索该通信以继续工作。 AEM Forms为您提供 [服务提供商界面](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). 客户应实施此界面以获取保存和恢复功能。

本文使用MySQL数据库来存储信件实例的元数据。 信件数据存储在文件系统中。

以下视频演示了用例：

>[!VIDEO](https://video.tv.adobe.com/v/342129/quality=9)

## 先决条件

您将需要满足以下条件来实施解决方案以满足您的需求

* 使用AEM Forms的工作经验
* AEM Server 6.5，带Forms Add
* 在构建OSGi包时应该很熟悉
