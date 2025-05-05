---
title: 部署示例
description: 在本地AEM Forms实例上运行用例
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# 部署示例

要使此用例在您的系统上正常工作，请按照以下说明操作：

>[!NOTE]
>假定您正在端口4502上运行AEM Forms。


## 创建数据库

此示例使用MySQL数据库存储自适应表单数据。 您需要通过将架构文件[&#128279;](assets/data-base-schema.sql)导入MySQL Workbench来创建数据库架构。

## 创建数据源

您需要创建一个名为&#x200B;**StoreAndRetrieveAfData**&#x200B;的Apache Sling连接池化数据源，指向在之前步骤中创建的数据库架构。 OSGi捆绑包中的代码使用此数据源名称。

## 创建表单数据模型

需要基于名为&#x200B;**StoreAndRetrieveAfData**&#x200B;的数据源创建表单数据模型。 此表单数据模型用于获取与应用程序ID关联的手机号码。 可从此处[下载表单数据模型。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## 使用nexmo创建开发人员帐户

创建具有[Nexmo](https://dashboard.nexmo.com/)的开发人员帐户，用于发送和验证OTP代码。 记下API密钥和API密钥。 已针对此服务为您创建了数据源和表单数据模型，并包含在上一步中提到的资产中。

## 部署以下OSGi包

部署具有[代码以从数据库](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)存储和提取数据的包
下载并解压缩[developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=zh-Hans)。
使用Felix Web控制台部署DevelopingWithServiceUser.jar文件。

## 部署客户端库

此示例使用2个客户端库。 将这些[客户端库](assets/store-af-with-attachments-client-lib.zip)导入AEM。

## 导入自定义自适应表单模板

此演示中使用的示例表单基于自定义模板。 将[自定义模板导入AEM](assets/custom-template-with-page-component.zip)

## 导入自适应表单示例

构成此示例的2个表单需要导入AEM。 可从此处[&#128279;](assets/sample-forms.zip)下载示例表单

在编辑模式下打开[MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html)。 在自适应表单的相应字段中指定Vonage API密钥和API密钥值。

## 测试解决方案

预览[StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
输入您的手机号码（包括国家/地区代码），填写您的用户详细信息并添加一些附件。 单击“保存并退出”按钮以保存自适应表单及其附件


## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/346927?quality=12&learn=on&captions=chi_hans)
