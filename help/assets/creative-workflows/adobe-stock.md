---
title: 将Adobe Stock资产与AEM Assets结合使用
description: AEM让用户能够直接从AEM搜索、预览、保存和许可Adobe Stock资产。 现在，组织可以将其Adobe Stock企业计划与AEM Assets相集成，以确保许可资产现在可广泛用于其创意和营销项目，并具有AEM的强大资产管理功能。
feature: Adobe Stock
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 3%

---

# 将Adobe Stock与AEM Assets结合使用{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2让用户能够直接从AEM搜索、预览、保存和许可Adobe Stock资产。 现在，组织可以将其Adobe Stock企业计划与AEM Assets相集成，以确保许可资产现在可广泛用于其创意和营销项目，并具有AEM的强大资产管理功能。

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=12&learn=on)

>[!NOTE]
>
>该集成需要 [企业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4，并且至少部署了Service Pack 2。 有关AEM 6.4 Service Pack详细信息，请参阅以下内容 [发行说明](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html).

Adobe Stock和AEM Assets集成允许内容作者和营销人员轻松地将库存资产许可和用于创意或营销目的。 您可以使用Omni Search、将位置筛选器添加为Adobe Stock，或通过浏览AEM Assets主导航并单击搜索Adobe Stock Coral UI图标来执行Stock资产搜索。

## 功能

### 搜索并保存

* 在不离开Adobe Stock工作区的情况下执行AEM资产搜索。
* 保存Adobe Stock资产以进行预览，而无需授权资产。
* 能够将Adobe Stock资产授权并保存到AEM Assets
* 能够在AEM Assets UI中从Adobe Stock搜索类似资产
* 在Adobe Stock网站的AEM Assets中，通过“股票搜索”查看选定的资产
* 授权资产文件使用蓝色的授权徽章进行标记，以便于识别

### 资产元数据

* 授权资产存储在AEM Assets中。 资产属性在单独的资产元数据选项卡下包含Stock元数据
* 能够向资产元数据添加许可证引用

### 资产库配置文件

* 用户可以在下面选择Adobe Stock配置文件 *用户>我的首选项>库存配置*
* 可以将必选和可选引用添加到“资产许可”窗口。
* 能够根据区域为资产授权窗口选择语言首选项。

### 过滤器

* 用户可以根据资产类型、方向和查看相似情况筛选库存资产
* 资产类型包括照片、插图、矢量、视频、模板、3D、Premium、编辑
* 方向包括“水平”、“垂直”和“正方形”。
* 查看相似过滤器需要Adobe Stock文件号

### 访问控制

* 管理员在设置Adobe Stock云服务配置时，可以向特定用户/组提供授权库存资产的权限。
* 如果特定用户/组没有许可库存资产的权限， *Stock资产搜索/资产许可* 功能将被禁用。

## 使用AEM Assets设置Adobe Stock{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2让用户能够直接从AEM搜索、预览、保存和许可Adobe Stock资产。 此视频介绍如何使用“Adobe控制台”在AEM Assets中设置Adobe I/O库。

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>对于Adobe Stock云服务配置，您必须选择生产环境和许可资产路径，以指向 `/content/dam`. 现在，环境字段已在AEM中删除。

>[!NOTE]
>
>该集成需要 [企业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) 和AEM 6.4(至少 [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=。%2Fjcr%3Content%2Fmetadata%2Fdc%3Rovase&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fdc%3Ati&amp;orderby.st=st&amp;p.offset=0&amp;p.limit=24) 已部署。 有关AEM 6.4 Service Pack详细信息，请参阅以下内容 [发行说明](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). 您还需要拥有 [Adobe I/O控制台](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager来设置集成。

### 安装 {#installations}

* 对于AEM 6.4，您需要安装 [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=。%2Fjcr%3Content%2Fmetadata%2Fdc%3Rovase&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aem%2F6-4&amp;3_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fdc%3Ati&amp;orderby.st=st&amp;p.offset=0&amp;p.limit=24) 然后重新安装cq-dam-stock-integration-content-1.0.4.zip文件。
* 确保您具有 [Adobe I/O控制台](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) 和Adobe Experience Manager来设置集成。

#### 使用Adobe I/O控制台设置Adobe IMS配置 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. 在 **工具>安全**
2. 选择 *云解决方案* as *Adobe Stock* 并创建新证书或重复使用现有证书进行配置。
3. 导航到Adobe I/O控制台并为创建新的服务帐户集成 *Adobe Stock*.
4. 将证书从步骤2上传到您的Adobe Stock服务帐户集成。
5. 选择所需的Adobe Stock配置文件配置并完成服务集成。
6. 使用集成详细信息完成Adobe IMS技术帐户配置
7. 确保您可以使用Adobe IMS技术帐户接收访问令牌。

![Adobe IMS 技术帐户](assets/screen_shot_2018-10-22at12219pm.png)

#### 设置Adobe StockCloud Services {#set-up-adobe-stock-cloud-services}

1. 在下面为Adobe Stock创建新的云服务配置 **工具>Cloud Services。**
2. 选择 *Adobe IMS配置* 在上面部分中为 *Adobe Stock Cloud* 配置

3. 确保选择 **环境** 作为PROD。
4. **授权资产路径** 可指向下的任何目录 `/content/dam`.
5. 选择您的区域设置并完成设置。
6. 您还可以将用户/组添加到Adobe Stock云服务，以启用特定用户或组的访问权限。

![Adobe资产库配置](assets/screen_shot_2018-10-22at12425pm.png)

### 其他资源

* [企业股票计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2发行说明](https://experienceleague.adobe.com/docs/experience-manager-64/release-notes/sp-release-notes.html?lang=zh-Hans)
* [集成AEM和Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/O控制台集成API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API文档](https://www.adobe.io/apis/creativecloud/stock/docs.html)
