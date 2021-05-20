---
title: 使用AEM体验片段设置社交发布
description: 体验片段允许营销人员将在AEM中创建的体验发布到社交媒体平台。 以下视频详细介绍将体验片段发布到Facebook和Pinterest所需的设置和配置。
sub-product: 站点，内容服务
feature: 体验片段
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: 内容管理
role: Administrator, Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# 使用体验片段{#set-up-social-posting-with-experience-fragments}设置社交发布

体验片段允许营销人员将在AEM中创建的体验发布到社交媒体平台。 以下视频详细介绍将体验片段发布到Facebook和Pinterest所需的设置和配置。

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[体验片段]  — 设置和配置Social帖子到Facebook和Pinterest*

## 配置体验片段以发布到Facebook和Pinterest的核对清单

1. AEM创作实例在HTTPS上运行
2. Facebook帐户+ Facebook开发人员应用程序
3. Pinterest帐户+ Pinterest开发人员应用程序
4. [!UICONTROL AEM云服] 务配置 — Facebook
5. [!UICONTROL AEM云服] 务配置 — Pinterest
6. AEM Experience Fragment(包含Facebook + Pinterest的Cloud Services)
7. 使用Facebook模板的体验片段变量
8. 使用Pinterest模板的体验片段变量

## 体验片段重定向URI

此URI用于Facebook和Pinterest应用程序，作为Oauth流程的一部分。

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

