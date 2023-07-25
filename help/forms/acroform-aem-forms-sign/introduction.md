---
title: 包含AEM Forms的Acroforms
description: 本教程介绍如何使用Acroform创建自适应表单并合并数据以获取PDF。 然后，可以发送包含合并数据的PDF以供使用Acrobat Sign进行签名。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.5
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 2%

---


# 从Acroforms创建自适应Forms

组织有多种形式。 其中一些表单在Microsoft Word中创建并转换为PDF。 默认情况下，使用Adobe Reader或Acrobat无法填写这些表单。 要使这些表单可使用Acrobat或Reader填写，我们需要将这些表单转换为Acroform。 Acroform是使用Acrobat创建的表单。 本文介绍了如何从Acroform创建自适应表单，并将数据合并回Acroform中以获取PDF。 也可以使用Acrobat Sign发送包含合并数据的PDF以进行签名。

>[!NOTE]
>
>如果您使用的是AEM Forms 6.5，请使用Automated forms conversion功能。

## 前提条件

* 已安装并配置AEM Forms 6.3或6.4
* 对Adobe Acrobat的访问权限
* 熟悉AEM/AEM Forms。

### 要使此功能在您的系统上正常工作，需要满足以下条件

* 使用下载并部署包 [Felix Web控制台](http://localhost:4502/system/console/bundles)
* [文档服务捆绑包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [下载此包并将其导入AEM](assets/acro-form-aem-form.zip). 此资源包包含从acroform创建XSD的示例工作流和html页面
* 打开 [configMgr](http://localhost:4502/system/console/configMgr)
   * 搜索“Apache Sling服务用户映射器服务”，然后单击以打开属性
   * 单击 `+` 图标（加号）以添加以下服务映射
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 单击“保存”
