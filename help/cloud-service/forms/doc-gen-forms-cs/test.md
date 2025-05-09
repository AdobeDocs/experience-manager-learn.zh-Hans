---
title: 测试解决方案
description: 运行Main.java以测试解决方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# 导入Eclipse项目

下载并解压缩[zip文件](./assets/aem-forms-cs-doc-gen.zip)

启动Eclipse并将项目导入Eclipse
该项目在资源文件夹中包含以下文件：

* DataFile1、DataFile2和DataFile3 — 要与模板合并以生成最终PDF文件的示例xml数据文件
* custom_fonts.xdp - XDP模板。
* service_token.json — 必须使用特定于您帐户的凭据替换此文件的内容
* options.json — 在此文件中指定的选项用于设置API生成的PDF文件的属性

![资源文件](./assets/resource-files.png)

## 测试解决方案

* 将服务凭据复制并粘贴到项目的service_token.json资源文件中。
* 打开DocumentGeneration.java文件，并指定要保存生成的PDF文件的文件夹
* 打开Main.java。 将变量postURL的值设置为指向您的实例。
* 将Main.java作为Java应用程序运行

>[!NOTE]
> 第一次运行Java程序时，您会收到HTTP 403错误。 若要解决此问题，请确保在AEM[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=zh-Hans#configure-access-in-aem)中为技术帐户用户授予适当的权限。

**AEM Forms Users**&#x200B;是我在此课程中使用的角色。
