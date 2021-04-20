---
title: 在Adobe Target中使用AEM Experience Fragment优惠
seo-title: 在Adobe Target中使用AEM Experience Fragment优惠
description: Adobe Experience Manager 6.4重新构思了AEM和目标之间的个性化工作流程。 现在，在AEM中创建的体验可以作为HTML优惠直接交付到Adobe Target。 它使营销人员能够跨不同渠道无缝测试和个性化内容。
seo-description: Adobe Experience Manager 6.4重新构思了AEM和目标之间的个性化工作流程。 现在，在AEM中创建的体验可以作为HTML优惠直接交付到Adobe Target。 它使营销人员能够跨不同渠道无缝测试和个性化内容。
sub-product: 内容服务
feature: Experience Fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: Personalization
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 11%

---


# 在Adobe Target{#using-experience-fragment-offers-within-adobe-target}中使用体验片段优惠

Adobe Experience Manager 6.4重新构思了AEM和目标之间的个性化工作流程。 现在，在AEM中创建的体验可以作为HTML优惠直接交付到Adobe Target。 它使营销人员能够跨不同渠道无缝测试和个性化内容。

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>建议使用at.js客户端库，最佳实践是使用Launch by Adobe、Adobe DTM或任何第三方标签管理解决方案等标签管理解决方案将目标库添加到您的网页

>[!NOTE]
>
>Adobe Target中的AEM Experience Fragment优惠也可作为AEM 6.3用户的功能包提供。 请参见下节了解功能包和依赖项。


* Adobe Experience Manager简单易用、功能强大的内容创建机制以及Adobe Target的人工智能(AI)和机器学习帮助内容作者在一个集中位置为所有渠道创建和管理内容。 借助将Experience Fragments作为HTML优惠导出到Adobe Target的能力，营销人员现在可以更灵活地使用这些优惠创建更个性化的体验，并且现在可以测试和扩展他们创建的每个体验。
* HTML优惠和体验片段优惠的主要区别在于，只能在AEM中进行后期编辑，然后再与Adobe Target同步
* 应用于Experience Fragment文件夹的目标云服务配置会继承直接在父文件夹下创建的所有Experience Fragments。 子文件夹不继承父云服务配置。
* 为了创建个性化的优惠，我们现在可以轻松利用存储在AEM中的内容。
* 您可以创建目标活动类型，包括由Sensei提供支持的活动，如自动分配、自动目标和Automated Personalization

## AEM 6.3功能包和依赖项{#aem-feature-packs-and-dependencies}

| AEM 6.3功能包 | 依赖关系 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## 其他资源 {#additional-resources}

* [体验片段文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [使用体验片段](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
