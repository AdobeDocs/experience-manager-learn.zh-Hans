---
title: AEM Forms中的认证文档
description: 使用Docassurance服务认证AEM Forms中的PDF文档
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 96
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# AEM Forms中的认证文档

经认证的文档为PDF文档提供并表单接收者增加了其真实性和完整性的保证。

要认证文档，您可以在桌面上使用Acrobat DC，或将AEM Forms Document Services作为服务器上自动化流程的一部分。

本文为您提供了使用AEM Forms Document Services验证pdf文档的示例OSGI捆绑包。示例中使用的代码为 [此处提供](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

要使用AEM Forms认证文档，需要执行以下步骤

## 正在将证书添加到信任存储区 {#adding-certificate-to-trust-store}

请按照下面提到的步骤将证书添加到AEM中的Keystore

* [初始化全局信任存储区](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜索fd-service](http://localhost:4502/security/users.html) 用户
* **您必须滚动结果页面才能加载所有用户以找到fd-service用户**
* 双击fd-service用户以打开用户设置窗口
* 单击“从Keystore文件添加私钥”。指定特定于证书的别名和密码
  ![add-certificate](assets/adding-certificate-keystore.PNG)
* 保存更改

## 创建OSGI服务

您可以编写自己的OSGi捆绑包，并使用AEM Forms客户端SDK实施服务来认证PDF文档。 以下链接对于编写您自己的OSGi捆绑包将会很有用

* [创建您的第一个OSGi捆绑包](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用文档服务API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

或者，您也可以使用作为本教程资源一部分的示例捆绑包。

>[!NOTE]
>
>示例捆绑包使用名为“ares”的别名来证明文档。 因此，请确保在使用此捆绑包时您的别名名为“ares”

## 在本地系统上测试示例

* 下载并安装 [自定义文档服务捆绑包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下载并安装 [使用服务用户捆绑包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [确保您已在Apache Sling服务用户映射器服务中添加以下条目](http://localhost:4502/system/console/configMgr)
  **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service** 如下面的屏幕快照所示
  ![用户映射器](assets/user-mapper-service.PNG)
* [导入自适应表单示例](assets/certify-pdf-af.zip)
* [导入并安装自定义提交](assets/custom-submit-certify.zip)
* [打开自适应表单](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上传需要认证的PDF文档
  **可选**  — 指定要用于证明文档的签名字段
* 单击提交。
* 应该把认证的PDF还给你
