---
title: 在Forms CS中使用调用DDX操作进行PDF操作
description: 使用调用DDX来组合PDF文件。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# 简介

在本课程中，我们将使用Forms CS对PDF文档进行PDF处理和存档。 要从外部应用程序使用这些微服务，请执行以下步骤：

1. 为AEM技术帐户生成服务凭据
1. 从服务凭据创建JSON Web令牌(JWT)，并将其交换为访问令牌
1. 在AEM中配置技术帐户的访问权限
1. 使用访问令牌进行HTTP调用以处理PDF文件/生成并验证PDF/A文件
