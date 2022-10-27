---
title: AEM Forms中的认证文档
description: 使用Docassurance Service验证AEM Forms中的PDF文档
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 1%

---

# AEM Forms中的认证文档

认证文档为PDF文档和表单接收者提供对其真实性和完整性的附加保证。

要验证文档，您可以在桌面上使用Acrobat DC，也可以在AEM Forms Document Services中将Document用作服务器上自动处理的一部分。

本文为您提供了使用AEM Forms Document Services对PDF文档进行认证的OSGI包示例。示例中使用的代码为 [此处提供](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

要使用AEM Forms验证文档，需执行以下步骤

## 将证书添加到信任存储 {#adding-certificate-to-trust-store}

请按照以下所述步骤将证书添加到AEM中的KeyStore

* [初始化全局信任存储](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜索fd-service](http://localhost:4502/security/users.html) 用户
* **您必须滚动结果页才能加载所有用户以查找fd-service用户**
* 双击fd-service用户以打开用户设置窗口
* 单击“从密钥库文件添加私钥”。指定特定于您证书的别名和密码
   ![添加证书](assets/adding-certificate-keystore.PNG)
* 保存更改

## 创建OSGI服务

您可以编写自己的OSGi包，并使用AEM Forms客户端SDK实施服务来验证PDF文档。 以下链接对编写您自己的OSGi包非常有用

* [创建您的第一个OSGi包](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用文档服务API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

或者，您也可以使用作为本教程资产一部分包含的示例包。

>[!NOTE]
>
>示例包使用名为“ares”的别名来验证文档。 因此，在使用此包时，请确保您的别名为“ares”

## 在本地系统上测试示例

* 下载并安装 [自定义文档服务包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下载并安装 [使用服务用户包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [确保已在Apache Sling服务用户映射器服务中添加以下条目](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 如以下屏幕截图所示
   ![用户映射器](assets/user-mapper-service.PNG)
* [导入示例自适应表单](assets/certify-pdf-af.zip)
* [导入并安装自定义提交](assets/custom-submit-certify.zip)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上传需要认证的PDF文档
   **可选**  — 指定要在验证文档时使用的签名字段
* 单击提交。
* 认证PDF应返回给您。
