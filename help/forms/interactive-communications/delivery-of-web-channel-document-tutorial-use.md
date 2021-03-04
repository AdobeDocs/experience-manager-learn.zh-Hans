---
title: 交互通信文档投放- Web渠道AEM Forms
seo-title: 交互通信文档投放- Web渠道AEM Forms
description: 通过电子邮件链接投放Web渠道文档
seo-description: 通过电子邮件链接投放Web渠道文档
feature: 交互通信
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# 电子邮件投放Web渠道文档

定义并测试了Web渠道交互式通信文档后，您需要一种投放机制来将Web渠道文档交付到收件人。

在本文中，我们将电子邮件视为Web渠道文档的投放机制。 收件人将通过电子邮件获得指向Web渠道文档的链接。单击该链接后，将要求用户进行身份验证，并为Web渠道文档填充特定于登录用户的数据。

让我们看下面的代码片断。 此代码是GET.jsp的一部分，当用户单击电子邮件中的链接以视图Web渠道文档时，会触发该代码。 我们使用jackrabbit UserManager获取登录用户。 获得登录用户后，我们将获得与用户用户档案关联的accountNumber属性值。

然后，我们将accountNumber值与映射中名为accountnumber的键关联。 键&#x200B;**accountnumber**&#x200B;在表单数据模式中定义为请求属性。 此属性的值作为输入参数传递给表单数据模态读取服务方法。

第7行：我们将根据由交互通信文档url标识的资源类型，将收到的请求发送到另一个servlet。 由第二个servlet返回的响应包含在第一个servlet的响应中。

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

![请求参数](assets/requestparameter.png)

为表单数据模式的读取服务定义的请求属性


[示例AEM包](assets/webchanneldelivery.zip)。
