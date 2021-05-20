---
title: 交付交互式通信文档 — Web渠道AEM Forms
seo-title: 交付交互式通信文档 — Web渠道AEM Forms
description: 通过电子邮件中的链接交付Web渠道文档
seo-description: 通过电子邮件中的链接交付Web渠道文档
feature: 交互式通信
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 1%

---


# Web渠道文档的电子邮件发送

定义并测试Web渠道交互式通信文档后，您需要一种传送机制来将Web渠道文档传送给收件人。

在本文中，我们将电子邮件视为Web渠道文档的投放机制。 收件人将通过电子邮件获得指向Web渠道文档的链接。单击该链接后，将要求用户进行身份验证，并且Web渠道文档将填充登录用户的特定数据。

让我们看一下以下代码片段。 此代码是GET.jsp的一部分，当用户单击电子邮件中的链接上的以查看Web渠道文档时，将触发该代码。 我们使用jackrabbit UserManager获取已登录用户。 获取登录用户后，我们会获取与用户配置文件关联的accountNumber属性的值。

然后，我们将accountNumber值与映射中名为accountnumber的键值关联。 键&#x200B;**accountnumber**&#x200B;在表单数据模式中定义为“请求属性”。 此属性的值将作为输入参数传递到表单数据模式读取服务方法。

第7行：我们将根据交互式通信文档url标识的资源类型，将收到的请求发送到另一个Servlet。 第二Servlet返回的响应包含在第一Servlet的响应中。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![includemethod](assets/includemethod.jpg)

第7行代码的可视表示

![requestparameter](assets/requestparameter.png)

为表单数据模式的读取服务定义的请求属性


[示例AEM包](assets/webchanneldelivery.zip)。
