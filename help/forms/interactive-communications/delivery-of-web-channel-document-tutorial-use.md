---
title: 交互式通信文档的交付 — Web渠道AEM Forms
description: 通过电子邮件中的链接投放Web渠道文档
feature: Interactive Communication
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 67
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Web渠道文档的电子邮件投放

定义并测试Web渠道交互式通信文档后，您需要一种交付机制将Web渠道文档交付给收件人。

在本文中，我们将电子邮件看作Web渠道文档的投放机制。 收件人将通过电子邮件获取指向Web渠道文档的链接。单击该链接时，将要求用户进行身份验证，并且使用特定于登录用户的数据填充Web渠道文档。

下面我们看一看以下代码片段。 此代码是GET.jsp的一部分，当用户单击电子邮件中的链接以查看Web渠道文档时，将触发该代码。 我们使用jackrabbit UserManager获取登录用户。 获得登录用户后，我们会获得与用户配置文件关联的accountNumber属性的值。

然后，我们将accountNumber值与映射中名为accountnumber的键关联。 键 **accountnumber** 在表单数据模式中定义为请求属性。 此属性的值作为输入参数传递给表单数据模式读取服务方法。

第7行：我们将根据交互式通信文档URL标识的资源类型，将收到的请求发送给另一个servlet。 此第二个servlet返回的响应包含在第一个servlet的响应中。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![Include方法](assets/includemethod.jpg)

7行代码的可视表示形式

![请求参数配置](assets/requestparameter.png)

为表单数据模式的读取服务定义的请求属性

[示例AEM包](assets/webchanneldelivery.zip).
