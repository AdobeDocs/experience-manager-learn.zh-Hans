---
title: 在自适应表单提交时发送电子邮件
description: 使用发送电子邮件组件在自适应表单提交时发送确认电子邮件
feature: Adaptive Forms
doc-type: article
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
duration: 40
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 在自适应表单提交时发送电子邮件 {#sending-email-on-adaptive-form-submission}

常见的操作之一是在自适应表单成功提交时向提交者发送确认电子邮件。 要完成此操作，我们将选择“发送电子邮件”作为提交操作。

您可以使用电子邮件模板，也可以只键入电子邮件的正文，如下面的屏幕快照中所示。

请注意在电子邮件中插入表单字段值的语法。我们还可以选择在电子邮件中包含表单附件，方法是选中配置属性中的“包含附件”复选框。

在提交自适应表单时，收件人将收到电子邮件。

![发送电子邮件](assets/sendemailaction.gif)

## 所需配置 {#configurations-needed}

您必须配置Day CQ Mail服务。 可通过将浏览器指向[Felix配置管理器](http://localhost:4502/system/console/configMgr)来配置此项

此屏幕截图显示了adobe邮件服务器的配置属性。

![邮件服务](assets/mailservice.png)

要在您的服务器上尝试此操作，请按照以下说明操作：

* [使用包管理器在AEM中导入与此文章关联的资源](assets/timeoffrequest.zip)。

* 打开[TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)

* 填写详细信息。确保在电子邮件字段中提供有效的电子邮件地址。

* 提交表单。
