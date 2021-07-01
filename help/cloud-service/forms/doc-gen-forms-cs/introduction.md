---
title: AEM Forms CS中的文档生成微服务
description: 从外部应用程序使用文档生成微服务。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: 文档服务
topic: 开发
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# 简介

在本课程中，我们将使用文档生成微服务通过将数据与XDP模板合并来生成PDF。 要从外部应用程序使用这些微服务，请执行以下步骤：

1. 为AEM技术帐户生成服务凭据
1. 从服务凭据创建JSON Web令牌(JWT)，并将其交换为访问令牌
1. 在AEM中配置技术帐户的访问权限
1. 使用访问令牌进行HTTP调用
