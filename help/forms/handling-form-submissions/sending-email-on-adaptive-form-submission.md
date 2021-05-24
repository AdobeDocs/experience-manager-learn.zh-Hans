---
title: 在自适应表单提交时发送电子邮件
seo-title: 在自适应表单提交时发送电子邮件
description: 使用发送电子邮件组件在自适应表单提交时发送确认电子邮件
seo-description: 使用发送电子邮件组件在自适应表单提交时发送确认电子邮件
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: 自适应表单
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 3%

---


# 在自适应表单提交时发送电子邮件{#sending-email-on-adaptive-form-submission}

其中一种常见操作是，在成功提交自适应表单时向提交者发送确认电子邮件。 要完成此操作，我们将选择“发送电子邮件”作为提交操作。

您可以使用电子邮件模板，或只是键入电子邮件的正文，如下面此屏幕截图所示。

请注意在电子邮件中插入表单字段值的语法。我们还可以选择在电子邮件中包含表单附件，方法是选中配置属性中的复选框“包含附件”。

提交自适应表单后，收件人将收到电子邮件。

![SendEmail](assets/sendemailaction.gif)

## 需要的配置{#configurations-needed}

您必须配置Day CQ Mail服务。 这可以通过将您的浏览器指向[Felix Configuration Manager](http://localhost:4502/system/console/configMgr)进行配置

屏幕截图显示了adobe邮件服务器的配置属性。

![邮件服务](assets/mailservice.png)

要在服务器上尝试此操作，请按照以下说明操作：

* [在AEM中](assets/timeoffrequest.zip) 使用包管理器导入与本文关联的资产。

* 打开[TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。

* 填写详细信息。确保在电子邮件字段中提供有效的电子邮件地址。

* 提交表单。
