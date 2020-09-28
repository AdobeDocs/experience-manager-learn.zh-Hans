---
title: 将Adobe Stock资产与AEM Assets
description: 'AEM允许用户直接从AEM搜索、预览、保存和许可Adobe Stock资产。 组织现在可以将其Adobe Stock企业计划与AEM Assets集成，以确保许可资产现在可广泛用于其创意和营销项目，并具有AEM的强大资产管理功能。 '
feature: creative-cloud-integration
topics: authoring, collaboration, operations, sharing, metadata, images, stock
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 6%

---


# 将Adobe Stock与AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2允许用户直接从AEM搜索、预览、保存和许可Adobe Stock资产。 组织现在可以将其Adobe Stock企业计划与AEM Assets集成，以确保许可资产现在可广泛用于其创意和营销项目，并具有AEM的强大资产管理功能。

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>该集成需要企 [业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4，并且至少部署了Service Pack 2。 有关AEM 6.4 Service Pack的详细信息，请参阅这 [些发行说明](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html)。

Adobe Stock和AEM Assets的集成使内容作者和营销人员能轻松许可和使用Stock资产进行创意或营销。 您可以使用Omni Search、将位置筛选器添加为Adobe Stock或通过在AEM Assets主导航中导航并单击搜索Adobe Stock珊瑚UI图标来执行Stock资产搜索。

## 功能

### 搜索并保存

* 无需离开AEM工作区即可执行Adobe Stock资产搜索。
* 节省Adobe Stock资源以用于预览，无需获得资源许可。
* 能够授权并将Adobe Stock资产保存到AEM Assets
* 能够在AEM AssetsUI内从Adobe Stock搜索类似资产
* 视图Adobe Stock网站AEM Assets内Stock Search中的选定资源
* 许可的资产文件带有蓝色许可徽章，便于识别

### 资产元数据

* 许可的资源存储在AEM Assets。 资产属性在单独的资产元数据选项卡下包含Stock元数据
* 能够向资产元数据添加许可证引用

### 资产库存用户档案

* 用户可以在“用户”>“我的首 *选项”>“库存配置”下选择“Adobe Stock”用户档案*
* 可以将强制和可选引用添加到资产授权窗口。
* 根据区域为“资产授权”窗口选择语言首选项的功能。

### 筛选器

* 用户可以根据资产类型、方向和相似视图筛选Stock资产
* 资产类型包括照片、插图、矢量、视频、模板、3D、高级、编辑
* 方向包括水平、垂直和正方形。
* 视图类似过滤器需要Adobe Stock文件号

### 访问控制

* 管理员可以在设置Adobe Stock云服务配置时向特定用户／组提供许可Stock资源的权限。
* 如果特定用户／组没有许可Stock资产的权限，则将禁 *用Stock资产搜索／资产许* 可功能。

## 与AEM Assets建立Adobe Stock{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2允许用户直接从AEM搜索、预览、保存和许可Adobe Stock资产。 此视频介绍如何使用AdobeI/O控制台在AEM Assets设置Adobe库。

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>对于Adobe Stock云服务配置，必须选择指向/content/dam的PROD环境和授权资产路径。 环境字段将在下一个AEM版本中删除，许可的资产路径是即将推出的功能的一部分，在下一个AEM版本中将引入对此字段的支持。

>[!NOTE]
>
>该集成需要企 [业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4，并且至少 [部署了Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) 。 有关AEM 6.4 Service Pack的详细信息，请参阅这 [些发行说明](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html)。 您还需要管理员权 [限才能AdobeI](https://console.adobe.io/)/O控 [制台、](https://adminconsole.adobe.com/) Adobe Admin Console和Adobe Experience Manager来设置集成。

### 安装 {#installations}

* 对于AEM 6.4，您需要安 [装AEM Service Pack](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) 2，然后重新安装cq-dam-stock-integration-content-1.0.4.zip文件。
* 确保您对AdobeI/ [O控制台](https://console.adobe.io/)、 [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager具有管理权限来设置集成。

#### 使用AdobeI/O控制台设置AdobeIMS配置 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. 在“工具”>“安全”下创建AdobeIMS **技术帐户配置**
2. 选择云 *解决方* 案 *,* 作为Adobe Stock，创建新证书或重新使用现有证书进行配置。
3. 导航到AdobeI/O控制台并为Adobe Stock创建新的服务帐户 *集成*。
4. 将证书从步骤2上传到您的Adobe Stock服务帐户集成。
5. 选择所需的Adobe Stock用户档案配置并完成服务集成。
6. 使用集成详细信息完成AdobeIMS技术帐户配置
7. 确保您可以使用访问令牌IMS技术帐户接收Adobe。

![Adobe IMS 技术帐户](assets/screen_shot_2018-10-22at12219pm.png)

#### 设置Adobe StockCloud Services {#set-up-adobe-stock-cloud-services}

1. 在“工具”>“Adobe Stock”下为Cloud Services新建一 **个云服务配置。**
2. 为您 *的Adobe Stock云配置* ，选择在上节中创建的 *AdobeIMS配置*

3. 确保选择 **环境** 作为PROD。 暂存环境不受支持，将在AEM的下一版本中删除。
4. **许可的资产路径** ，可以指向/content/dam下的任何目录。 在下一个版本的AEM中将添加对此字段的功能支持
5. 选择您的区域设置并完成设置。
6. 您还可以将用户／用户组添加到您的Adobe Stock云服务，以启用特定用户或用户组的访问权限。

![Adobe资产库存配置](assets/screen_shot_2018-10-22at12425pm.png)

### 其他资源

* [企业库存计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2发行说明](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html)
* [集成AEM和Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [AdobeI/O控制台集成API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe StockAPI文档](https://www.adobe.io/apis/creativecloud/stock/docs.html)