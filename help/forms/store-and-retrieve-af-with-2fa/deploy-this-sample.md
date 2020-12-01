---
title: 部署示例
description: 在您的本地AEM Forms实例上运行用例
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---



# 部署示例

要使此用例在您的系统上工作，请按照以下说明操作：

>[!NOTE]
>假定你在4502港运营AEM Forms。


## 创建数据库

此示例使用MySQL数据库存储自适应表单数据。 您需要通过将模式文件](assets/data-base-schema.sql)导入MySQL工作台来创建[模式库。

## 创建数据源

您需要创建一个名为&#x200B;**StoreAndRetrieveAfData**&#x200B;的数据源。 OSGi捆绑包中的代码使用此数据源名称

## 创建表单数据模型

表单数据模型需要基于此名为&#x200B;**StoreAndRetrieveAfData**&#x200B;的数据源创建。 此表单数据模型用于获取与应用程序ID关联的手机号码。 表单数据模型可从此处下载。[](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用Nexmo创建开发人员帐户

创建具有[Nexmo](https://dashboard.nexmo.com/)的开发人员帐户，以发送和验证OTP代码。 记下API密钥和API密钥。 已为此服务创建数据源和表单数据模型，并包含在上一步中提到的资产中。

## 部署以下OSGi包

部署具有[代码的包，以存储数据库](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)并从中提取数据
部署[DevelopingWithServiceUser Bundle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)。

## 部署客户端库

该示例使用2个客户端库。 将这些[客户端库](assets/client-libraries.zip)导入AEM。

## 导入自定义自适应表单模板

此演示中使用的示例表单基于自定义模板。 将[自定义模板导入AEM](assets/custom-template-with-page-component.zip)

## 导入示例自适应表单

组成此范例的2个表单需要导入AEM。 范例表单可从此处[下载](assets/sample-forms.zip)

在编辑模式下打开[MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html)。 在自适应表单的相应字段中指定API密钥和API机密值。

## 测试解决方案

预览[StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
输入您的手机号码，包括国家／地区代码，填写用户详细信息并添加一些附件。 单击“保存并退出”按钮以保存自适应表单及其附件


## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
