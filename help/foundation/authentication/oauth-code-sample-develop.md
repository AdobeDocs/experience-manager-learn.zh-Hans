---
title: 在AEM中开发OAuth范围
description: Adobe Experience Manager的可扩展OAuth作用域允许从最终用户授权的客户端应用程序访问控制资源。 下图说明了AEM上下文中的请求流。
version: 6.3, 6.4, 6.5
feature: 用户和组
topics: authentication, security
activity: develop
audience: developer
doc-type: code
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 3%

---


# 开发OAuth范围

Adobe Experience Manager的可扩展OAuth作用域允许从最终用户授权的客户端应用程序访问控制资源。 下图说明了AEM上下文中的请求流。

![Oauth作用域流](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM提供三个范围：

* 个人资料
* 脱机访问
* 复制

AEM可扩展OAuth作用域允许定义其他自定义作用域。 例如，可以开发自定义范围并将其部署到AEM，从而允许通过OAuth授权的移动应用程序仅限于读取，而不能写入资源。

OAuth是对客户端应用程序授权的首选方法，因为它使用访问令牌，而不是要求向该应用程序提供AEM用户凭据。

* [视图代码](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
