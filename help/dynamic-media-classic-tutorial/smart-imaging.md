---
title: 智能成像
description: Dynamic Media Classic中的Smart Imaging可根据客户端浏览器功能自动优化图像格式和质量，从而增强图像投放性能。 它通过利用Adobe Sensei AI功能并使用现有图像预设来实现这一点。 了解有关智能图像处理的更多信息，以及如何使用智能图像处理通过更快的页面加载优惠更好的客户体验。
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: 内容管理
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 3%

---


# 智能成像 {#smart-imaging}

在您的网站或移动站点或应用程序上，客户体验最重要的方面之一是页面加载时间。 如果页面加载时间过长，客户通常会放弃网站或应用程序。 图像占页面加载时间的大部分。 Dynamic Media Classic中的Smart Imaging可根据客户端浏览器功能自动优化图像格式和质量，从而增强图像投放性能。 它通过利用Adobe Sensei AI功能并使用现有图像预设来实现这一点。 Smart Imaging将图像大小缩小30%或更多，从而加快页面加载和改善客户体验。

Smart Imaging还得益于与同类最佳的优质服务完全集成所带来的额外性能提升。 此服务在服务器、网络和对等点之间找到最佳的Internet路由，这些点与Internet上的默认路由相比具有最低的延迟和/或数据包丢失率。

了解有关[智能成像](https://docs.adobe.com/content/help/zh-Hans/experience-manager-64/assets/dynamic/imaging-faq.html)的更多信息。

## 智能成像优势

由于图像占页面加载时间的大部分，因此，智能图像处理的性能改进会对业务KPI产生深远影响，如更高的转化率、网站逗留时间和更低的网站跳出率。

![图像](assets/smart-imaging/smart-imaging-1.png)

## 智能成像的工作原理

如前所述，Smart Imaging利用Adobe Sensei AI功能并与现有的图像预设配合工作，将图像自动转换为WebP等最佳的下一代图像格式，同时保持视觉保真度。

进一步了解[智能成像的工作原理](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支持的图像格式等详细信息（以及不使用这些格式时会发生什么情况）及其对正在使用的现有图像预设的影响。

## Smart Imaging的影响

您可能担心必须更改您网站上的URL、图像预设和代码才能利用智能图像。 如果您满足使用智能成像的先决条件，并且您只能使用支持的JPEG和PNG图像格式的图像，则不必进行任何更改。

智能成像可处理通过HTTP、HTTPS和HTTP/2传送的图像。

>[!NOTE]
>
>切换到智能成像可清除CDN中的缓存。 CDN中的缓存通常在一两天内重新构建。

您现有的Dynamic Media Classic许可证中包含Smart Imaging。 此功能不需要额外的费用。 要利用它，您必须满足以下两个要求：拥有Adobe捆绑的CDN和专用域。 然后，您必须为您的帐户启用它，因为它未自动启用。

通过发送请求的技术支持，启用智能成像开始 |创建支持案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。 支持部门将与您合作设置一个自定义域，您将与智能成像关联。 您将更改一个与缓存相关的参数（生存时间或TTL），并且支持将清除缓存。 如果您喜欢，也可以在推送到生产之前执行可选的暂存步骤。 然后，当启用Smart Imaging时，您将向客户交付尺寸较小的图像，但质量与他们要求的相同。 这意味着他们的页面加载时间更短 — 所有这些操作都是自动完成的，因为Adobe Sensei可帮助选择最有效的大小。

启用智能成像后，您需要验证它是否按预期工作。

您可能还会遇到有关Smart Imaging的其他问题。 我们汇编了一列表带有答案的常见问题解答(FAQ)。 阅读[常见问题解答](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 其他资源

观看[Dynamic Media经典优化页面性能技能生成器](https://seminars.adobeconnect.com/pzc1gw0cihpv)点播网络研讨会，进一步了解智能成像。
