---
title: Adobe Asset Link和AEM
description: 设计人员和创意用户可在他们最喜爱的Adobe Experience Manager桌面应用程序中使用Adobe Creative Cloud资源。 适用于Adobe Creative Cloud企业版的Adobe Asset Link扩展增强了在Creative Cloud工具(如Adobe XD、Photoshop、InDesign和Illustrator)中搜索和浏览、排序、预览、上传资源、签出、修改、签入和查看AEM资源元数据的功能。
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '983'
ht-degree: 1%

---


# Adobe Asset Link 3.0

设计人员和创意用户可在他们最喜爱的Adobe Experience Manager桌面应用程序中使用Adobe Creative Cloud资源。

适用于Adobe Creative Cloud企业版的Adobe Asset Link扩展增强了在Creative Cloud应用程序中搜索和浏览、排序、预览、上传资源、签出、修改、签入和查看AEM资源元数据的功能。

## Adobe Asset Link功能

+ Adobe Asset Link与AEM Assets和Assets Essentials集成。
+ Adobe Asset Link自动配置与基于云的AEM环境(AEM Assets as a Cloud Service和Assets Essentials)的连接
+ Adobe Asset Link是一个可在Adobe Creative Cloud应用程序中使用的扩展：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用他们的Adobe Enterprise ID或Federated ID自动向AEM进行身份验证
+ 在AEM中浏览和搜索数字资源
+ 使用面板访问AEM中资产的文件详细信息：
   + 缩略图
   + 基本元数据
   + 版本
+ 将资产放置、下载或拖放到其布局中
+ 通过从AEM中签出资源并在其Creative Cloud Assets帐户中处理这些资源(WIP)来修改资源
+ 完成修改后将资源签回AEM，并且新版本会反映在AEM中
+ 在AEM中，通过Adobe Asset Link应用程序内面板搜索资源
+ 直接从Asset Link面板浏览AEM Assets收藏集和智能收藏集
+ 直接从面板将新创建的资源添加到AEM
+ 将资源直接拖放到InDesign框架中

## 在InDesign中置入资源

Adobe Asset Link在InDesign Asset Link和Adobe AEM之间提供直接链接支持。 借助InDesign直接链接支持，您可以放置（__放置链接的__&#x200B;或&#x200B;__放置副本__），或通过Adobe Asset Link面板将数字资产从AEM拖放到InDesign中。 此外，还引入了*For Placement Only+ (FPO)演绎版。

>[!VIDEO](https://video.tv.adobe.com/v/37235?quality=12&learn=on&captions=chi_hans)

>[!NOTE]
>
>仅使用您的Adobe Creative Cloud Enterprise ID或Federated ID。 确保您[为Adobe Asset Link](https://helpx.adobe.com/cn/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)配置AEM。

您可以使用以下选项之一将资源放置到InDesign布局中：

+ **置入副本** — 嵌入资源（使用“置入副本”选项）会在将二进制文件下载到本地系统后，将原始资源的副本置入您的InDesign布局中。 Adobe Asset Link不会维护嵌入的副本与原始资产之间的任何链接。 如果在AEM中修改了原始资源，则必须从InDesign文件中删除嵌入的资源，然后从AEM中重新嵌入该资源。

+ **置入链接的** — 使用InDesign文档时，除了直接嵌入资源外，您还可以选择引用AEM中的资源（使用上下文菜单中的“置入副本”选项）。 通过引用资源，您可以与其他用户协作，并在AEM中合并对原始资源所做的任何更新。 要从AEM引用资源，请使用上下文菜单中的置入链接选项。

### 对于仅用于置入的图像

当使用InDesign Asset Link从AEM将大资源文件置入Adobe文档时，创意用户需要在启动置入操作后等待几秒钟。 这会影响整体用户体验。 借助Adobe Asset Link，您可以暂时从AEM置入原始资源的低分辨率图像，从而减少置入图像所花费的时间。 同时，它还提高了总体用户体验和生产效率。 较低分辨率的图像是临时放置的，当需要最终输出以进行打印或发布时，您需要将FPO格式副本替换为原始图像。 如果要将多个FPO图像替换为各自的原始图像，请导航到&#x200B;**_Windows >链接_**&#x200B;面板，然后下载原始资产。 下载原始图像后，选择“Replace all FPO&#39;s With Originals（用原始图像替换所有FPO）”。

FPO演绎版是原始资源的轻型替代项。 它们具有相同的纵横比，但与原始图像相比尺寸较小。 目前，InDesign仅支持为以下图像类型导入FPO演绎版：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

如果FPO演绎版不适用于AEM中的特定资源，则会引用原始高分辨率资源。 对于FPO图像，状态FPO显示在InDesign链接面板中。

## Adobe Asset Link身份验证与AEM Assets

如何在Adobe Identity Management Services (IMS)和Adobe Author的上下文中进行Adobe Experience Manager Asset Link身份验证。

![Adobe Asset Link架构](assets/adobe-asset-link-article-understand.png)

1. Adobe Asset Link扩展通过Adobe Creative Cloud桌面应用程序向Adobe Identity Manage Service (IMS)发出授权请求，并在成功后接收持有者令牌。
1. Adobe Asset Link扩展使用扩展设置JSON中提供的方案(HTTP/HTTPS)、主机和端口，通过HTTP(S)连接到AEM Author，包括在&#x200B;**步骤1**&#x200B;中获取的持有者令牌。
1. AEM的持有者身份验证处理程序从请求中提取持有者令牌并使用Adobe IMS对其进行验证。
1. Adobe IMS验证持有者令牌后，将在AEM中创建用户（如果尚不存在），并从Adobe IMS同步配置文件和组/成员身份数据。 AEM用户会获得标准的AEM登录令牌，该令牌会作为HTTP(S)响应的Cookie发送回Adobe Asset Link扩展。
1. 后续交互(即 使用Adobe Asset Link扩展浏览、搜索、签入/签出资源等)会导致向AEM Author发送HTTP(S)请求，这些请求将使用AEM登录令牌并使用标准AEM令牌身份验证处理程序进行验证。

>[!NOTE]
>
>登录令牌到期后，**步骤1-5**&#x200B;将自动调用，使用持有者令牌验证Adobe Asset Link扩展，并重新颁发新的有效登录令牌。

## 其他资源

+ [Adobe Asset Link网站](https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html)
