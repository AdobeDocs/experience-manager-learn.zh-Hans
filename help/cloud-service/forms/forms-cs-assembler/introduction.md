---
title: Forms CS中的PDF操作（使用调用DDX操作）
description: 使用调用DDX来组合PDF文件。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# 简介

在本课程中，我们将使用Forms CS对PDF文档进行PDF操作和存档。 要从外部应用程序使用这些微服务，需要执行以下步骤：

1. 为AEM技术帐户生成服务凭据
1. 根据服务凭据创建一个JSON Web令牌(JWT)，并将其交换为访问令牌
1. 在AEM中配置技术帐户的访问权限
1. 使用访问令牌进行HTTP调用以处理PDF文件/生成和验证PDF/A文件
