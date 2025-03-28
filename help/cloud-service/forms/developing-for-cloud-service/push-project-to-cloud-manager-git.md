---
title: 将AEM项目推送到Cloud Manager存储库
description: 将本地Git存储库推送到Cloud Manager存储库
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# 将AEM项目推送到Cloud Manager Git存储库

在上一步中，我们将AEM项目与在AEM实例中创建的自适应Forms和主题同步。
现在，我们需要将这些更改添加到本地Git存储库，然后将本地Git存储库推送到Cloud Manager Git存储库。
打开命令提示符并导航至c：\cloudmanager\aem-banking-app
执行以下命令

```
git add .
```

这会将新文件添加到本地Git存储库的暂存分支

```
git commit -m "My First AF"
```

这会将文件提交到本地Git存储库的主分支

```
git push -f bankingapp master:"MyFirstAF"
```

在上面命令中，我们将主分支从本地Git存储库推送到Cloud Manager存储库的MyFirstAF分支，该分支由银行应用程序友好名称标识。

## 后续步骤

[将项目部署到开发环境](./deploy-to-dev-environment.md)
