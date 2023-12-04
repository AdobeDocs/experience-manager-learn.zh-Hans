---
title: 安装和设置Git
description: 初始化您的本地Git存储库
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 74
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# 安装Git


[安装Git](https://git-scm.com/downloads). 您可以选择默认设置并完成安装过程。
转到命令提示符在git —version中导航到c：\cloudmanager\aem-banking-app类型。 您应该会看到系统上安装的GIT版本

## 初始化本地Git存储库

确保您位于c：\cloudmanager\aem-banking-app文件夹中

```
git init
```

上述命令将作为Git本地存储库初始化项目

```
git add .
```

这会将所有项目文件添加到Git存储库中，以便提交到Git存储库

```
git commit -m "initial commit"
```

这会将文件提交到Git存储库



## 在本地Git存储库中注册Cloud Manager存储库

访问您的Cloud Manager存储库
![访问代表信息](assets/cloud-manager-repo.png)
获取Cloud Manager存储库凭据
![get-credentials](assets/cloud-manager-repo1.png)

将用户名保存在配置文件中

```java
git config --global credential.username "gbedekar-adobe-com"
```

将密码保存在配置文件中

```java
git config --global user.password "XXXX"
```

（密码是您的Cloud Manager Git存储库密码）

在本地git存储库中注册cloud manager git存储库。 以下命令将关联 **bankingapp** 使用远程cloud manager git存储库。 您可以使用任何名称，而不是 **bankingapp**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

（确保使用存储库URL）

检查远程存储库是否已注册

```java
git remote -v
```

## 后续步骤

[在IntelliJ中将AEM与AEM项目同步](./intellij-and-aem-sync.md)
