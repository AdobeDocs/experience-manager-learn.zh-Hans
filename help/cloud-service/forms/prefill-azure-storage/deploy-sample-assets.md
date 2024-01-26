---
title: 部署示例资源
description: 在本地云就绪系统上部署示例资产。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 50
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 部署示例资源

要使此用例在您的系统上正常工作，请在本地云就绪系统上部署以下资产。

* 请确保您已创建[简介文档](./introduction.md)

* [安装自定义自适应表单模板和关联的页面组件](./assets/azure-portal-template-page-component.zip)

* [安装SendGrid表单数据模型](./assets/send-grid-form-data-model.zip). 您必须提供API密钥和经验证来自地址的SendGrid，此表单数据模型才能正常工作。 在表单数据模型编辑器中测试表单数据模型

* [安装Azure支持的表单数据模型](./assets/azure-storage-fdm.zip). 你必须提供你的Azure存储帐户凭据才能使表单数据模型正常工作。 在表单数据模型编辑器中测试表单数据模型。

* [导入自适应表单示例](./assets/credit-applications-af.zip)
* [导入客户端库](./assets/client-lib.zip)
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). 输入有效的电子邮件，然后单击“保存”按钮。 表单数据应存储在Azure Storage中，并且会向指定的电子邮件地址发送一封电子邮件，其中包含指向所保存表单的链接。
