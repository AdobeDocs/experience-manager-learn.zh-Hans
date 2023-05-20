---
title: 瞭解AEM中的驗證支援
description: AEM支援的驗證（有時是授權）機制的整合檢視。
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
topic: Architecture
role: Architect
level: Experienced
exl-id: 96c542ae-6ab6-4d8a-94df-a58b03469320
last-substantial-update: 2022-09-10T00:00:00Z
thumbnail: KT-406.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 14%

---

# 瞭解AEM 6.x中的驗證支援

AEM支援的驗證（有時是授權）機制的整合檢視。

*下表說明使用者如何在AEM中進行驗證。*

<table>
    <tbody>
        <tr>
            <td><strong>身份验证</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM作為標準身分提供者</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>基本驗證</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Forms型</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>權杖型(含 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">封裝權杖</a>)</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>非AEM系統作為標準身分提供者</strong></td>
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
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
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
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-gems-events/assets/oauth-server-functionality-in-aem-7-23-14.pdf" target="_blank">OAuth 1.0a和2.0</a></td>
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

⁕URL *透過社群專案提供，但Adobe不直接支援。*
