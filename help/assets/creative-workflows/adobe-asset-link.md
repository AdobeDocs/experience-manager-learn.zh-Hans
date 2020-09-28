---
title: 将Adobe资产链接扩展与AEM Assets
description: 'Adobe Experience Manager资源现在可供设计人员和创意用户在他们喜爱的Adobe Creative Cloud桌面应用程序中使用。 Adobe Creative Cloud企业的Adobe资产链接扩展扩展了在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和浏览AEM资产、排序、预览、上传资产、签出、修改、签入和视图资产元数据的功能。 '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 1%

---


# 将Adobe资产链接扩展与AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Adobe Experience Manager资源现在可供设计人员和创意用户在他们喜爱的Adobe Creative Cloud桌面应用程序中使用。 Adobe Creative Cloud企业的Adobe资产链接扩展扩展了在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和浏览AEM资产、排序、预览、上传资产、签出、修改、签入和视图资产元数据的功能。


## Adobe资产链接1.1

Adobe资产链接v1.1现在在Adobe资产链接和AEM Assets之间提供InDesign直接链接支持。 借助InDesign直接链接支持，您现在可以通过“InDesign资产链接”面板将数字资产放置（放置链接或置入副本）或从AEM Assets拖放到Adobe。 此外，还引入 *“仅用于放置* (FPO)”再现。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>仅使用您的Adobe Creative CloudEnterprise ID或Federated ID。 确保为 [Adobe资产链接配置AEM](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html)。


### Adobe资产链接功能

* Adobe资产链接是一种扩展，可在PS、AI、ID中使用，并提供对驻留在AEM Assets的数字资产的直接访问
* 创意人员将使用其AdobeIMSEnterprise ID或Federated ID自动登录AEM
* 除了在AEM Assets和Creative Cloud资产中进行搜索外，创意人员还可以浏览AEM Assets的数字资产
* 创意人员可以访问居住在AEM Assets的资产的文件详细信息；缩略图、基本元数据和面板中的版本
* 创意人员可以放置、下载资产或将资产拖放到版面中
* 创意人员可以修改资产，方法是从AEM Assets签出资产，并在其Creative Cloud资产帐户中处理这些资产(WIP)
* 创意人员完成修改后，可以将资产签回AEM Assets，新版本将反映在AEM Assets
* 支持Creative Cloud2020、2019和2018InDesign、Photoshop和Illustrator桌面应用程序
* 用户可以从“Adobe资产链接应用程序内”面板执行资产搜索，并根据大小、字母顺序和相关性对它们进行排序
* 用户可以直接从“资产链接”面板访问和浏览AEM Assets集合和智能集合
* 直接从小组将新创建的资源添加到AEM Assets
* 用户可以将资产直接拖放到InDesign框架中

### 让AEM Assets进入InDesign

您可以使用以下选项之一将资产放置到InDesign布局中：

* **置入副本** -嵌入资产（使用置入副本选项）会在将二进制文件下载到本地系统后，将原始资产的副本放置到InDesign布局中。 Adobe资产链接不维护嵌入副本与原始资产之间的任何链接。 如果原始资产在AEM Assets进行了修改，则必须从InDesign文件中删除嵌入的资产，并从AEM Assets重新嵌入该资产。

* **置入链接** -在使用InDesign文档时，除了直接嵌入资产（使用上下文菜单中的置入副本选项）外，您现在还可以选择引用AEM Assets的资产。 通过引用资产，您可以与其他用户协作并纳入对AEM Assets原始资产所做的任何更新。 要引用AEM Assets的资产，请使用上下文菜单中的“放置链接的资产”选项。

### 仅用于放置(FPO)分辨率

当使用Adobe资产链接将大型资产文件从AEM Assets放入InDesign文档时，创意用户需要在启动置入操作后等待几秒钟。 这会影响整体用户体验。 利用Adobe资产链接，您现在可以暂时放置AEM Assets原始资产的低分辨率图像，从而缩短放置图像所花费的时间。 同时，它提高了整体用户体验和工作效率。 低分辨率图像将临时放置，当打印或发布需要最终输出时，您需要将FPO再现替换为原始图像。 如果要将多个FPO图像替换为相应的原始图像，请导航到 **_Windows >链接面板_** ，然后下载原始资源。 下载原始图像后，选择“Replace all FPO&#39;s With Originals（用原始图像替换所有FPO）”。

>[!NOTE]
>
> *仅适用于置入(FPO)* ，再现仅适用于“置入链接”选项。 您还应在AEM AssetsDam更新资产工作流中启 *用FPO再现支* 持。

FPO演绎版是原始资产的轻量级替代。 它们具有相同的长宽比，但与原始图像相比尺寸较小。 目前，InDesign仅支持为以下图像类型导入FPO演绎版：

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

如果FPO再现不适用于AEM Assets的特定资产，则会引用原始高分辨率资产。 对于FPO图像，状态FPO显示在“InDesign链接”面板中。



## 了解Adobe资产链接身份验证与AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Adobe资产链接身份验证如何在AdobeIdentity Management服务(IMS)和Adobe Experience Manager作者的环境中工作。

![Adobe资产链接架构](assets/adobe-asset-link-article-understand.png)

下载 [Adobe资产链接体系结构](assets/adobe-asset-link-article-understand-1.png)

1. Adobe资产链接扩展通过Adobe Creative Cloud桌面应用程序向Adobe标识管理服务(IMS)发出授权请求，并在成功时接收承载令牌。
2. Adobe资产链接扩展通过HTTP(S)连接到AEM作者，包括在 **步骤** 1中使用扩展设置JSON中提供的方案(HTTP/HTTPS)获取的承载令牌。
3. AEM的承载身份验证处理程序从请求中提取承载令牌，并针对AdobeIMS验证它。
4. 一旦AdobeIMS验证Bearer令牌，将在AEM中创建用户（如果尚不存在），并同步用户档案和AdobeIMS的组／成员数据。 AEM用户将获得一个标准AEM登录令牌，该令牌作为HTTP(S)响应上的Cookie发送回Adobe资产链接扩展。
5. 后续交互(即 浏览、搜索、登记／注销资产等) adobe资产链接扩展导致对AEM作者的HTTP请求，这些请求使用AEM登录令牌(使用标准的AEM令牌身份验证处理程序)进行验证。

>[!NOTE]
>
>登录令牌到期后， **步骤1-5** 将自动调用，使用承载令牌验证Adobe资产链接扩展，并重新发布新的有效登录令牌。

## 其他资源{#additional-resources}

* [Adobe资产链接网站](https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html)