---
title: 在AEM中开发OAuth作用域
description: Adobe Experience Manager的可扩展OAuth作用域允许从最终用户授权的客户端应用程序对资源进行访问控制。 下图说明了AEM上下文中的请求流。
version: 6.3, 6.4, 6.5
feature: 用户和群组
topic: 开发
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---


# 开发OAuth作用域

Adobe Experience Manager的可扩展OAuth作用域允许从最终用户授权的客户端应用程序对资源进行访问控制。 下图说明了AEM上下文中的请求流。

![Oauth作用域流](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM提供了三个作用域：

* 个人资料
* 脱机访问
* 复制

AEM可扩展OAuth作用域允许定义其他自定义作用域。 例如，可以开发自定义范围并将其部署到AEM，以便允许通过OAuth授权的移动设备应用程序被限制为读取资产，但不能写入资产。

OAuth是授权客户端应用程序的首选方法，因为它使用访问令牌，而不是要求向该应用程序提供AEM用户的凭据。

* [查看代码](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
