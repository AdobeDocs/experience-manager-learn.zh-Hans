---
title: 了解AEM中的身份验证支持
description: '对AEM支持的身份验证（有时还是授权）机制的整合视图。 '
version: 6.3, 6.4, 6.5
feature: 用户和群组
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
topic: 架构
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 3%

---


# 了解AEM 6.x中的身份验证支持

对AEM支持的身份验证（有时还是授权）机制的整合视图。

*下表介绍了用户如何在AEM中进行身份验证。*

<table>
    <tbody>
        <tr>
            <td><strong>身份验证</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM作为规范身份提供程序</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>基本身份验证</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Forms</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>基于令牌（带有<a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">封装的令牌</a>）</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>非AEM系统作为规范身份提供程序</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">单点登录</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a和2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

⁕*通过社区项目提供，但不直接受Adobe支持。*
