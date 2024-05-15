---
title: 安装先决条件
description: 安装必要的软件以设置开发环境
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 44
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# 安装所需的软件

本教程将指导您完成创建AEM Forms项目、使用IntelliJ和repo工具将AEM Forms项目与本地AEM实例同步所需的步骤。 您还将了解如何将项目添加到本地Git存储库并将本地Git存储库推送到Cloud Manager存储库。





本教程将引用此文件夹结构。

* [安装JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). 我已下载jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html)例如，如果您在c：\maven文件夹中安装了Maven，则需要创建一个名为M2_HOME的环境变量，其值为C:\maven\apache-maven-3.6.0。然后将M2_HOME\bin添加到路径并保存设置。

## 使用AEM项目原型创建Maven项目

* 创建名为的文件夹 **cloudmanager**（您可以为其指定任何名称）
* 打开命令提示符窗口并导航至 **c：\cloudmanager**
* 复制并粘贴的内容 [文本文件](assets/creating-maven-project.txt) 命令提示符窗口中。 您可能需要更改DarchetypeVersion=30，具体取决于 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 在撰写本文时，最新版本是30版。
* 按Enter键执行命令。如果一切运行正常，您应会看到生成成功消息。

## 后续步骤

[安装IntelliJ](./intellij-set-up.md)