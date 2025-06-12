---
title: Adobe Asset Link 和 AEM
description: 设计师和创意用户可以在他们喜爱的 Adobe Creative Cloud Desktop 应用程序中使用 Adobe Experience Manager Assets。适用于 Adobe Creative Cloud 企业版的 Adobe Asset Link 扩展增强了在 Creative Cloud 工具中（Adobe XD, Photoshop, InDesign 和 Illustrator）搜索和浏览、排序、预览、上传资产、签出、修改、签入和查看 AEM 资产元数据的能力。
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '983'
ht-degree: 100%

---


# Adobe Asset Link 3.0

设计师和创意用户可以在他们喜爱的 Adobe Creative Cloud Desktop 应用程序中使用 Adobe Experience Manager Assets。

适用于 Adobe Creative Cloud 企业版的 Adobe Asset Link 扩展增强了在 Creative Cloud 应用程序中搜索和浏览、排序、预览、上传资产、签出、修改、签入和查看 AEM 资产元数据的能力。

## Adobe Asset Link 功能

+ Adobe Asset Link 与 AEM Assets 和 Assets Essentials 集成。
+ Adobe Asset Link 自动配置与基于云的 AEM 环境（AEM Assets as a Cloud Service 和 Assets Essentials）的连接
+ Adobe Asset Link 是一款可在 Adobe Creative Cloud 应用程序中运行的扩展程序：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用 Adobe Enterprise ID 或 Federated ID 对 AEM 进行自动身份验证
+ 在 AEM 中浏览和搜索数字资产
+ 通过面板访问驻留在 AEM 中的资产的详细文件信息：
   + 缩略图
   + 基本元数据
   + 版本
+ 将资产放置、下载或拖放到其版面中
+ 通过从 AEM 中签出资产，并在其 Creative Cloud 资产账户中进行编辑 (WIP) 来修改资产
+ 在完成对资产的修改后，将其重新签入 AEM，新版本将会在 AEM 中得以体现
+ 从 Adobe Asset Link 应用程序内的面板中搜索 AEM 中的资源
+ 直接从 Asset Link 面板浏览 AEM 资产集合和智能集合
+ 直接从面板将新创建的资产添加到 AEM
+ 将资源直接拖放到 InDesign 框架中

## 在 InDesign 中放置资源

Adobe Asset Link 提供 Adobe Asset Link 与 AEM 之间的 InDesign 直接链接支持。借助 InDesign 直接链接支持，您可以通过 Adobe Asset Link 面板，从 AEM 向 InDesign 放置（__放置链接__&#x200B;或&#x200B;__放置副本__）或拖放数字资产。此外，还引入了“仅供放置” (FPO) 演绎版。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>仅使用您的 Adobe Creative Cloud Enterprise ID 或 Federated ID。确保你[为 Adobe Asset Link 配置 AEM](https://helpx.adobe.com/cn/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)。

您可以使用以下选项之一将资产放置到 InDesign 版面中：

+ **放置副本** - 在将二进制文件下载到本地系统后，使用“放置副本”选项将原始资产的副本嵌入到您的 InDesign 布局中。Adobe Asset Link 不会维护嵌入副本与原始资产之间的任何链接。如果原始资产在 AEM 中进行了修改，您必须从 InDesign 文件中删除嵌入的资产，然后从 AEM 中重新嵌入该资产。

+ **放置链接** - 在处理 InDesign 文档时，除了直接嵌入资源（使用上下文菜单中的“放置副本”选项）外，您还可以选择引用 AEM 中的资产。通过引用资产，您可以与其他用户协作，并将对 AEM 中原始资源所做的任何更新纳入其中。要引用 AEM 中的资源，请使用上下文菜单中的“放置链接”选项。

### 仅供放置的图像

当使用 Adobe Asset Link 将大型资产文件从 AEM 放入 InDesign 文档时，创意用户在启动放置操作后需要等待几秒钟。这会影响整体用户体验。借助 Adobe Asset Link，您可以从 AEM 临时放置原始资产的低分辨率图像，从而缩短放置图像所需的时间。同时，这还能提升整体用户体验和工作效率。低分辨率图像是临时放置的，当需要最终输出进行打印或发布时，您需要用原始图像替换 FPO 演绎版的图像。如果您想用相应的原始图像替换多个 FPO 图像，请导航至 **_Windows > 链接_**&#x200B;面板，然后下载原始资产。下载原始图像后，选择“将所有 FPO 替换为原始图像”。

FPO 演绎版是原始资产的轻量级替代品。它们具有相同的宽高比，但与原始图像相比，尺寸更小。目前，InDesign 仅支持导入以下图像类型的 FPO 演绎版：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

如果 AEM 中的特定资产没有 FPO 演绎版，则将引用原始的高分辨率资产。对于 FPO 图像，InDesign 的“链接”面板中会显示 FPO 状态。

## 使用 AEM Assets 进行 Adobe Asset Link 身份验证

Adobe Asset Link 身份验证在 Adobe Identity Management Services (IMS) 和 Adobe Experience Manager Author 中的应用方式。

![Adobe Asset Link 架构](assets/adobe-asset-link-article-understand.png)

1. Adobe Asset Link 扩展程序通过 Adobe Creative Cloud Desktop 应用程序向 Adobe Identity Manage Service (IMS) 发出授权请求，并在请求成功后接收一个 Bearer 令牌。
1. Adobe Asset Link 扩展程序通过 HTTP(S) 连接到 AEM Author，其中包括在&#x200B;**步骤 1**&#x200B;中获取的 Bearer 令牌，使用扩展程序设置 JSON 中提供的架构 (HTTP/HTTPS)、主机和端口。
1. AEM 的 Bearer 身份验证处理程序从请求中提取 Bearer 令牌，并通过 Adobe IMS 进行验证。
1. 一旦 Adobe IMS 验证了 Bearer 令牌，如果 AEM 中不存在该用户，则会在 AEM 中创建一个用户，并从 Adobe IMS 同步轮廓和群组/成员资格数据。AEM 用户会获得一个标准的 AEM 登录令牌，该令牌会作为 HTTP(S) 响应中的 Cookie 发送回 Adobe Asset Link 扩展程序。
1. 随后，通过 Adobe Asset Link 扩展程序进行的交互（如浏览、搜索、资产签入/签出等）会向 AEM Author 发起 HTTP(S) 请求，这些请求会使用 AEM 登录令牌，并通过标准的 AEM 令牌身份验证处理程序进行验证。

>[!NOTE]
>
>登录令牌过期后，**步骤 1-5** 将自动调用，并使用 Bearer 令牌对 Adobe Asset Link 扩展程序进行身份验证，并重新颁发一个新的有效登录令牌。

## 其他资源

+ [Adobe Asset Link 网站](https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html)
