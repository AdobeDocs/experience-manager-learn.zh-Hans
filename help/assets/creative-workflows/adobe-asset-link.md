---
title: 将Adobe Asset Link Extension与AEM Assets结合使用
description: 'Adobe Experience Manager资源现在供设计人员和创意用户在他们喜爱的Adobe Creative Cloud桌面应用程序中使用。 Adobe Asset Link Extension for Adobe Creative Cloud Enterprise扩展了在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和浏览AEM资产、排序、预览、上传资产、签出、修改、签入和视图元数据的功能。 '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1102'
ht-degree: 1%

---


# 将Adobe Asset Link Extension与AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}一起使用

Adobe Experience Manager资源现在供设计人员和创意用户在他们喜爱的Adobe Creative Cloud桌面应用程序中使用。 Adobe Asset Link Extension for Adobe Creative Cloud Enterprise扩展了在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和浏览AEM资产、排序、预览、上传资产、签出、修改、签入和视图元数据的功能。


## Adobe资产链接1.1

Adobe Asset Link v1.1现在在Adobe Asset Link和AEM Assets之间提供InDesign直接链接支持。 借助InDesign直接链接支持，您现在可以通过Adobe Asset Link面板放置（置入链接或置入副本）或将数字资产从AEM Assets拖放到InDesign中。 此外，还引入了&#x200B;*仅用于放置*(FPO)再现。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>仅使用Adobe Creative CloudEnterprise ID或Federated ID。 确保[配置AEM以用于Adobe资产链接](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)。


### Adobe资产链接功能

* Adobe Asset Link是一款可在PS、AI、ID中使用的扩展，可提供对驻留在AEM Assets中的数字资产的直接访问
* 创意人员将使用其Adobe IMSEnterprise ID或Federated ID自动登录AEM
* 除了在AEM Assets和Creative Cloud资产中进行搜索外，创意人员还可以浏览AEM Assets中的数字资产
* 创意人员可以访问驻留在AEM Assets的资产的文件详细信息；缩略图、基本元数据和面板中的版本
* 创意人员可以放置、下载资产或将资产拖放到版面中
* 创意人员可以修改资产，方法是从AEM Assets签出资产，并在其“Creative Cloud资产”帐户中处理这些资产(WIP)
* 创意人员在完成修改后，可以将资产签回AEM Assets，新版本将反映在AEM Assets中
* 支持Creative Cloud 2020、2019和2018InDesign、Photoshop和Illustrator桌面应用程序
* 用户可以从“Adobe资产链接应用程序内”面板中执行资产搜索，并根据大小、字母顺序和相关性对它们进行排序
* 用户可以直接从“资源链接”面板访问和浏览AEM Assets集合和智能集合
* 直接从面板将新创建的资源添加到AEM Assets
* 用户可以将资源直接拖放到InDesign框架中

### 将AEM Assets置入InDesign

您可以使用以下选项之一将资产放置到InDesign布局中：

* **置入副本**  — 嵌入资产（使用置入副本选项）会在将二进制文件下载到本地系统后，将原始资产的副本放置到您的InDesign布局中。Adobe资产链接不会维护嵌入副本与原始资产之间的任何链接。 如果原始资产在AEM Assets中进行了修改，则必须从InDesign文件中删除嵌入的资产，然后从AEM Assets中重新嵌入该资产。

* **置入链接**  — 使用InDesign文档时，除了直接嵌入资产（使用上下文菜单中的置入复制选项），您现在还可以选择从AEM Assets引用资产。通过引用资产，您可以与其他用户协作，并合并对AEM Assets中原始资产所做的任何更新。 要从AEM Assets引用资源，请使用上下文菜单中的放置链接选项。

### 仅用于放置(FPO)分辨率

当使用Adobe Asset Link将大型资源文件从AEM Assets放入InDesign文档时，创意用户需要在启动置入操作后等待几秒钟。 这会影响整体用户体验。 借助Adobe Asset Link，您现在可以暂时放置AEM Assets中原始资产的低分辨率图像，从而缩短放置图像所花的时间。 同时，它提高了整体用户体验和工作效率。 低分辨率图像将临时放置，当打印或发布需要最终输出时，您需要将FPO再现替换为原始图像。 如果要将多个FPO图像替换为相应的原始图像，请导航到&#x200B;**_Windows >链接_**&#x200B;面板，然后下载原始资源。 下载原始图像后，选择“将所有FPO替换为原始图像”。

>[!NOTE]
>
> *仅适用于“置入链接”(FPO)* 再现仅适用于“置入链接”选项。您还应在AEM Assets *Dam更新资产*&#x200B;工作流中启用FPO再现支持。

FPO演绎版是原始资产的轻量级替代。 它们具有相同的长宽比，但与原始图像相比尺寸较小。 目前，InDesign仅支持为以下图像类型导入FPO演绎版：

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

如果FPO演绎版不适用于AEM Assets中的特定资产，则引用原始高分辨率资产。 对于FPO图像，状态FPO显示在“InDesign链接”面板中。

## 了解Adobe Asset Link身份验证与AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Adobe Asset Link身份验证在Adobe Identity Management Services(IMS)和Adobe Experience Manager Author上下文中的工作方式。

![Adobe资产链接架构](assets/adobe-asset-link-article-understand.png)

下载[Adobe资产链接架构](assets/adobe-asset-link-article-understand-1.png)

1. Adobe Asset Link扩展通过Adobe Creative Cloud桌面应用程序向Adobe Identity Manage Service(IMS)发出授权请求，并在成功后接收Bearer令牌。
2. Adobe资产链接扩展通过HTTP(S)连接到AEM作者，包括使用方案(HTTP/HTTPS)在&#x200B;**步骤1**&#x200B;中获取的承载令牌、扩展设置JSON中提供的主机和端口。
3. AEM Bearer Authentication Handler从请求中提取Bearer令牌，并针对Adobe IMS验证它。
4. 一旦Adobe IMS验证了Bearer令牌，将在AEM中创建用户（如果它尚不存在），并同步AdobeIMS的用户档案和组/成员关系数据。 AEM用户会获得一个标准AEM登录令牌，该令牌会作为HTTP(S)响应的Cookie发送回Adobe Asset Link扩展。
5. 后续交互(即 浏览、搜索、登记/注销资产等) 使用Adobe Asset Link扩展时，会生成对AEM作者的HTTP请求，这些请求使用标准的AEM令牌身份验证处理程序通过AEM登录令牌验证。

>[!NOTE]
>
>登录令牌到期后， **步骤1-5**&#x200B;将自动调用，使用承载令牌验证Adobe资产链接扩展，并重新发布新的有效登录令牌。

## 其他资源{#additional-resources}

* [Adobe资产链接网站](https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html)