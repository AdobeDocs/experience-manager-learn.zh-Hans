---
title: 在AEMas a Cloud Service中进行身份验证
description: 了解AEMas a Cloud Service中的身份验证。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.jpeg
source-git-commit: e666e38d6b2a7057f7016b35ad1034a4487e9bc7
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 4%

---


# AEMas a Cloud Service身份验证

AEMas a Cloud Service支持多个身份验证选项，并因服务类型而异。

|  | AEM Author | AEM 发布 |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [令牌身份验证](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## 身份验证选项

有关如何设置和使用身份验证方法的详细信息，请单击以下相应的链接到。

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          使用Adobe IMS通过Adobe Admin Console管理AEM作者访问权限。
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        使用AEM发布服务的SAML 2.0集成，对网站用户的IDP进行身份验证。
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="令牌" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">令牌身份验证</a></strong></div>
      <p>
        允许应用程序和中间件使用API服务令牌对AEM进行身份验证。
      </p>
    </td>   
  </tr>
</table>
