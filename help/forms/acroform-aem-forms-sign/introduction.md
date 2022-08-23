---
title: 带有AEM Forms的Acroforms
description: 本教程将介绍如何使用Acroform创建自适应表单，以及如何合并数据以获取PDF。 随后，可以发送包含合并数据的PDF，以便使用Adobe Sign进行签名。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---


# 从Acroforms创建自适应Forms

组织有多种形式。 其中一些表单在Microsoft Word中创建并转换为PDF。 默认情况下，使用Adobe Reader或Acrobat无法填写这些表单。 要使用Acrobat或Reader填写这些表单，我们需要将这些表单转换为Acroform。 Acroforms是使用Acrobat创建的表单。 本文将介绍如何从Acroform创建自适应表单，并将数据合并回Acroform以获取PDF。 还可以发送包含合并数据的PDF，以便使用Adobe Sign进行签名。

>[!NOTE]
>
>如果您使用的是AEM Forms 6.5，请使用Automated forms conversion功能。

## 前提条件

* 已安装和配置AEM Forms 6.3或6.4
* 访问Adobe Acrobat
* 熟悉AEM/AEM Forms。

### 需要满足以下条件才能使此功能在您的系统上正常工作

* 使用 [Felix Web Console](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [将此包下载并导入AEM](assets/acro-form-aem-form.zip). 此包包含用于从Acroform创建XSD的示例工作流和html页面
* 打开 [configMgr](http://localhost:4502/system/console/configMgr)
   * 搜索“Apache Sling服务用户映射器服务”并单击以打开属性
   * 单击 `+` 图标（加号）以添加以下服务映射
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 单击“保存”
