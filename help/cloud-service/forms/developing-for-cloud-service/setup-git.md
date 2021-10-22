---
title: 安装和设置Git
description: 初始化本地git存储库
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 安装Git


[安装Git](https://git-scm.com/downloads). 您可以选择默认设置并完成安装过程。
转到命令提示符，导航到c:\cloudmanager\aem-banking-app type in git —version。 您应会看到系统上安装的GIT版本

## 初始化本地Git存储库

确保您位于c:\cloudmanager\aem-banking-app folder

```
git init
```

以上命令将作为git本地存储库初始化项目

```
git add .
```

这会将所有项目文件添加到git存储库，以便提交到git存储库

```
git commit -m "initial commit"
```

这会将文件提交到git存储库



## 在本地Git存储库中注册Cloud Manager存储库

访问cloud manager存储库
![访问rep信息](assets/cloud-manager-repo.png)
获取cloud manager存储库凭据
![get-credentials](assets/cloud-manager-repo1.png)

在配置文件中保存用户名

```java
git config --global credential.username "gbedekar-adobe-com"
```

在配置文件中保存密码

```java
git config --global user.password "bqwxfvxq2akawtqx3oztacb5wax5a7"
```

（密码是您的cloud manager git存储库密码）

在本地git存储库中注册cloud manager git存储库。 以下命令会关联 **adobe** 与远程cloud manager git存储库。 您可以使用任何名称，而不是 **adobe**


```java
git remote add adobe https://git.cloudmanager.adobe.com/techmarketingdemos/Program2-p24107/
```

（确保使用您的存储库URL）

检查远程存储库是否已注册

```java
git remote -v
```



