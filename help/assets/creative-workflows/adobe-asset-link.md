---
title: AdobeAsset Link和AEM
description: 设计人员和创意用户可在他们最喜爱的Adobe Experience Manager桌面应用程序中使用Adobe Creative Cloud资源。 适用于Adobe Creative Cloud for enterprise的AdobeAsset Link扩展增强了在Adobe XD、Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和浏览、排序、预览、上传资源、签出、修改、签入和查看AEM资源元数据的功能。
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
source-git-commit: 8dfc538e93ea5dc114cf0a5d57dd82d924e476ff
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 2%

---

# AdobeAsset Link 3.0

设计人员和创意用户可在他们最喜爱的Adobe Experience Manager桌面应用程序中使用Adobe Creative Cloud资源。

适用于Adobe Creative Cloud for enterprise的AdobeAsset Link扩展增强了在Creative Cloud应用程序中搜索和浏览、排序、预览、上传资源、签出、修改、签入和查看AEM资源元数据的功能。

>[!TIP]
>
> 进一步了解 [Adobe XD Premium培训方案](https://helpx.adobe.com/support/xd.html) 可以帮助您将Asset Link与Adobe Experience Manager工作流集成。

## AdobeAsset Link和AEM创意工作流

以下视频演示了创意人员在Adobe Creative Cloud应用程序中工作并使用AdobeAsset Link直接与AEM集成时所使用的常见工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## AdobeAsset Link功能

+ Adobe Asset Link与AEM Assets和Assets Essentials集成。
+ Adobe Asset Link自动配置与基于云的AEM环境(AEM Assetsas a Cloud Service和Assets Essentials)的连接
+ AdobeAsset Link是一个可在Adobe Creative Cloud应用程序中使用的扩展：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用其AdobeEnterprise ID或Federated ID自动向AEM进行身份验证
+ 在AEM中浏览和搜索数字资源
+ 使用面板访问AEM中资产的文件详细信息：
   + 缩略图
   + 基本元数据
   + 版本
+ 将资产放置、下载或拖放到其布局中
+ 通过从AEM中签出资源并在其Creative CloudAssets帐户中处理这些资源(WIP)来修改资源
+ 完成修改后，将资源签回AEM，新版本将反映在AEM中
+ 在AEM中通过AdobeAsset Link应用程序内面板搜索资源
+ 直接从Asset Link面板浏览AEM Assets收藏集和智能收藏集
+ 直接从面板将新创建的资源添加到AEM
+ 将资源直接拖放到InDesign框架中

## 将资源置于InDesign中

AdobeAsset Link在AdobeAsset Link和AEM之间提供InDesign直接链接支持。 借助InDesign直接链接支持，您可以放置(__置入链接的对象__ 或 __置入副本__)或通过AdobeAsset Link面板将数字资源从AEM拖放到InDesign中。 此外，还引入了*For Placement Only+ (FPO)演绎版。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>仅使用您的Adobe Creative CloudEnterprise ID或Federated ID。 确保您 [为AdobeAsset Link配置AEM](https://helpx.adobe.com/cn/enterprise/using/adobe-asset-link.html).

您可以使用以下选项之一将资源置于InDesign布局中：

+ **置入副本**  — 嵌入资源（使用“置入副本”选项）会在将二进制文件下载到本地InDesign后，将原始资源的副本置入系统布局中。 AdobeAsset Link不会维护嵌入的副本与原始资源之间的任何链接。 如果在AEM中修改了原始资源，则必须从InDesign文件中删除嵌入的资源，然后从AEM中重新嵌入该资源。

+ **置入链接的对象**  — 使用InDesign文档时，除了直接嵌入资源之外，您还可以选择引用AEM中的资源（使用上下文菜单中的置入复制选项）。 通过引用资源，您可以与其他用户协作，并在AEM中合并对原始资源所做的任何更新。 要从AEM引用资源，请使用上下文菜单中的置入链接选项。

### 对于仅用于置入的图像

当使用AdobeAsset Link从AEM将大型资源文件置入InDesign文档时，创意用户需要在启动置入操作后等待几秒钟。 这会影响整体用户体验。 通过AdobeAsset Link ，您可以暂时从AEM置入原始资源的低分辨率图像，从而减少置入图像所需的时间。 同时，它还提高了总体用户体验和生产效率。 较低分辨率的图像是临时放置的，当需要最终输出以进行打印或发布时，您需要将FPO格式副本替换为原始图像。 如果要将多个FPO图像替换为各自的原始图像，请导航至 **_“窗口”>“链接”_** 面板，然后下载原始资源。 下载原始图像后，选择“Replace all FPO&#39;s With Originals（用原始图像替换所有FPO）”。

FPO演绎版是原始资源的轻型替代项。 它们具有相同的纵横比，但与原始图像相比尺寸较小。 目前，InDesign仅支持为以下图像类型导入FPO演绎版：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

如果FPO演绎版对AEM中的特定资源不可用，则会引用原始高分辨率资源。 对于FPO图像，状态FPO显示在“InDesign链接”面板中。

## 使用AEM AssetsAdobeAsset Link身份验证

如何在AdobeIdentity Management Services (IMS)和Adobe Experience Manager Author的上下文中进行AdobeAsset Link身份验证。

![AdobeAsset Link架构](assets/adobe-asset-link-article-understand.png)

1. AdobeAsset Link扩展通过Adobe Creative Cloud桌面应用程序发出授权请求以AdobeIdentity Manage Service (IMS)，并在成功时接收持有者令牌。
1. AdobeAsset Link扩展通过HTTP(S)连接到AEM Author，包括在中获取的持有者令牌 **步骤1**，使用扩展设置JSON中提供的方案(HTTP/HTTPS)、主机和端口。
1. AEM持有者身份验证处理程序从请求中提取持有者令牌并使用Adobe IMS对其进行验证。
1. Adobe IMS验证持有者令牌后，将在AEM中创建用户（如果尚不存在），并从Adobe IMS同步配置文件和组/成员身份数据。 AEM用户会获得标准的AEM登录令牌，该令牌会作为HTTP(S)响应上的Cookie发送回AdobeAsset Link扩展。
1. 后续交互(即 浏览、搜索、签入/签出资产等) 使用AdobeAsset Link扩展时，会生成对AEM Author的HTTP(S)请求，并且这些请求会使用标准AEM令牌身份验证处理程序通过AEM登录令牌进行验证。

>[!NOTE]
>
>登录令牌到期后， **步骤1-5** 将自动调用，使用持有者令牌对AdobeAsset Link扩展进行身份验证，并重新颁发新的有效登录令牌。

## 其他资源

+ [AdobeAsset Link网站](https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html)
