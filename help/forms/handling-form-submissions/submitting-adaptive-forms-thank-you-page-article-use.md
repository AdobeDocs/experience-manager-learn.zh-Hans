---
title: “提交致谢”页面
seo-title: “提交致谢”页面
description: 在提交自适应表单时显示感谢页面
seo-description: 在提交自适应表单时显示感谢页面
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
translation-type: tm+mt
source-git-commit: 0bccdea82f6db391cbca3ab06009a7b4420a38bf
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# 提交致谢页面{#submitting-to-thank-you-page}

作为HTTPGET请求的一部分，“提交到REST端点”选项会将表单中填写的数据传递到配置的确认页。 您可以添加要请求的字段的名称。 请求的格式为：

\{fieldName\} = \{parameterName\}。 例如，submitterName是自适应表单字段的名称，submitter是参数的名称。 在感谢页中，您可以使用request.getParameter(&quot;submitter&quot;)访问提交者参数，以便保留提交者名称字段的值。

submitterName=submitter

在下面的屏幕截图中，我们将提交自适应表单以感谢您页面，该页位于/content/thankyou。 在此感谢页面中，我们传递了3个将包含表单字段值的请求属性。

![感谢](assets/thankyoupage.gif)

您还可以通过POST提交到外部端点。 要完成此操作，您只需选中“启用发布请求”复选框，并提供外部端点的URL。 提交表单时，您将收到“感谢”页面，并同时调用POST端点。

![捕获](assets/capture.gif)


要在服务器上测试此功能，请按照以下说明操作：

* 使用包管理器](assets/submittingtorestendpoint.zip)将与本文关联的[资产文件导入AEM
* 将您的浏览器指向[结束请求表](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填写必填字段并提交表单
* 您应该在感谢页面中填入您的信息

