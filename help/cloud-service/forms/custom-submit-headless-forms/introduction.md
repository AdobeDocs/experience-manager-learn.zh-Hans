---
title: 将Headless表单提交到自定义提交服务
description: 根据提交的数据自定义您的响应
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 2%

---

# 根据提交的数据自定义响应

在提交表单后，请务必向用户提供有关提交结果的反馈。 提交响应可能包括交易ID，也可能仅包括个性化响应。 为了满足此用例，在AEM Forms中编写自定义提交服务，并将Headless表单提交到此自定义提交服务。

## 先决条件

要成功实施此功能，建议熟悉以下内容

* Git使用体验
* AEM Cloud Manager使用体验
* Maven（本文通过3.8.6版本进行了测试）
* 本地AEM Forms Cloud就绪创作实例
* 访问AEM Forms as Cloud Service环境
* IntelliJ或任何其他IDE


## 后续步骤

[编写自定义提交服务](./custom-submit-service.md)
