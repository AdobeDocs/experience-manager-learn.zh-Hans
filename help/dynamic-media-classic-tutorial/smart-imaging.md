---
title: 智能成像
description: Dynamic Media Classic中的Smart Imaging可根据客户端浏览器功能自动优化图像格式和质量，从而增强图像交付性能。 它通过利用Adobe Sensei AI功能并使用现有图像预设来实现此目的。 进一步了解智能成像，以及如何通过加快页面加载速度来使用它提供更好的客户体验。
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: 内容管理
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 1%

---


# 智能成像 {#smart-imaging}

客户在您的网站、移动设备网站或应用程序上体验的最重要方面之一是页面加载时间。 如果页面加载时间过长，客户通常会放弃网站或应用程序。 图像构成了页面加载时间的大部分。 Dynamic Media Classic中的Smart Imaging可根据客户端浏览器功能自动优化图像格式和质量，从而增强图像交付性能。 它通过利用Adobe Sensei AI功能并使用现有图像预设来实现此目的。 “智能成像”功能将图像大小减少了30%或更多，这意味着页面加载速度更快，客户体验更佳。

Smart Imaging还得益于与Adobe中同类最佳的优质服务完全集成而带来的额外性能提升。 此服务可找到在服务器、网络和对等点之间的最佳Internet路由，这些路由的延迟和/或数据包丢失率低于Internet上默认路由。

了解有关[智能成像](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html)的更多信息。

## 智能成像的优势

由于图像是页面加载时间的大部分，因此“智能成像”的性能改进可能会对业务KPI产生深远影响，例如更高的转化率、网站逗留时间和较低的网站跳出率。

![图像](assets/smart-imaging/smart-imaging-1.png)

## 智能成像的工作原理

如前所述，智能成像可利用Adobe Sensei AI功能并与现有的图像预设配合使用，将图像自动转换为最佳的下一代图像格式，如WebP，同时保持可视保真度。

进一步了解[智能成像的工作原理](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支持的图像格式等详细信息（以及如果不使用这些格式会发生什么情况）及其对现有使用的图像预设的影响。

## 智能成像的影响

您可能担心必须更改网站上的URL、图像预设和代码才能利用智能成像。 如果您满足使用智能成像的先决条件，并且您只能处理支持的JPEG和PNG图像格式的图像，则无需进行任何更改。

智能成像可处理通过HTTP、HTTPS和HTTP/2传送的图像。

>[!NOTE]
>
>移动到智能成像会清除CDN中的缓存。 CDN中的缓存通常会在一两天内重新构建。

Smart Imaging包含在您现有的Dynamic Media Classic许可证中。 此功能不需要额外付费。 要利用此功能，您必须满足以下两个要求：拥有Adobe捆绑的CDN和专用域。 然后，您必须为您的帐户启用它，因为它未自动启用。

启用智能成像时，首先需要通过 |创建支持案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。 支持团队将与您一起设置一个将与智能成像相关联的自定义域。 您将更改一个与缓存相关的参数（生存时间或TTL），并且支持将清除缓存。 如果需要，您还可以在推送到生产之前执行可选的暂存步骤。 然后，当启用“智能成像”功能后，您将为客户提供较小尺寸的图像，但质量与他们要求的相同。 这意味着他们的页面加载时间更短 — 所有这些操作都会自动完成，因为Adobe Sensei可帮助选择最高效的大小。

启用“智能成像”后，您将需要验证它是否按预期工作。

您可能还有其他有关智能成像的问题。 我们汇编了一系列常见问题解答(FAQ)及其答案。 阅读[常见问题解答](https://experienceleague.adobe.com/docs/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 其他资源

观看[Dynamic Media Classic优化页面性能技能培养网络研讨会](https://seminars.adobeconnect.com/pzc1gw0cihpv) ，了解有关智能成像的更多信息。
