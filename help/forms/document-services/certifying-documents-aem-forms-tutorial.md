---
title: AEM Forms文档认证
seo-title: AEM Forms文档认证
description: 使用Docassuance服务在AEM Forms验证PDF文档
seo-description: 使用Docassuance服务在AEM Forms验证PDF文档
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 1%

---


# AEM Forms文档认证

认证文档为PDF文档和表单收件人提供对其真实性和完整性的更多保证。

要认证文档，您可以在桌面上使用AcrobatDC，或在服务器上使用AEM Forms文档服务作为自动化流程的一部分。

本文提供了使用AEM Forms文档服务验证pdf文档的示例OSGI包。示例中使用的代码在[此处提供](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

要使用AEM Forms认证文档，需要遵循以下步骤

## 将证书添加到信任存储{#adding-certificate-to-trust-store}

请按照以下步骤将证书添加到AEM中的密钥库

* [初始化全局信任存储](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜索fd-](http://localhost:4502/security/users.html) service用户
* **您必须滚动结果页以加载所有用户以查找fd-service用户**
* 多次单击fd-service用户以打开用户设置窗口
* 单击“从密钥库文件添加私钥”。指定证书的特定别名和密码
   ![添加证书](assets/adding-certificate-keystore.PNG)
* 保存更改

## 创建OSGI服务

您可以编写自己的OSGi捆绑包，并使用AEM Forms客户端SDK来实施服务以验证PDF文档。 以下链接对于编写您自己的OSGi捆绑包非常有用

* [创建您的第一个OSGi捆绑包](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用文档服务API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

您也可以使用作为本教程资源的一部分提供的示例捆绑包。

>[!NOTE]
>
>示例捆绑使用名为“ares”的别名来验证文档。 因此，请确保在使用此捆绑包时您的别名称为“ares”

## 在本地系统上测试示例

* 下载并安装[自定义文档服务包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下载并安装[使用服务用户捆绑进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [确保已在Apache Sling Service User Mapper服务中添加以下条目](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** service，如下图所示
   ![用户映射器](assets/user-mapper-service.PNG)
* [导入示例自适应表单](assets/certify-pdf-af.zip)
* [导入和安装自定义提交](assets/custom-submit-certify.zip)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上传需要认证的PDF文档
   **可选** -指定要在验证文档时使用的签名字段
* 单击“提交”。
* 应将认证的PDF返回给您。


