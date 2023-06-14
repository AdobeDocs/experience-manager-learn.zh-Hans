---
title: 部署到开发环境
description: 从Cloud Manager存储库分支部署代码
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# 部署到开发环境

在上一步中，我们将主控分支从本地Git存储库推送到Cloud Manager存储库的MyFirstAF分支。

下一步是将代码部署到开发环境中。
登录Cloud Manager并选择您的项目

选择部署到开发环境，如下所示


![第一步](assets/deploy-first-step1.png)


选择部署管道，如下所示
![第一步](assets/deploy1.png)

选择源代码和相应的Git分支
![第一步](assets/deploy2.png)
确保更新更改

运行管道
![运行管道](assets/run-pipeline.png)

部署代码后，您应该会在AEM Forms的云服务实例中看到更改。

## 后续步骤

[更新maven原型项目](./updating-project-archetype.md)
