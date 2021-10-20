---
title: 安装先决条件
description: 安装必要的软件以设置开发环境
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---


# 安装所需软件

本教程将指导您完成创建AEM Forms项目、使用IntelliJ和repo工具将AEM Forms项目与本地AEM实例同步所需的步骤。 您还将了解如何将您的项目添加到本地git存储库，以及如何将本地git存储库推送到cloud manager存储库。

在c驱动器上创建以下文件夹结构
**c:\cloudmanager\adoberepo**

本教程将参考此文件夹结构。

* [安装JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). 我已下载了jdk-11.0.6_windows-x64_bin.zip
* [马文](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).例如，如果您在c:\maven folder中安装了Maven，则需要创建一个名为M2_HOME的环境变量，其值为C:\maven\apache-maven-3.6.0。然后，将M2_HOME\bin添加到路径中，并保存您的设置

## 使用AEM项目原型创建Maven项目

* 创建名为 **cloudmanager**（可以给它任何名称）
* 打开命令提示符窗口并导航到 **c:\cloudmanager**
* 在命令提示符窗口中复制并粘贴文本文件的内容(assets/creating-maven-project.txt)。 您可能需要根据 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 最新版本是撰写本文时的30个版本。

* 按Enter键执行命令。  如果一切正常，您应会看到生成成功消息




