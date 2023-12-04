---
title: 在AEM中开发OAuth作用域
description: Adobe Experience Manager的可扩展OAuth范围允许对来自最终用户授权的客户端应用程序的资源进行访问控制。 下图说明了AEM上下文中的请求流。
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 43
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 1%

---

# 开发OAuth范围

Adobe Experience Manager的可扩展OAuth范围允许对来自最终用户授权的客户端应用程序的资源进行访问控制。 下图说明了AEM上下文中的请求流。

![Oauth范围流](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM提供三个作用域：

* 配置文件
* 脱机访问
* 复制

AEM可扩展的OAuth范围允许定义其他自定义范围。 例如，可以开发自定义范围并将其部署到AEM，从而允许通过OAuth授权的移动应用程序只能读取而不能写入资产。

OAuth是授权客户端应用程序的首选方法，因为它使用访问令牌，而不是要求向该应用程序提供AEM用户的凭据。

* [查看代码](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
