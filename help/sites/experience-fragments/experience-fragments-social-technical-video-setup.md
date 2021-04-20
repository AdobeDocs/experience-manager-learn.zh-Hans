---
title: 使用AEM体验片段设置社交发布
description: 体验片段使营销人员能够将在AEM中创建的体验发布到社交媒体平台。 以下视频详细介绍了将体验片段发布到Facebook和Pinterest所需的设置和配置。
sub-product: 站点，内容服务
feature: Experience Fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Content Management
role: Administrator, Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 2%

---


# 使用体验片段{#set-up-social-posting-with-experience-fragments}设置社交发布

体验片段使营销人员能够将在AEM中创建的体验发布到社交媒体平台。 以下视频详细介绍了将体验片段发布到Facebook和Pinterest所需的设置和配置。

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[体验片段]  — 设置和配置社交帖子到Facebook和Pinterest*

## 配置体验片段以发布到Facebook和Pinterest的核对清单

1. AEM作者实例正在HTTPS上运行
2. Facebook帐户+ Facebook开发人员应用程序
3. Pinterest帐户+ Pinterest开发人员应用程序
4. [!UICONTROL AEM Cloud ] Services配置 — Facebook
5. [!UICONTROL AEM Cloud ] Services配置 — Pinterest
6. AEM Experience Fragment与Facebook + PinterestCloud Services
7. 使用Facebook模板的体验片段变体
8. 使用Pinterest模板的体验片段变体

## 体验片段重定向URI

此URI用于Facebook和Pinterest应用程序，作为Oauth流的一部分。

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

