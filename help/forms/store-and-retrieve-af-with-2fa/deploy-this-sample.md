---
title: 部署示例
description: 在本地AEM Forms实例上运行用例
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---

# 部署示例

要使此用例在您的系统上工作，请按照以下说明操作：

>[!NOTE]
>假定您在4502端口上运行AEM Forms。


## 创建数据库

此示例使用MySQL数据库来存储自适应表单数据。 您需要创建 [通过导入模式文件实现数据库模式](assets/data-base-schema.sql) 到MySQL Workbench中。

## 创建数据源

您需要创建一个名为 **StoreAndRetrieveAfData**. OSGi包中的代码使用此数据源名称

## 创建表单数据模型

表单数据模型需要基于此数据源(称为 **StoreAndRetrieveAfData**. 此表单数据模型用于获取与应用程序ID关联的手机号码。 表单数据模型可以是 [从此处下载。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用nexmo创建开发人员帐户

使用创建开发人员帐户 [Nexmo](https://dashboard.nexmo.com/) 用于发送和验证OTP代码。 记下API密钥和API密钥。 数据源和表单数据模型已针对此服务为您创建，并包含在上一步中提到的资产中。

## 部署以下OSGi包

部署包，包中包含 [用于从数据库存储和获取数据的代码](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
下载并解压缩 [developmentwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
使用Felix Web控制台部署DevelopingWithServiceUser.jar文件。

## 部署客户端库

示例使用2个客户端库。 导入这些 [客户端库](assets/client-libraries.zip) 到AEM。

## 导入自定义自适应表单模板

本演示中使用的示例表单基于自定义模板。 导入 [自定义模板到AEM](assets/custom-template-with-page-component.zip)

## 导入示例自适应表单

构成此示例的2个表单需要导入AEM。 示例表单可以是 [从此处下载](assets/sample-forms.zip)

打开 [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) 在编辑模式下。 在自适应表单的相应字段中指定API密钥和API密钥值。

## 测试解决方案

预览 [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
输入您的手机号码（包括国家/地区代码），填写用户详细信息并添加一些附件。 单击“保存并退出”按钮以保存自适应表单及其附件


## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
