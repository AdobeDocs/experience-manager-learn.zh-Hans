---
title: 使用AEM配置OKTA
description: 了解使用okta进行单点登录的各种配置设置
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 1%

---

# 使用OKTA验证AEM作者身份

第一步是在OKTA门户上配置您的应用程序。 在您的应用程序获得OKTA管理员的批准后，您将有权访问IdP证书和URL单点登录。 以下是注册新应用程序时通常使用的设置。

* **应用程序名称：** 这是您的应用程序名称。 确保为应用程序提供唯一的名称。
* **SAML收件人：** 从OKTA进行身份验证后，这是将在AEM实例上点击的URL，并显示SAML响应。 SAML身份验证处理程序通常会通过/ saml_login截获所有URL，但最好在应用程序根目录之后附加它。
* **SAML受众**:这是您的应用程序的域URL。 请勿在域URL中使用协议（http或https）。
* **SAML名称ID:** 从下拉列表中选择电子邮件。
* **环境**:选择适当的环境。
* **属性**:这些是您在SAML响应中获取的与用户有关的属性。 根据您的需要指定它们。


![okta-application](assets/okta-app-settings-blurred.PNG)


## 将OKTA(IdP)证书添加到AEM信任存储

由于SAML断言已加密，因此我们需要将IdP(OKTA)证书添加到AEM信任存储，以便在OKTA和AEM之间进行安全通信。
[初始化信任存储](http://localhost:4502/libs/granite/security/content/truststore.html)，如果尚未初始化。
记住信任存储密码。 在此过程的后续阶段，我们需要使用此密码。

* 导航到 [全局信任存储](http://localhost:4502/libs/granite/security/content/truststore.html).
* 单击“从CER文件添加证书”。 添加由OKTA提供的IdP证书，然后单击提交。

   >[!NOTE]
   >
   >请勿将证书映射到任何用户

将证书添加到信任存储时，您应获得证书别名，如以下屏幕快照所示。 在您的用例中，别名可能不同。

![证书别名](assets/cert-alias.PNG)

**记下证书别名。 在后续步骤中，您需要执行此操作。**

### 配置SAML身份验证处理程序

导航到 [configMgr](http://localhost:4502/system/console/configMgr).
搜索并打开“AdobeGranite SAML 2.0身份验证处理程序”。
提供以下指定的属性以下是需要指定的关键属性：

* **路径**  — 这是触发身份验证处理程序的路径
* **IdP Url**：这是由OKTA提供的您的IdP URL
* **IDP证书别名**：这是您在将IdP证书添加到AEM信任存储时获得的别名
* **服务提供商实体Id**：这是您的AEM Server的名称
* **密钥存储的密码**：这是您使用的信任存储密码
* **默认重定向**：这是成功身份验证后要重定向到的URL
* **用户ID属性**:uid
* **使用加密**:false
* **自动创建CRX用户**:true
* **添加到群组**:true
* **默认组**:oktausers(这是将用户添加到的组。 您可以在AEM中提供任何现有群组)
* **命名IDPolicy**:指定用于表示所请求主题的名称标识符的约束。 复制并粘贴以下突出显示的字符串 **urn:oasis:名称:tc:SAML:2.0:nameidformat:emailAddress**
* **同步属性**  — 这些是从AEM配置文件的SAML断言中存储的属性

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### 配置Apache Sling反向链接过滤器

导航到 [configMgr](http://localhost:4502/system/console/configMgr).
搜索并打开“Apache Sling反向链接过滤器”。按照以下指定设置以下属性：

* **允许为空**:false
* **允许主机**:IdP的主机名（这在您的用例中是不同的）
* **允许Regexp主机**:IdP的主机名（在您的用例中不同）Sling反向链接过滤器反向链接属性屏幕截图

![referrer-filter](assets/okta-referrer.png)

#### 为OKTA集成配置调试日志记录

在AEM上设置OKTA集成时，查看AEM SAML身份验证处理程序的调试日志可能会有所帮助。 要将日志级别设置为DEBUG，请通过AEM OSGi Web控制台创建新的Sling Logger配置。

请记住在暂存和生产环境中删除或禁用此日志记录器，以减少日志噪音。

在AEM上设置OKTA集成时，查看AEM SAML身份验证处理程序的调试日志可能会有所帮助。 要将日志级别设置为DEBUG，请通过AEM OSGi Web控制台创建新的Sling Logger配置。
**请记住在暂存和生产环境中删除或禁用此日志记录器，以减少日志噪音。**
* 导航到 [configMgr](http://localhost:4502/system/console/configMgr)

* 搜索并打开“Apache Sling日志记录器配置”
* 使用以下配置创建日志记录器：
   * **日志级别**:调试
   * **日志文件**:logs/saml.log
   * **记录器**:com.adobe.granite.auth.saml
* 单击保存以保存设置

#### 测试OKTA配置

注销AEM实例。 尝试访问链接。 您应会看到OKTA SSO正在运行。
