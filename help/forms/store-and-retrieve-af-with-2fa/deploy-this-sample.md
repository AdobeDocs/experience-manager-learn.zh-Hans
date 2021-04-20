---
title: 部署示例
description: 获取在本地AEM Forms实例上运行的用例
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 2%

---



# 部署示例

要使此用例在您的系统上工作，请按照以下说明操作：

>[!NOTE]
>假定您在4502端口运行AEM Forms。


## 创建数据库

此示例使用MySQL数据库存储自适应表单数据。 您需要通过将模式文件](assets/data-base-schema.sql)导入MySQL工作台来创建[模式库。

## 创建数据源

您需要创建一个名为&#x200B;**StoreAndRetrieveAfData**&#x200B;的数据源。 OSGi捆绑包中的代码使用此数据源名称

## 创建表单数据模型

需要基于此名为&#x200B;**StoreAndRetrieveAfData**&#x200B;的数据源创建表单数据模型。 此表单数据模型用于获取与应用程序ID关联的移动电话号码。 表单数据模型可从此处下载[。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用Nexmo创建开发人员帐户

创建具有[Nexmo](https://dashboard.nexmo.com/)的开发人员帐户，以发送和验证OTP代码。 记下API密钥和API密钥。 已针对此服务为您创建数据源和表单数据模型，并包含在上一步中提到的资产中。

## 部署以下OSGi包

部署包含[代码的包，以存储数据库](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)并从中提取数据
部署[DevelopingWithServiceUser Bundle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)。

## 部署客户端库

该示例使用2个客户端库。 将这些[客户端库](assets/client-libraries.zip)导入AEM。

## 导入自定义自适应表单模板

此演示中使用的示例表单基于自定义模板。 将[自定义模板导入AEM](assets/custom-template-with-page-component.zip)

## 导入示例自适应表单

组成此示例的2个表单需要导入AEM。 可从此处下载示例表单[](assets/sample-forms.zip)

在编辑模式下打开[MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html)。 在自适应表单的相应字段中指定API密钥和API密钥值。

## 测试解决方案

预览[StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
输入您的手机号码（包括国家/地区代码），填写您的用户详细信息并添加一些附件。 单击“保存并退出”按钮以保存自适应表单及其附件


## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
