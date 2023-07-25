---
title: 部署示例资源
description: 在本地云就绪系统上部署示例资产。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# 部署示例资源

要使此用例在您的系统上正常工作，请在本地云就绪系统上部署以下资产。

* 请确保您已创建[简介文档](./introduction.md)

* [安装自定义自适应表单模板和关联的页面组件](./assets/azure-portal-template-page-component.zip)

* [安装SendGrid表单数据模型](./assets/send-grid-form-data-model.zip). 您必须提供API密钥和从地址验证的SendGrid，此表单数据模型才能正常工作。 在表单数据模型编辑器中测试表单数据模型

* [安装Azure支持的表单数据模型](./assets/azure-storage-fdm.zip). 您必须提供Azure存储帐户凭据才能使用表单数据模型。 在表单数据模型编辑器中测试表单数据模型。

* [导入自适应表单示例](./assets/credit-applications-af.zip)
* [导入客户端库](./assets/client-lib.zip)
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). 输入有效的电子邮件，然后单击保存按钮。 表单数据应存储在Azure Storage中，并且会向指定的电子邮件地址发送一封电子邮件，其中包含已保存表单的链接。


