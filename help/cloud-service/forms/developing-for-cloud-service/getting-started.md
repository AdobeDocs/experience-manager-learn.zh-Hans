---
title: 安装先决条件
description: 安装必要的软件以设置开发环境
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 44
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 1%

---

# 安装所需的软件

本教程将指导您完成创建AEM Forms项目、使用IntelliJ和repo工具将AEM Forms项目与本地AEM实例同步所需的步骤。 您还将了解如何将项目添加到本地Git存储库并将本地Git存储库推送到Cloud Manager存储库。





本教程将引用此文件夹结构。

* [安装JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows)。 我已下载jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html)。例如，如果您在c：\maven文件夹中安装了Maven，则需要创建一个名为M2_HOME的环境变量，其值为C:\maven\apache-maven-3.6.0。然后将M2_HOME\bin添加到路径并保存设置。

## 使用AEM项目原型创建Maven项目

* 在c驱动器中创建一个名为&#x200B;**cloudmanager**&#x200B;的文件夹（您可以为其指定任何名称）
* 打开命令提示符窗口并导航到&#x200B;**c：\cloudmanager**
* 在命令提示符窗口中复制并粘贴[文本文件](assets/creating-maven-project.txt)的内容。 您可能需要根据[最新版本](https://github.com/adobe/aem-project-archetype/releases)更改DarchetypeVersion=30。 在撰写本文时，最新版本是30版。
* 按Enter键执行命令。如果一切运行正常，您应会看到生成成功消息。

## 后续步骤

[安装IntelliJ](./intellij-set-up.md)