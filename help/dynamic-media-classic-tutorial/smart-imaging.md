---
title: 智能图像处理
description: Dynamic Media Classic中的“智能成像”通过基于客户端浏览器功能自动优化图像格式和质量，增强了图像投放性能。 它通过使用现有的图像预设来实现这一点。 详细了解智能成像，以及如何使用智能成像通过更快的页面加载提供更好的客户体验。
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
duration: 130
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 1%

---

# 智能图像处理 {#smart-imaging}

客户在您网站、移动网站或应用程序上的体验的最重要方面之一是页面加载时间。 如果页面加载时间过长，客户通常会放弃网站或应用程序。 图像构成了页面加载时间的大部分。 Dynamic Media Classic中的“智能成像”通过基于客户端浏览器功能自动优化图像格式和质量，增强了图像投放性能。 智能成像将图像大小减少30%或更多 — 这随之而来的是更快的页面加载和更好的客户体验。

智能成像还能够与Adobe提供的同类最佳优质服务完全集成，从而进一步提升性能。 此服务在服务器、网络和对等点之间找到最佳的Internet路由，该路由具有比Internet上的默认路由最低的延迟和/或数据包丢失率。

了解有关[智能成像](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)的更多信息。

## 智能成像优势

由于图像构成了页面加载时间的大多数，因此智能成像的性能改进可能会对业务KPI产生深远影响，例如转化率高、网站逗留时间和网站跳出率低。

![图像](assets/smart-imaging/smart-imaging-1.png)

## 智能成像的工作原理

如前所述，智能成像可与现有的图像预设配合使用，自动将图像转换为最佳的新一代图像格式（如WebP），同时保持可视保真度。

了解有关[智能成像的工作方式](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)的详细信息，包括支持的图像格式（以及如果不使用这些格式将发生什么情况）及其对正在使用的现有图像预设的影响。

## 智能成像的影响

您可能担心必须对网站上的URL、图像预设和代码进行更改才能利用智能成像。 如果您满足使用智能成像的先决条件，并且只处理受支持JPEG和PNG图像格式的图像，则无需进行任何更改。

智能成像处理通过HTTP、HTTPS和HTTP/2交付的图像。

>[!NOTE]
>
>迁移到智能成像会清除CDN上的缓存。 CDN中的缓存通常在一两天内再次构建。

智能成像包含在您现有的Dynamic Media Classic许可证中。 此功能不需要额外付费。 要利用此功能，您必须满足两个要求：拥有Adobe捆绑的CDN和专用域。 然后，必须为帐户启用它，因为它未自动启用。

要启用智能成像，您需要发送技术支持请求，请求者： |创建支持案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/cn/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。 支持人员将与您合作，一起设置一个自定义域，您将与该域关联到“智能成像”。 您将更改一个与缓存（生存时间或TTL）相关的参数，支持将清除缓存。 如果您愿意，也可以在推送到生产环境之前执行可选的暂存步骤。 然后，当智能成像开启时，您将向客户提供较小大小的图像，但图像质量与他们所请求的相同。 这意味着客户体验到的页面加载速度更快 — 而且所有这些都是自动完成的。

启用智能成像后，您需要验证它是否按预期工作。

您可能还有其他有关智能成像的问题。 我们整理了一个包含答案的常见问题解答(FAQ)列表。 阅读[常见问题解答](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)。

## 其他资源

观看[Dynamic Media Classic优化页面性能技能强化](https://seminars.adobeconnect.com/pzc1gw0cihpv)按需网络研讨会，了解有关智能成像的更多信息。
