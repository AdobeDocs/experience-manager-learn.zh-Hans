---
title: AEM Forms
description: 一个教程，其中将逐步介绍如何使用Acroform创建自适应表单并合并数据以获得PDF。 然后，可以使用Adobe Sign发送包含合并数据的PDF进行签名。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---


# 从Acroforms创建自适应Forms

组织有各种各样的表单。 其中一些表单是在Microsoft Word中创建并转换为PDF的。 默认情况下，这些表单不能使用Adobe Reader或Acrobat填写。 要使用Acrobat或Reader填写这些表单，我们需要将这些表单转换为Acroform。 Acroforms是使用Acrobat创建的表单。 本文将逐步介绍如何从Acroform创建自适应表单，并将数据合并回Acroform以获取PDF。 包含合并数据的PDF也可以使用Adobe Sign发送以供签名。

>[!NOTE]
>
>如果您使用的是AEM Forms6.5，请使用Automated forms conversion功能。

## 前提条件

* AEM Forms6.3或6.4的安装和配置
* 访问Adobe Acrobat
* 熟悉AEM/AEM Forms。

### 需要以下各项才能使此功能在您的系统上工作

* 使用[Felix Web控制台](http://localhost:4502/system/console/bundles)下载和部署捆绑套件
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [下载此包并将其导入AEM](assets/acro-form-aem-form.zip)。此包包含从acroform创建XSD的示例工作流和html页
* 打开[configMgr](http://localhost:4502/system/console/configMgr)
   * 搜索“Apache Sling Service User Mapper Service”并单击以打开属性
   * 单击`+`图标（加号）以添加以下服务映射
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 单击“保存”
