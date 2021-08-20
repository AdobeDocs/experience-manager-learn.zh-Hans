---
title: 测试解决方案
description: 通过向表单中添加附件来测试解决方案，并触发发送电子邮件的工作流。
feature: 自适应表单
version: 6.5
topic: 开发
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# 测试两种方法所需的步骤

* 配置[Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service)以从AEM Forms服务器发送电子邮件
* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)部署[formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar)包

## 将zip文件作为电子邮件附件发送



* 部署[SendFormAttachmentsViaEmail工作流。](assets/zipped-form-attachments-model.zip) 此工作流使用发送电子邮件组件发送zipped_attachments.zip文件，该文件通过自定义流程步骤保存在有效负荷文件夹下。根据需要配置发件人和收件人的电子邮件地址。
* 从[Forms和文档UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入[示例表单](assets/zip-form-attachments-form.zip)
* [预览表](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 单并添加几个附件并提交表单。
* 应触发工作流，并发送包含zip文件的电子邮件通知。

## 将表单附件作为单个文件发送

* 部署[SendForm工作流。](assets/send-form-attachments-model.zip) 此工作流使用发送电子邮件组件将表单附件作为单个文件发送。根据需要配置发件人和收件人的电子邮件地址。
* 从[Forms和文档UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入[示例表单](assets/send-list-attachments-form.zip)
* [预览表](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) 单并添加几个附件并提交表单。
* 应触发工作流，并发送带有表单附件的电子邮件通知。
