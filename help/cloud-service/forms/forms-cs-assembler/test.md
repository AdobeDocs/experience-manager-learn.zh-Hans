---
title: 测试Forms汇编程序解决方案
description: 运行ExecuteAssemblerService.java以测试解决方案
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
duration: 55
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 导入Eclipse项目

* 下载并解压缩[zip文件](./assets/pdf-manipulation.zip)
* 启动Eclipse并将项目导入Eclipse
* 该项目在资源文件夹中包含以下文件夹：
   * ddxFiles — 此文件夹包含用于描述要生成的输出的ddx文件
   * pdf — 此文件夹包含您要汇编的pdf文件以及用于测试PDFA实用程序的pdf文件
   * 凭据 — 此文件夹包含pdfa-options.json文件

![资源文件](./assets/resources.png)

## 测试组合PDF文件

* 将服务凭据复制并粘贴到项目的service_token.json资源文件中。
* 打开AssemblePDFFiles.java文件，并指定要保存生成的PDF文件的文件夹
* 打开ExecuteAssemblerService.java。 将变量&#x200B;_AEM_FORMS_CS_&#x200B;的值设置为指向您的实例。
* 取消注释相应的行以测试装配两个或多个PDF文件
* 以Java应用程序形式运行ExecuteAssemblerService.java

### 测试PDFA实用程序

* 将服务凭据复制并粘贴到项目的service_token.json资源文件中。
* 打开PDFAUtilities.java文件，并指定要保存生成的PDF文件的文件夹。
* 打开ExecuteAssemblerService.java。 将变量&#x200B;_AEM_FORMS_CS_&#x200B;的值设置为指向您的实例。
* 取消注释相应的行以测试PDFA操作。
* 以Java应用程序形式运行ExecuteAssemblerService.java。



>[!NOTE]
> 第一次运行Java程序时，您会收到HTTP 403错误。 若要解决此问题，请确保在AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem)中为[技术帐户用户授予适当的权限。

**AEM Forms Users**&#x200B;是我在此课程中使用的角色。
