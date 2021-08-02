---
title: 测试解决方案
description: 运行Main.java以测试解决方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: 自适应表单
topic: 开发
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 1%

---


# 导入Eclipse项目

下载并解压缩[zip文件](./assets/aem-forms-doc-gen.zip)

启动Eclipse并将项目导入Eclipse
项目在资源文件夹中包含以下文件：

* DataFile1和DataFile2 — 要与模板合并以生成最终PDF文件的示例XML数据文件
* address.xdp - XDP模板
* service_token.json — 您必须将此文件的内容替换为您的帐户特定凭据
* options.json — 此文件中指定的选项用于设置由API生成的PDF文件的属性

![资源文件](./assets/resource-files.JPG)

## 测试解决方案

* 将您的服务凭据复制并粘贴到项目的service_token.json资源文件中。
* 打开DocumentGeneration.java文件，并指定要在其中保存生成的PDF文件的文件夹
* 打开Main.java。 设置变量postURL的值以指向您的实例。
* 将Main.java作为Java应用程序运行

>[!NOTE]
> 第一次运行java程序时，您将收到HTTP 403错误。 要解决此问题，请确保在AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)中为技术帐户用户授予[适当的权限。

**AEM Forms** Users是我在本课程中使用的角色。

