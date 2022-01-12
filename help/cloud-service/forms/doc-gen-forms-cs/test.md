---
title: 测试解决方案
description: 运行Main.java以测试解决方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: f712e86600ed18aee43187a5fb105324b14b7b89
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---


# 导入Eclipse项目

下载并解压缩 [zip文件](./assets/aem-forms-cs-doc-gen.zip)

启动Eclipse并将项目导入Eclipse 。该项目在资源文件夹中包含以下文件：

* DataFile1、DataFile2和DataFile3 — 要与模板合并以生成最终PDF文件的示例XML数据文件
* custom_fonts.xdp - XDP模板。
* service_token.json — 您必须将此文件的内容替换为您的帐户特定凭据
* options.json — 此文件中指定的选项用于设置由API生成的PDF文件的属性

![资源文件](./assets/resource-files.png)

## 测试解决方案

* 将您的服务凭据复制并粘贴到项目的service_token.json资源文件中。
* 打开DocumentGeneration.java文件，并指定要在其中保存生成的PDF文件的文件夹
* 打开Main.java。 设置变量postURL的值以指向您的实例。
* 将Main.java作为Java应用程序运行

>[!NOTE]
> 第一次运行java程序时，您将收到HTTP 403错误。 要通过，请确保将 [在AEM中授予技术帐户用户的适当权限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms用户** 是我在本课程中所用的角色。

