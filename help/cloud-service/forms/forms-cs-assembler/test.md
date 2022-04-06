---
title: 测试解决方案
description: 运行ExecuteAssemblerService.java以测试解决方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# 导入Eclipse项目

* 下载并解压缩 [zip文件](./assets/pdf-manipulation.zip)
* 启动Eclipse并将项目导入Eclipse
* 项目在资源文件夹中包含以下文件夹：
   * ddxFiles — 此文件夹包含用于描述要生成的输出的ddx文件
   * pdffiles — 此文件夹包含要组合的pdf文件和用于测试PDFA实用工具的pdf文件
   * 凭据 — 此文件夹包含pdfa-options.json文件

![资源文件](./assets/resources.png)

## 测试组合PDF文件

* 将您的服务凭据复制并粘贴到项目的service_token.json资源文件中。
* 打开AssemblePDFFiles.java文件，并指定要在其中保存所生成PDF文件的文件夹
* 打开ExecuteAssemblerService.java。 设置变量的值 _AEM_FORMS._CS_ 来指向您的实例。
* 取消相应行的注释以测试组装两个或更多PDF文件
* 将ExecuteAssemblerService.java作为Java应用程序运行

### 测试PDFA实用程序

* 将您的服务凭据复制并粘贴到项目的service_token.json资源文件中。
* 打开PDFAUtilities.java文件，并指定要在其中保存生成的PDF文件的文件夹。
* 打开ExecuteAssemblerService.java。 设置变量的值 _AEM_FORMS._CS_ 来指向您的实例。
* 取消相应行的注释以测试PDFA操作。
* 将ExecuteAssemblerService.java作为Java应用程序运行。



>[!NOTE]
> 第一次运行java程序时，您将收到HTTP 403错误。 要通过，请确保将 [在AEM中授予技术帐户用户的适当权限](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms用户** 是我在本课程中所用的角色。
