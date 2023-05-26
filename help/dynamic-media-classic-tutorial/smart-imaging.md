---
title: 智能成像
description: Dynamic Media Classic中的智能成像通过基于客户端浏览器功能自动优化图像格式和质量，增强了图像投放性能。 它通过利用Adobe Sensei AI功能并使用现有图像预设来实现这一点。 详细了解智能成像以及如何使用它通过更快的页面加载提供更好的客户体验。
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 2%

---

# 智能成像 {#smart-imaging}

客户在您网站或移动网站或应用程序上的体验的最重要方面之一是页面加载时间。 如果页面加载时间过长，客户通常会放弃网站或应用程序。 图像构成了页面加载时间的大部分。 Dynamic Media Classic中的智能成像通过基于客户端浏览器功能自动优化图像格式和质量，增强了图像投放性能。 它通过利用Adobe Sensei AI功能并使用现有图像预设来实现这一点。 智能成像将图像大小减少30%或更多，这转化为更快的页面加载和更好的客户体验。

智能成像还能够与Adobe提供的同类最佳优质服务完全集成，从而进一步提升性能。 此服务可找到服务器、网络和对等点之间的最佳互联网路由，该路由具有比Internet上的默认路由最低的延迟和/或数据包丢失率。

详细了解 [智能成像](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## 智能成像的优点

由于图像构成了页面加载时间的大多数，因此智能成像的性能改进会对业务KPI产生深远的影响，例如更高的转化率、网站逗留时间和更低的网站跳出率。

![图像](assets/smart-imaging/smart-imaging-1.png)

## 智能成像的工作原理

如前所述，智能成像利用Adobe Sensei AI功能，并与现有的图像预设一起使用，自动将图像转换为最佳的新一代图像格式，例如WebP，同时保持视觉保真度。

详细了解 [智能成像的工作原理](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支持的图像格式（以及如果您未使用这些格式时会出现什么情况）等详细信息及其对正在使用的现有图像预设的影响。

## 智能成像的影响

您可能担心必须更改网站上的URL、图像预设和代码，才能利用智能成像。 如果您满足使用智能成像的先决条件，并且只处理受支持JPEG和PNG图像格式的图像，则无需进行任何更改。

智能成像处理通过HTTP、HTTPS和HTTP/2交付的图像。

>[!NOTE]
>
>迁移到智能成像会清除CDN上的缓存。 CDN中的缓存通常在一两天内再次构建。

智能成像包含在您现有的Dynamic Media Classic许可证中。 此功能无需额外付费。 要利用此功能，您必须满足两个要求：具有Adobe捆绑的CDN和专用域。 然后，必须为帐户启用它，因为它未自动启用。

启用智能成像首先需要您通过以下方式发送技术支持请求： |创建支持案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). 支持人员将与您一起设置一个自定义域，您将与该域关联到“智能成像”。 您将更改一个与缓存（生存时间或TTL）相关的参数，支持将清除缓存。 如果您愿意，也可以在推送到生产环境之前执行可选的暂存步骤。 然后，当智能成像开启时，您将向客户提供较小大小的图像，但图像质量与他们所请求的相同。 这意味着用户体验到的页面加载速度更快 — 所有这一切都是自动完成的，因为Adobe Sensei有助于选择最有效的大小。

启用智能成像后，您需要验证它是否按预期工作。

您可能还有其他有关智能成像的问题。 我们整理了一组常见问题解答(FAQ)及其答案。 阅读 [常见问题解答](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## 其他资源

观看 [Dynamic Media Classic优化页面性能技能培养](https://seminars.adobeconnect.com/pzc1gw0cihpv) 按需网络研讨会，了解有关智能成像的更多信息。
