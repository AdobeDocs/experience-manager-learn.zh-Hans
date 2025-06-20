---
title: 在AEM中创建Adobe Target Cloud Service帐户
description: 使用Adobe Experience Manager as a Cloud Service和Adobe IMS身份验证将Cloud Service与Adobe Target集成。
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 8ebaba01d4b470a1ca7c1630f55b756796f3640d
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 8%

---

# 创建 Adobe Target 云服务帐户 {#adobe-target-cloud-service}

以下视频提供了有关如何将AEM as a Cloud Service与Adobe Target连接的演练。

此集成允许AEM创作服务直接与Adobe Target通信，并将体验片段作为选件从AEM推送到Target。  此集成&#x200B;*不*&#x200B;将Adobe Target JavaScript (AT.js)添加到AEM Sites网页，针对该集成[AEM和使用Target扩展的标记](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md)。

>[!WARNING]
>
>视频演示了用于将AEM连接到Adobe Target的已弃用JWT身份验证方法。 但是，推荐的方法是使用OAuth服务器到服务器身份验证方法。 有关详细信息，请参阅[AEM的JWT-To-OAuth凭据迁移](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration)。 我们正在更新视频以反映此更改。


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>视频中显示的Adobe Target云服务配置存在一个已知问题。 在解决此问题之前，请执行视频中的相同步骤，但使用[旧版Adobe Target云服务配置](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html?lang=zh-Hans)。
