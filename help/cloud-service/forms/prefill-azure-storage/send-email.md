---
title: 使用SendGrid发送电子邮件
description: 触发包含已保存表单链接的电子邮件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-8474
duration: 25
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 发送电子邮件

表单数据保存到Azure Blob Storage中后，会向用户发送一封电子邮件，其中包含指向所保存表单的链接。 此电子邮件使用SendGrid REST API发送。

发送电子邮件所需的Swagger文件、表单数据模型和云服务配置将作为文章资源的一部分提供给您。

您将必须创建一个SendGrid帐户，这是一个动态模板，可以摄取在自适应表单中捕获的数据。


以下是动态模板中使用的html代码片段。 使用表单数据模型调用服务传递blobID参数的值。

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


