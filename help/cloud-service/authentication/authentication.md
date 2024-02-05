---
title: AEMas a Cloud Service中的身份验证
description: 了解AEMas a Cloud Service中的身份验证。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 51
source-git-commit: dfb9281abacfe28068b866a8eda786e2d30b9ea6
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 3%

---

# AEMas a Cloud Service身份验证

AEMas a Cloud Service支持多种身份验证选项，并且因服务类型而异。

|                       | AEM Author | AEM Publish |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| · [通过Adobe IMS的SAML 2.0](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [单点登录(SSO)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [令牌身份验证](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |
| 基本身份验证 | ✘ | ✘ |

## 身份验证选项

有关如何设置和使用身份验证方法的详细信息，请单击下面的相应链接。

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          通过Adobe Admin Console使用Adobe IMS管理AEM创作访问权限。
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        使用AEM发布服务的SAML 2.0集成向IDP验证网站用户。
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="令牌" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">令牌身份验证</a></strong></div>
      <p>
        允许应用程序和中间件使用API服务令牌向AEM进行身份验证。
      </p>
    </td>   
  </tr>
</table>
