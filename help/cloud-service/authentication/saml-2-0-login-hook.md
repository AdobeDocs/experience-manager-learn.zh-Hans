---
title: SAML 2.0自定义登录挂钩
description: 了解如何为AEM开发自定义SAML 2.0登录挂接。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# SAML 2.0登录挂接

了解如何为AEM开发自定义SAML 2.0登录挂接。 本教程提供了创建与SAML 2.0身份提供程序集成的自定义登录挂接的分步说明，允许用户使用其SAML凭据进行身份验证。

如果IDP无法在SAML断言中发送用户配置文件数据和用户组成员资格，或者如果在同步到AEM之前需要转换数据，则可以实施自定义SAML挂接以扩展SAML身份验证过程。 SAML挂接允许在身份验证流程期间自定义组成员身份分配、修改用户配置文件属性以及添加自定义业务逻辑。

>[!NOTE]
>**AEM as a Cloud Service**&#x200B;和&#x200B;**AEM LTS**&#x200B;支持自定义SAML挂钩。 此功能在旧版AEM上不可用。

## 常见用例

自定义SAML挂接在需要：

+ 除SAML断言中提供的内容外，还根据自定义业务逻辑动态分配组成员资格
+ 在将用户配置文件数据同步到AEM之前，对其进行转换或扩充
+ 将复杂的SAML属性结构映射到AEM用户属性
+ 实施自定义授权规则或条件组分配
+ 在SAML身份验证期间添加自定义日志记录或审核
+ 在身份验证过程中与外部系统集成

## `SamlHook` OSGi服务接口

`com.adobe.granite.auth.saml.spi.SamlHook`接口提供了在SAML身份验证过程的不同阶段调用的两种挂接方法：

### `postSamlValidationProcess()`方法

此方法在&#x200B;**之后调用**，SAML响应已经过验证，但在&#x200B;**之前用户同步进程已启动**。 这是修改SAML断言数据（如添加或变换属性）的理想位置。

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### 用例

+ 将其他组成员资格添加到断言
+ 在同步之前转换属性值
+ 使用外部来源的数据扩充断言
+ 验证自定义业务规则

### `postSyncUserProcess()`方法

此方法在&#x200B;**之后调用**，用户同步过程已完成。 此挂接可用于在创建或更新AEM用户后执行其他操作。

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### 用例

+ 更新标准同步未涵盖的其他用户配置文件属性
+ 在AEM中创建或更新自定义用户相关资源
+ 在用户验证后触发工作流或通知
+ 记录自定义身份验证事件

**重要信息：**&#x200B;要修改存储库中的用户属性，挂接实现需要：

+ 通过`SlingRepository`注入的`@Reference`引用
+ 已配置具有适当权限的[服务用户](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)（在“Apache Sling服务用户映射器服务修正”中配置）
+ 使用try-catch-finally块进行正确的会话管理

## 实施自定义SAML挂接

以下步骤概述如何创建和部署自定义SAML挂接。

### 创建SAML挂接实施

在实现`com.adobe.granite.auth.saml.spi.SamlHook`接口的AEM项目中创建新的Java类：

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### 配置SAML挂接

SAML挂接使用OSGi配置指定它应应用于哪个IDP。 在项目中创建一个OSGi配置文件，位置：

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

`idpIdentifier`必须与在相应的SAML身份验证处理程序OSGi工厂配置(PID： `idpIdentifier`)中配置的`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`值匹配。 此匹配非常重要：将仅对具有相同`idpIdentifier`值的SAML身份验证处理程序实例调用SAML挂接。 SAML身份验证处理程序是工厂配置，这意味着您可以拥有多个实例（例如，`com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`、`com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`），并且每个挂接通过`idpIdentifier`绑定到特定的处理程序。 当配置了多个挂接（首先执行较高的值）时，`service.ranking`属性控制执行顺序。

### 添加Maven依赖关系

将所需的SAML SPI依赖项添加到AEM Maven核心项目的`pom.xml`。

**对于AEM as a Cloud Service项目**，请使用包含SAML接口的AEM SDK API依赖项：

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

`aem-sdk-api`项目包含所有必需的Adobe Granite SAML接口，包括`com.adobe.granite.auth.saml.spi.SamlHook`。

### 配置服务用户（可选）

如果SAML挂接需要修改AEM JCR存储库中的内容，例如用户属性（如`postSyncUserProcess`示例中所示），则必须配置[服务用户](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)：

1. 在`/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`的项目中创建服务用户映射：

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. 创建重新定位脚本以定义`/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json`处的服务用户和权限：

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

这将授予服务用户读取和修改存储库中用户属性的权限。

### 部署到AEM

将自定义SAML挂接部署到AEM as a Cloud Service：

1. 构建AEM项目
1. 将代码提交到Cloud Manager Git存储库
1. 使用完整栈栈部署管道进行部署
1. 当用户通过SAML进行身份验证时，SAML挂钩将自动激活


### 重要注意事项

+ **IDP标识符匹配**：在SAML挂接中配置的`idpIdentifier`必须与SAML身份验证处理程序工厂配置(`idpIdentifier`)中的`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`完全匹配
+ **属性名称**：确保挂接中引用的属性名称（如`groupMembership`）与SAML身份验证处理程序中配置的属性相匹配
+ **性能**：在每个SAML身份验证期间执行挂接实现时保持轻量级
+ **错误处理**：当发生应使身份验证失败的严重错误时，SAML挂接实施应引发`com.adobe.granite.auth.saml.spi.SamlHookException`。 SAML身份验证处理程序将捕获这些异常并返回`AuthenticationInfo.FAIL_AUTH`。 对于存储库操作，始终捕获`RepositoryException`并正确记录错误。 使用try-catch-finally块确保正确清理资源
+ **测试**：在部署到生产环境之前，在较低环境中彻底测试自定义挂钩
+ **多个挂钩**：可以配置多个SAML挂钩实施；将执行所有匹配的挂钩。 使用OSGi组件中的`service.ranking`属性来控制执行顺序（先执行排名较高的值）。 要在多个SAML身份验证处理程序工厂配置(`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)之间重用SAML挂接，请创建多个挂接配置（OSGi工厂配置），每个挂接配置具有不同的`idpIdentifier`，与相应的SAML身份验证处理程序匹配
+ **安全性**：在业务逻辑中使用SAML断言的所有数据之前，对其进行验证和整理
+ **存储库访问**：在`postSyncUserProcess`中修改用户属性时，请始终使用具有适当权限的[服务用户](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)，而不是管理会话
+ **服务用户权限**：向[服务用户](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users)授予所需的最低权限（例如，仅`jcr:read`上的`rep:write`和`/home/users`，不是完全的管理权限）
+ **会话管理**：始终使用try-catch-finally块，以确保即使发生异常也正确关闭存储库会话
+ **用户同步计时**： `postSyncUserProcess`挂接在用户已同步到OAK之后执行，因此该用户对象在该点保证存在于存储库中
