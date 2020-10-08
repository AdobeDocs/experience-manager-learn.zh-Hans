---
title: 使用AEM Experience Fragments设置社交发布
description: 体验片段使营销人员能够将AEM中创建的体验发布到社交媒体平台。 以下视频详细介绍了将体验片段发布到Facebook和Pinterest所需的设置和配置。
sub-product: 站点，内容服务
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# 设置包含体验片段的社交发布 {#set-up-social-posting-with-experience-fragments}

体验片段使营销人员能够将AEM中创建的体验发布到社交媒体平台。 以下视频详细介绍了将体验片段发布到Facebook和Pinterest所需的设置和配置。

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[体验片段]-设置和配置社交帖子到Facebook和Pinterest*

## 将体验片段配置为发布到Facebook和Pinterest的核对清单

1. AEM作者实例正在HTTPS上运行
2. Facebook帐户+ Facebook开发人员应用程序
3. Pinterest帐户+ Pinterest开发人员应用程序
4. [!UICONTROL AEM Cloud Services] Configuration - Facebook
5. [!UICONTROL AEM Cloud Services] Configuration - Pinterest
6. AEM Experience Fragment与Facebook + PinterestCloud Services
7. 使用Facebook模板的体验片段变体
8. 使用Pinterest模板的体验片段变化

## 体验片段重定向URI

此URI用于Facebook和Pinterest应用程序，作为Oauth流的一部分。

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

