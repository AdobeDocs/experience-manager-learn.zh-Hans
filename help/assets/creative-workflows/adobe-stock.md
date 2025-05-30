---
title: 将Adobe Stock资源与AEM Assets结合使用
description: AEM为用户提供了直接从AEM搜索、预览、保存和许可Adobe Stock资源的功能。 现在，组织可以将Adobe Stock企业计划与AEM Assets集成，以确保通过AEM的强大资源管理功能，许可资产现在可广泛用于其创意和营销项目。
feature: Adobe Stock
version: Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1079
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 1%

---

# 将Adobe Stock与AEM Assets一起使用{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2为用户提供了直接从AEM搜索、预览、保存和许可Adobe Stock资源的功能。 现在，组织可以将Adobe Stock企业计划与AEM Assets集成，以确保通过AEM的强大资源管理功能，许可资产现在可广泛用于其创意和营销项目。

>[!VIDEO](https://video.tv.adobe.com/v/40219?quality=12&learn=on&captions=chi_hans)

>[!NOTE]
>
>该集成需要至少部署Service Pack 2的[企业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)和AEM 6.4。 有关AEM 6.4 Service Pack的详细信息，请参阅以下[发行说明](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html)。

通过Adobe Stock和AEM Assets集成，内容创作者和营销人员可轻松授权并使用股票资产进行创意或营销。 您可以使用Omni Search、将位置筛选器添加为Adobe Stock或者在AEM Assets主导航中导航并单击搜索Adobe Stock Coral UI图标来执行Stock资产搜索。

## 功能

### 搜索并保存

* 在不离开Adobe Stock工作区的情况下执行AEM资源搜索。
* 保存Adobe Stock资源以供预览（不许可该资源）。
* 能够许可并将Adobe Stock资源保存到AEM Assets
* 能够在AEM Assets UI中从Adobe Stock搜索类似的资源
* 在Adobe Stock网站的AEM Assets的Stock Search中查看选定的资产
* 许可的资源文件标有蓝色许可徽章，以便于识别

### 资产元数据

* 许可的资源存储在AEM Assets中。 资产属性在单独的资产元数据选项卡下包含股票元数据
* 能够将许可证引用添加到资产元数据

### 资产库存配置文件

* 用户可以在&#x200B;*用户>我的偏好设置> Stock配置*&#x200B;下选择Adobe Stock配置文件
* 可以将必选和可选参考添加到资产许可窗口。
* 能够根据地区为“资产许可”窗口选择语言首选项。

### 过滤器

* 用户可以根据资产类型、方向和查看类似内容来筛选股票资产
* 资产类型包括照片、插图、矢量、视频、模板、3D、Premium、编辑
* “方向”包括“水平”、“垂直”和“方形”。
* 查看类似过滤器需要Adobe Stock文件编号

### 访问控制

* 管理员可以在设置Adobe Stock云服务配置时，向某些用户/组提供许可Stock资源的权限。
* 如果特定用户/组无权许可Stock资产，则将禁用&#x200B;*Stock资产搜索/资产许可*&#x200B;功能。

## 使用AEM Assets设置Adobe Stock{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2为用户提供了直接从AEM搜索、预览、保存和许可Adobe Stock资源的功能。 本视频涵盖有关如何使用Adobe Console在AEM Assets中设置Adobe I/O Stocks的快速演练。

>[!VIDEO](https://video.tv.adobe.com/v/328767?quality=12&learn=on&captions=chi_hans)

>[!NOTE]
>
>对于Adobe Stock Cloud Service配置，您必须选择生产环境以及指向`/content/dam`的已许可资源路径。 AEM中的环境字段现已删除。

>[!NOTE]
>
>该集成需要部署了至少[Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24)的[企业Adobe Stock计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)和AEM 6.4。 有关AEM 6.4 Service Pack的详细信息，请参阅以下[发行说明](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html)。 您还需要[Adobe I/O Console](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)和Adobe Experience Manager的管理权限才能设置集成。

### 安装 {#installations}

* 对于AEM 6.4，您需要安装[AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=。%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24)，然后重新安装cq-dam-stock-integration-content-1.0.4.zip文件。
* 确保您拥有[Adobe I/O Console](https://console.adobe.io/)、[Adobe Admin Console](https://adminconsole.adobe.com/)和Adobe Experience Manager上的管理员权限以设置集成。

#### 使用Adobe I/O控制台设置Adobe IMS配置 {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. 在&#x200B;**工具>安全**&#x200B;下创建Adobe IMS技术帐户配置
2. 选择&#x200B;*云解决方案*&#x200B;作为&#x200B;*Adobe Stock*，然后创建新证书或重复使用现有证书进行配置。
3. 导航到Adobe I/O控制台并为&#x200B;*Adobe Stock*&#x200B;创建新的服务帐户集成。
4. 将证书从Step2上传到Adobe Stock服务帐户集成。
5. 选择所需的Adobe Stock配置文件配置并完成服务集成。
6. 使用集成详细信息完成Adobe IMS技术帐户配置
7. 确保您可以使用Adobe IMS技术帐户接收访问令牌。

![Adobe IMS技术帐户](assets/screen_shot_2018-10-22at12219pm.png)

#### 设置Adobe Stock云服务 {#set-up-adobe-stock-cloud-services}

1. 在&#x200B;**Tools > CLoud Services.**&#x200B;下，为Adobe Stock创建新的云服务配置。
2. 选择在上面部分中为您的&#x200B;*Adobe Stock Cloud*&#x200B;配置创建的&#x200B;*Adobe IMS配置*

3. 确保选择&#x200B;**环境**&#x200B;作为PROD。
4. **许可的资产路径**&#x200B;可以指向`/content/dam`下的任何目录。
5. 选择您的区域设置并完成设置。
6. 您还可以将用户/组添加到Adobe Stock云服务，以启用特定用户或组的访问权限。

![Adobe Assets Stock配置](assets/screen_shot_2018-10-22at12425pm.png)

### 其他资源

* [企业库存计划](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2发行说明](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=zh-Hans)
* [集成AEM和Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=zh-Hans)
* [Adobe I/O控制台集成API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API文档](https://www.adobe.io/apis/creativecloud/stock/docs.html)
