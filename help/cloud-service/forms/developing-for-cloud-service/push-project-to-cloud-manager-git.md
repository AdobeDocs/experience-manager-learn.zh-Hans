---
title: 將AEM專案推送到Cloud Manager存放庫
description: 將本機Git存放庫推送到Cloud Manager存放庫
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# 將AEM專案推送到Cloud Manager Git存放庫

在上一步中，我們將AEM專案與AEM例項中建立的最適化Forms和主題同步。
我們現在需要將這些變更新增到本機Git存放庫，然後將本機Git存放庫推送到Cloud Manager Git存放庫。
開啟命令提示字元並瀏覽至c：\cloudmanager\aem-banking-app執行以下命令

```
git add .
```

這會將新檔案新增至本機Git存放庫的階段分支

```
git commit -m "My First AF"
```

這會將檔案提交至本機Git存放庫的主要分支

```
git push -f bankingapp master:"MyFirstAF"
```

在上述命令中，我們將主分支從本機Git存放庫推送到Cloud Manager存放庫的MyFirstAF分支，該分支由銀行應用程式易記名稱識別。
