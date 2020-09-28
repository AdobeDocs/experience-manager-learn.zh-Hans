---
title: 智能图像处理
description: 动态媒体经典中的智能图像功能根据客户端浏览器功能自动优化图像格式和质量，从而增强图像投放性能。 它通过利用Adobe Sensei人工智能功能并使用现有图像预设来实现这一点。 进一步了解智能图像处理，以及如何通过更快的页面加载使用它来优惠更好的客户体验。
sub-product: 动态媒体
feature: smart-crop
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 1%

---


# 智能图像处理 {#smart-imaging}

在您的网站或移动站点或应用程序上，客户体验最重要的方面之一是页面加载时间。 如果页面加载时间过长，客户通常会放弃站点或应用程序。 图像占页面加载时间的大部分。 动态媒体经典中的智能图像功能根据客户端浏览器功能自动优化图像格式和质量，从而增强图像投放性能。 它通过利用Adobe Sensei人工智能功能并使用现有图像预设来实现这一点。 智能图像处理功能可将图像大小缩减30%或更多，从而加快页面加载和改善客户体验。

Smart Imaging还得益于与Adobe一流的高级服务完全集成所带来的性能提升。 此服务在服务器、网络和对等点之间找到最佳的因特网路由，这些点的延迟和／或数据包丢失率低于因特网上的默认路由。

进一步了 [解智能成像](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 智能成像优势

由于图像是页面加载时间的大部分，因此智能图像处理的性能改进会对业务KPI产生深远影响，如更高的转化率、在网站上花费的时间以及更低的网站弹出率。

![图像](assets/smart-imaging/smart-imaging-1.png)

## 智能成像的工作原理

如前所述，智能成像利用Adobe Sensei人工智能功能并与现有的图像预设配合工作，将图像自动转换为WebP等最佳的下一代图像格式，同时保持视觉保真度。

进一步了 [解智能成像的工作方式](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支持的图像格式等详细信息（以及不使用这些格式时会发生什么情况）及其对正在使用的现有图像预设的影响。

## 智能成像的影响

您可能担心必须更改您的URL、图像预设和站点上的代码才能利用智能成像。 如果您满足使用智能成像的先决条件，并且您只能处理支持的JPEG和PNG图像格式的图像，则无需进行任何更改。

智能成像可处理通过HTTP、HTTPS和HTTP/2传送的图像。

>[!NOTE]
>
>切换到智能成像可清除CDN中的缓存。 CDN中的缓存通常在一两天内重新构建。

Smart Imaging包含在您现有的Dynamic Media Classic许可证中。 此功能不需要额外费用。 要利用它，您必须满足以下两个要求：拥有Adobe捆绑的CDN和专用域。 然后，您必须为您的帐户启用它，因为它未自动启用。

在向技术支持发送请求时启用智能成像开始，方法是发送电子邮件 [至s7support@adobe.com](mailto:s7support@adobe.com)。 他们将与您合作，以设置您将与智能成像关联的自定义域。 您将更改一个与缓存相关的参数（生存时间或TTL），并且支持将清除缓存。 如果您喜欢，还可以在推送到生产之前执行可选的暂存步骤。 然后，启用智能成像后，您将为客户提供尺寸较小的图像，但质量与客户要求的相同。 这意味着他们体验到的页面加载时间更短——所有这一切都是自动完成的，因为Adobe Sensei帮助选择最有效的大小。

启用智能成像后，您需要验证它是否按预期工作。

您可能还有其他有关智能成像的问题。 我们编译了一列表带有答案的常见问题(FAQ)。 阅读常见 [问题](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 其他资源

观看Dynamic [Media Classic优化页面性能技能构建器点播网络研讨会](https://seminars.adobeconnect.com/pzc1gw0cihpv) ，进一步了解智能成像。
