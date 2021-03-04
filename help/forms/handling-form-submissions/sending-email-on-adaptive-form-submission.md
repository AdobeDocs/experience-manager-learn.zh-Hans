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
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 3%

---


# 在自适应表单提交时发送电子邮件{#sending-email-on-adaptive-form-submission}

常见操作之一是，在自适应表单成功提交时向提交者发送确认电子邮件。 为此，我们将选择“发送电子邮件”作为提交操作。

您可以使用电子邮件模板，或只键入电子邮件正文，如下面的此屏幕截图所示。

请注意在电子邮件中插入表单字段值的语法。我们还可以选择在配置属性中选中复选框“包括附件”，将表单附件包含在电子邮件中。

提交自适应表单后，收件人将收到电子邮件。

![SendEmail](assets/sendemailaction.gif)

## 需要的配置{#configurations-needed}

您必须配置Day CQ邮件服务。 可通过将您的浏览器指向[Felix Configuration Manager](http://localhost:4502/system/console/configMgr)来配置此配置

屏幕截图显示了adobe邮件服务器的配置属性。

![mailservice](assets/mailservice.png)

要在服务器上尝试，请按照以下说明操作：

* [使用包](assets/timeoffrequest.zip) 管理器在AEM中导入与此文章关联的资产。

* 打开[TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。

* 填写详细信息。请确保在电子邮件字段中提供有效的电子邮件地址。

* 提交表单。
