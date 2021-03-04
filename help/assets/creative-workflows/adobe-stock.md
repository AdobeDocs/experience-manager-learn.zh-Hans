---
title: 将Adobe Stock资源与AEM Assets结合使用
description: 'AEM使用户能够直接从AEM中搜索、预览、保存和许可Adobe Stock资产。 组织现在可以将其Adobe Stock企业计划与AEM Assets集成，以确保许可的资产现在可广泛用于其创意和营销项目，并具备AEM的强大资产管理功能。 '
feature: Adobe Stock
version: 6.4, 6.5
topic: 内容管理
role: 业务从业者
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 6%

---


# 将Adobe Stock用于AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2使用户能够直接从AEM中搜索、预览、保存和许可Adobe Stock资源。 组织现在可以将其Adobe Stock企业计划与AEM Assets集成，以确保许可的资产现在可广泛用于其创意和营销项目，并具备AEM的强大资产管理功能。

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>集成需要[企业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)和至少部署了Service Pack 2的AEM 6.4。 有关AEM 6.4 Service Pack的详细信息，请参阅这些[发行说明](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)。

Adobe Stock和AEM Assets集成使内容作者和营销人员能轻松许可和使用stock资产进行创意或营销。 您可以使用Omni Search、将位置筛选器添加为Adobe Stock或通过在AEM Assets主导航中导航并单击搜索Adobe Stock Coral UI图标来执行Stock资源搜索。

## 功能

### 搜索并保存

* 执行Adobe Stock资产搜索而不离开AEM工作区。
* 保存Adobe Stock资源以用于预览，无需许可该资源。
* 能够许可Adobe Stock资源并将其保存到AEM Assets
* 能在AEM Assets UI中从Adobe Stock搜索类似资产
* 视图Adobe Stock网站上AEM Assets内的Stock Search中的选定资源
* 许可的资源文件标有蓝色许可徽章，便于识别

### 资产元数据

* 许可的资源存储在AEM Assets中。 资产属性在单独的资产元数据选项卡下包含Stock元数据
* 能够向资产元数据添加许可证引用

### 资产库存用户档案

* 用户可以在&#x200B;*“用户”>“我的首选项”>“Stock配置”*&#x200B;下选择Adobe Stock 用户档案
* 可以将强制引用和可选引用添加到“资产许可”窗口。
* 可以根据区域为“资产授权”窗口选择语言首选项。

### 筛选器

* 用户可以根据资产类型、方向和类似视图筛选库存资产
* 资源类型包括照片、插图、矢量、视频、模板、3D、高级、编辑
* 方向包括“水平”、“垂直”和“正方形”。
* 视图类似过滤器需要Adobe Stock文件号

### 访问控制

* 管理员可以在设置Adobe Stock云服务配置时向特定用户/组提供许可Stock资产的权限。
* 如果特定用户/组没有许可Stock资源的权限，将禁用&#x200B;*Stock资源搜索/资产许可*&#x200B;功能。

## 使用AEM Assets{#set-up-adobe-stock-with-aem-assets}设置Adobe Stock

AEM 6.4.2使用户能够直接从AEM中搜索、预览、保存和许可Adobe Stock资源。 此视频介绍如何使用Adobe Console在AEM Assets中设置Adobe I/O Stock。

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>对于Adobe Stock Cloud服务配置，必须选择指向/content/dam的PROD环境和授权资产路径。 环境字段将在下一个AEM版本中删除，许可的资产路径是即将推出的功能的一部分，下一个AEM版本中将引入对此字段的支持。

>[!NOTE]
>
>该集成需要[企业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)和至少部署了[Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0)的AEM 6.4。 有关AEM 6.4 Service Pack的详细信息，请参阅这些[发行说明](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)。 您还需要对[Adobe I/O Console](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)和Adobe Experience Manager的管理员权限才能设置集成。

### 安装{#installations}

* 对于AEM 6.4，您需要安装[AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0)，然后重新安装cq-dam-stock-integration-content-1.0.4.zip文件。
* 确保您对[Adobe I/O Console](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)和Adobe Experience Manager具有管理员权限以设置集成。

#### 使用Adobe控制台{#set-up-adobe-ims-configuration-using-adobe-i-o-console}设置Adobe I/O IMS配置

1. 在&#x200B;**工具>安全性**&#x200B;下创建AdobeIMS技术帐户配置
2. 将&#x200B;*云解决方案*&#x200B;选为&#x200B;*Adobe Stock*&#x200B;并创建新证书或重新使用现有证书进行配置。
3. 导航到Adobe I/O Console，然后为&#x200B;*Adobe Stock*&#x200B;创建新的服务帐户集成。
4. 将证书从步骤2上传到您的Adobe Stock服务帐户集成。
5. 选择所需的Adobe Stock用户档案配置并完成服务集成。
6. 使用集成详细信息完成AdobeIMS技术帐户配置
7. 确保您可以使用Adobe IMS技术帐户接收访问令牌。

![Adobe IMS 技术帐户](assets/screen_shot_2018-10-22at12219pm.png)

#### 设置Adobe StockCloud Services{#set-up-adobe-stock-cloud-services}

1. 在&#x200B;**“工具”>“Cloud Services”下为Adobe Stock新建云服务配置。**
2. 选择在上节中为您的&#x200B;*Adobe Stock Cloud*&#x200B;配置创建的&#x200B;*AdobeIMS配置*

3. 确保选择&#x200B;**环境**&#x200B;作为PROD。 暂存环境不受支持，将在AEM的下一版本中删除。
4. **授权资** 产路径可指向/content/dam下的任何目录。在下一版AEM中将添加对此字段的功能支持
5. 选择您的区域设置并完成设置。
6. 您还可以将用户/用户组添加到Adobe Stock Cloud服务，以启用特定用户或组的访问权限。

![Adobe资产Stock配置](assets/screen_shot_2018-10-22at12425pm.png)

### 其他资源

* [企业库存计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2发行说明](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [集成AEM和Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [Adobe I/O Console Integration API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API文档](https://www.adobe.io/apis/creativecloud/stock/docs.html)