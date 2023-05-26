---
title: 提交以感谢页面
seo-title: Submitting To Thank You Page
description: 在提交自适应表单时显示感谢页面
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 提交以感谢页面 {#submitting-to-thank-you-page}

“提交到REST端点”选项将表单中填写的数据作为HTTPGET请求的一部分传递到配置的确认页面。 您可以添加要请求的字段的名称。 请求的格式为：

\{fieldName\} = \{parameterName\}。 例如，submitterName是自适应表单字段的名称，submitter是参数的名称。 在感谢页面中，您可以使用request.getParameter(&quot;submitter&quot;)访问提交者参数以获取对提交者名称字段值的保留。

`submitterName=submitter`

在下面的屏幕快照中，我们将提交自适应表单以感谢位于/content/thankyou的页面。 在此感谢页面中，我们将传递3个包含表单字段值的请求属性。

![感谢页面](assets/thankyoupage.gif)

您还可以通过POST提交到外部端点。 要完成此操作，您只需选中“启用post请求”复选框并提供外部端点的URL。 当您提交表单时，您会看到一个感谢页面，并且同时调用POST端点。

![捕获配置](assets/capture.gif)

要在您的服务器上测试此功能，请按照以下说明操作：

* 导入 [使用包管理器将与本文关联的资源文件导入AEM](assets/submittingtorestendpoint.zip)
* 将浏览器指向 [休假请求表单](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填写必填字段并提交表单
* 您应该会获得一个感谢页面，并在该页面中填充了您的信息
