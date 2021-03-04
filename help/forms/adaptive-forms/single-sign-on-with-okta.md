---
title: 使用AEM配置OKTA
description: 了解使用okta进行单点登录的各种配置设置
feature: 自适应表单
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
topic: 管理
role: 管理员
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 2%

---


# 使用OKTA验证AEM作者身份

第一步是在OKTA门户上配置您的应用程序。 在您的应用程序获得OKTA管理员的批准后，您将有权访问IdP证书和在URL上单一登录。 以下是注册新应用程序时通常使用的设置。

* **应用程序名** 称：这是您的应用程序名称。确保为应用程序指定唯一的名称。
* **SAML收件人:** 从OKTA进行身份验证后，这是SAML响应将在AEM实例上点击的URL。SAML身份验证处理程序通常会截取所有包含/saml_login的URL，但最好在应用程序根目录之后附加它。
* **SAML受众**:这是您的应用程序的域URL。请勿在域URL中使用协议（http或https）。
* **SAML名称ID：从** 下拉列表中选择电子邮件。
* **环境**:选择适当的环境。
* **属性**:这些属性是您在SAML响应中获得的有关用户的属性。根据您的需要指定它们。


![okta应用程序](assets/okta-app-settings-blurred.PNG)


## 将OKTA(IdP)证书添加到AEM信任存储

由于SAML声明已加密，因此我们需要将IdP(OKTA)证书添加到AEM信任存储，以便在OKTA和AEM之间实现安全通信。
[初始化信任存储](http://localhost:4502/libs/granite/security/content/truststore.html)，如果尚未初始化。记住信任存储密码。 我们需要在此过程的稍后阶段使用此密码。

* 导航到[全局信任存储](http://localhost:4502/libs/granite/security/content/truststore.html)。
* 单击“从CER文件添加证书”。 添加OKTA提供的IdP证书，然后单击“submit”。

   >[!NOTE]
   >
   >请勿将证书映射到任何用户

在将证书添加到信任存储时，您应获得证书别名，如下面的屏幕快照所示。 在您的情况下，别名可能不同。

![证书别名](assets/cert-alias.PNG)

**记下证书别名。在后面的步骤中，您需要此项。**

### 配置SAML身份验证处理程序

导航到[configMgr](http://localhost:4502/system/console/configMgr)。
搜索并打开“Adobe Granite SAML 2.0身份验证处理程序”。
提供以下属性，如下所示
以下是需要指定的键属性：

* **path**  — 这是将触发身份验证处理函数的路径
* **IdP Url**：这是由OKTA提供的IdP URL
* **IDP证书别名**：这是您将IdP证书添加到AEM信任存储时获得的别名
* **服务提供商实体** Id：这是AEM Server的名称
* **密钥存储的密码**：这是您使用的信任存储密码
* **默认重定向**：这是在成功验证时重定向到的URL
* **UserID属性**:uid
* **使用加密**:false
* **自动创建CRX用户**:true
* **添加到组**:true
* **默认组**:oktausers(这是要向其添加用户的组。您可以在AEM中提供任何现有组)
* **NamedIDPolicy**:指定用于表示所请求主题的名称标识符的约束。复制并粘贴以下高亮显示的字符串&#x200B;**urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **同步属性**  — 这些属性是从AEM 用户档案中的SAML断言存储的

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### 配置Apache Sling推荐人筛选器

导航到[configMgr](http://localhost:4502/system/console/configMgr)。
搜索并打开“Apache Sling推荐人过滤器”。按如下指定设置以下属性：

* **允许为空**:true
* **允许主机**:IdP的主机名（在您的情况下将有所不同）
* **允许Regexp主机**:IdP的主机名（在您的情况下将有所不同）Sling推荐人过滤器推荐人属性屏幕截图

![推荐人滤镜](assets/sling-referrer-filter.PNG)

#### 为OKTA集成配置DEBUG日志

在AEM上设置OKTA集成时，查看AEM SAML身份验证处理程序的DEBUG日志会很有帮助。 要将日志级别设置为DEBUG，请通过AEM OSGi Web控制台新建一个Sling Logger配置。

请记住在“Stage”（级）和“Production”（生产）上删除或禁用此记录器，以减少日志噪声。

在AEM上设置OKTA集成时，查看AEM SAML身份验证处理程序的DEBUG日志会很有帮助。 要将日志级别设置为DEBUG，请通过AEM OSGi Web控制台新建一个Sling Logger配置。
**请记住在“Stage”（级）和“Production”（生产）上删除或禁用此记录器，以减少日志噪声。**
* 导航到[configMgr](http://localhost:4502/system/console/configMgr)

* 搜索并打开“Apache Sling日志记录记录器配置”
* 使用以下配置创建记录器：
   * **日志级别**:调试
   * **日志文件**:logs/saml.log
   * **记录器**:com.adobe.granite.auth.saml
* 单击保存以保存设置



#### 测试OKTA配置

注销您的AEM实例。 尝试访问链接。 您应当看到OKTA SSO的实际操作情况。
