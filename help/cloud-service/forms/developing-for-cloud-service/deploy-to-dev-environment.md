---
title: 部署至開發環境
description: 從您的Cloud Manager存放庫分支部署計畫碼
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: f0beb8b32aa25d6c26243879c9c0e42095488e23
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# 部署至開發環境

在上一步中，我們將主分支從本機Git存放庫推送到Cloud Manager存放庫的MyFirstAF分支。

下一步是將程式碼部署到開發環境。
登入cloud manager並選取您的計畫

選取「部署至開發」，如下所示


![第一步](assets/deploy-first-step1.png)


選取部署管道，如下所示
![第一步](assets/deploy1.png)

選取原始程式碼和適當的Git分支
![第一步](assets/deploy2.png)
請務必更新變更

執行管道
![執行管道](assets/run-pipeline.png)

部署程式碼後，您應該會在AEM Forms的雲端服務執行個體中看到變更。
