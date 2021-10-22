---
title: 将AEM项目推送到cloud manager存储库
description: 将本地git存储库推送到Cloud Manager存储库
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---


# 将AEM项目推送到cloud manager git存储库

在上一步中，我们将AEM项目与在AEM实例中创建的自适应Forms和主题同步。
现在，我们需要将这些更改添加到我们的本地git存储库，然后将本地git存储库推送到cloud manager git存储库。
打开命令提示符并导航到c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .**
```

这会将新文件添加到本地git存储库的阶段分支

```
git commit -m "My First AF"
```

这会将文件提交到我们本地git存储库的主控分支

```
git push -f bankingapp master:"MyFirstAF"
```

在以上命令中，我们将我们的主控分支从本地git存储库推送到由分支应用程序友好名称标识的云管理器存储库的MyFirstAF分支。



