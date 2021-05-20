---
title: 将Adobe资产链接扩展与AEM Assets结合使用
description: 'Adobe Experience Manager资产现在可供设计人员和创意用户在其最喜爱的Adobe Creative Cloud桌面应用程序中使用。 适用于Adobe Creative Cloud企业的Adobe资产链接扩展了在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和浏览、排序、预览、上传资产、签出、修改、签入和查看AEM资产的元数据的功能。 '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: 内容管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 1%

---


# 将Adobe资产链接扩展与AEM Assets结合使用{#using-adobe-asset-link-extension-with-aem-assets}

Adobe Experience Manager资产现在可供设计人员和创意用户在其最喜爱的Adobe Creative Cloud桌面应用程序中使用。 适用于Adobe Creative Cloud企业的Adobe资产链接扩展了在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和浏览、排序、预览、上传资产、签出、修改、签入和查看AEM资产的元数据的功能。


## Adobe资产链接1.1

Adobe资产链接v1.1现在在Adobe资产链接和AEM Assets之间提供InDesign直接链接支持。 借助InDesign直接链接支持，您现在可以通过Adobe资产链接面板将数字资产放置（放置链接的资产或置入副本）或从AEM Assets中拖放到InDesign中。 此外，还引入了&#x200B;*仅用于放置*(FPO)呈现版本。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>仅使用Adobe Creative CloudEnterprise ID或Federated ID。 确保[为Adobe资产链接](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)配置AEM。


### Adobe资产链接功能

* Adobe资产链接是一种在PS、AI、ID中工作的扩展，可直接访问驻留在AEM Assets中的数字资产
* 创意人员将使用其AdobeIMSEnterprise ID或Federated ID自动登录AEM
* 除了在AEM Assets和Creative Cloud资产中进行搜索之外，创意人员还可以浏览位于AEM Assets的数字资产
* 创意人员可以访问位于AEM Assets的资产的文件详细信息；缩略图、基本元数据和版本
* 创意人员可以将资产放置、下载或拖放到其布局中
* 创意人员可以修改资产，方法是从AEM Assets中签出资产，并在其Creative Cloud资产帐户中处理资产(WIP)
* 创意人员在完成对资产的修改后，可以将资产签回AEM Assets，新版本将反映在AEM Assets中
* 支持Creative Cloud2020、2019和2018InDesign、Photoshop和Illustrator桌面应用程序
* 用户可以从Adobe资产链接应用程序内面板执行资产搜索，并根据资产大小、字母顺序和相关性对资产进行排序
* 用户可以直接从Asset Link面板访问和浏览AEM Assets收藏集和智能收藏集
* 直接从面板将新创建的资产添加到AEM Assets
* 用户可以将资产直接拖放到InDesign框架中

### 将AEM Assets置于InDesign

您可以使用以下选项之一将资产放置到InDesign布局：

* **置入副本**  — 嵌入资产（使用置入副本选项）会在将二进制文件下载到本地系统后，将原始资产的副本放置到InDesign布局中。Adobe资产链接不维护嵌入副本与原始资产之间的任何链接。 如果原始资产在AEM Assets中进行了修改，则必须从InDesign文件中删除嵌入的资产，然后从AEM Assets中重新嵌入该资产。

* **放置链接的**  — 现在，处理InDesign文档时，除了直接嵌入资产（使用上下文菜单中的“放置副本”选项）之外，您还可以选择从AEM Assets引用资产。通过引用资产，您可以与其他用户协作，并整合对AEM Assets中原始资产所做的任何更新。 要从AEM Assets中引用资产，请使用上下文菜单中的放置链接选项。

### 仅用于放置(FPO)分辨率

当使用“InDesign资产链接”将大型资产文件从AEM Assets放入“Adobe文档”中时，创意用户在启动置入操作后需要等待几秒钟。 这会影响整体用户体验。 通过Adobe资产链接，您现在可以暂时放置AEM Assets中原始资产的低分辨率图像，从而减少放置图像所花费的时间。 同时，它还提高了整体用户体验和工作效率。 将临时放置分辨率较低的图像，当打印或发布需要最终输出时，您需要将FPO呈现版本替换为原始版本。 如果要将多个FPO图像替换为相应的原始图像，请导航到&#x200B;**_Windows >链接_**&#x200B;面板，然后下载原始资产。 下载原始图像后，选择“将所有FPO替换为原件”。

>[!NOTE]
>
> *仅用于置入(FPO)* 呈现版本仅适用于“置入链接的内容”选项。您还应该在AEM Assets *Dam更新资产*&#x200B;工作流中启用FPO呈现支持。

FPO演绎版是原始资产的轻量级替代形式。 它们具有相同的宽高比，但与原始图像相比尺寸较小。 目前，InDesign仅支持为以下图像类型导入FPO演绎版：

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

如果FPO演绎版不适用于AEM Assets中的特定资产，则会引用原始高分辨率资产。 对于FPO图像，状态FPO显示在“InDesign链接”面板中。

## 了解Adobe资产链接的AEM Assets身份验证{#understanding-adobe-asset-link-authentication-with-aem-assets}

Adobe资产链接身份验证如何在AdobeIdentity Management服务(IMS)和Adobe Experience Manager作者的上下文中工作。

![Adobe资产链接架构](assets/adobe-asset-link-article-understand.png)

下载[Adobe资产链接架构](assets/adobe-asset-link-article-understand-1.png)

1. Adobe资产链接扩展通过Adobe Creative Cloud桌面应用程序发出授权请求以Adobe身份管理服务(IMS)，并在成功时接收载体令牌。
2. Adobe资产链接扩展通过HTTP(S)连接到AEM作者，包括在&#x200B;**Step 1**&#x200B;中使用扩展设置JSON中提供的方案(HTTP/HTTPS)、主机和端口获取的载体令牌。
3. AEM载体身份验证处理程序从请求中提取载体令牌，并针对AdobeIMS对其进行验证。
4. 一旦AdobeIMS验证载体令牌，则在AEM中创建用户（如果它不存在），并从AdobeIMS同步用户档案和组/成员关系数据。 AEM用户会获得一个标准AEM登录令牌，该令牌将作为HTTP(S)响应中的Cookie发送回Adobe资产链接扩展。
5. 后续交互(即 浏览、搜索、签入/签出资产等) 使用Adobe资产链接扩展时，会生成对AEM作者的HTTP(S)请求，这些请求将使用标准的AEM令牌身份验证处理程序通过AEM登录令牌进行验证。

>[!NOTE]
>
>登录令牌到期后， **步骤1-5**&#x200B;将自动调用，并使用载体令牌对Adobe资产链接扩展进行身份验证，并重新发布新的有效登录令牌。

## 其他资源{#additional-resources}

* [Adobe资产链接网站](https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html)