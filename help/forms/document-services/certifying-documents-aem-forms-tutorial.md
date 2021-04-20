---
title: AEM Forms认证文档
seo-title: AEM Forms认证文档
description: 在AEM Forms中使用Docassance服务验证PDF文档
seo-description: 在AEM Forms中使用Docassance服务验证PDF文档
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: Document Security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 2%

---


# AEM Forms认证文档

认证文档为PDF文档和表单收件人提供了对其真实性和完整性的额外保证。

要验证文档，您可以在桌面上使用Acrobat DC，也可以在服务器上使用AEM Forms 文档 Services作为自动化流程的一部分。

本文为您提供了使用AEM Forms 文档 Services验证pdf文档的示例OSGI包。示例中使用的代码为[，可在此处](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)获取

要使用AEM Forms验证文档，需要执行以下步骤

## 将证书添加到信任存储{#adding-certificate-to-trust-store}

请按照以下步骤将证书添加到AEM中的密钥库

* [初始化全局信任存储](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜索fd-](http://localhost:4502/security/users.html) service用户
* **您必须滚动结果页以加载所有用户以查找fd-service用户**
* 多次单击fd-service用户以打开用户设置窗口
* 单击“从密钥库文件添加私钥”。指定特定于证书的别名和密码
   ![添加证书](assets/adding-certificate-keystore.PNG)
* 保存更改

## 创建OSGI服务

您可以编写自己的OSGi捆绑包，并使用AEM Forms Client SDK来实施服务以验证PDF文档。 以下链接对编写您自己的OSGi捆绑包很有用

* [创建您的第一个OSGi包](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用文档服务API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

您也可以使用作为本教程资源的一部分提供的示例捆绑包。

>[!NOTE]
>
>示例包使用名为“ares”的别名来验证文档。 因此，请确保使用此包时您的别名称为“ares”

## 在本地系统上测试示例

* 下载并安装[自定义文档服务包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下载并安装[使用服务用户包开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [确保已在Apache Sling Service用户映射器服务中添加以下条目](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** service，如以下屏幕截图所示
   ![用户映射器](assets/user-mapper-service.PNG)
* [导入示例自适应表单](assets/certify-pdf-af.zip)
* [导入并安装自定义提交](assets/custom-submit-certify.zip)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上传需要认证的PDF文档
   **可选**  — 指定要在验证文档时使用的签名字段
* 单击“提交”。
* 应将认证的PDF退还给您。


