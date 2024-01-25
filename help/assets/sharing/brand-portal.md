---
title: 使用Brand Portal
description: AEM创作与AEM Assets Brand Portal集成的视频演练。
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2494
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 0%

---

# 将Brand Portal与AEM Assets一起使用{#using-brand-portal-with-aem-assets}

Adobe Experience Manager (AEM) Assets Brand Portal集成的视频指南。

## Brand Portal 2019年9月版功能和增强功能

Brand Portal在2019年9月推出的最显着功能是引入资源采购，这可提升内容速度，并允许Experience Manager作者与第三方创意人员和投稿人之间轻松快速地交换资源。

### Brand Portal资源源{#asset-sourcing}

Brand Portal的资源源用于从第三方机构和团队收集资源，并将其无缝同步回Experience Manager创作以供审阅和使用。

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*要使用Experience Manager源，需要安装资源创作6.5 SP2 (6.5.2)或更高版本*

审核 [为资源源启用Experience Manager创作](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=zh-Hans) 有关如何在Experience Manager创作中配置和设置资源源的说明。

## Brand Portal 2019年2月版功能和增强功能{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

Brand Portal 2019年2月版重点对文本搜索和主要客户请求进行了增强。

### 搜索增强功能

Brand Portal通过筛选窗格中的属性谓词的部分文本搜索来增强搜索。 要允许部分文本搜索，您需要在搜索表单的属性谓词中启用部分搜索。

请阅读下文，了解关于部分文本搜索和通配符搜索的更多信息。

#### 部分短语搜索

现在，您可以在筛选窗格中仅指定搜索短语的一部分（即一两个字）来搜索资源。

**用例** ：当您不确定搜索到的短语中出现的确切单词组合时，部分短语搜索会很有帮助。

例如，如果您在Brand Portal中的搜索表单使用属性谓词对资源标题进行部分搜索，则指定术语camp将返回所有在其标题短语中带有camp一词的资源。

#### 通配符搜索

Brand Portal允许在搜索查询中使用星号(*)以及搜索短语中的单词的一部分。

**用例** ：如果您不确定搜索短语中出现的确切单词，可以使用通配符搜索来填补搜索查询中的空白。

例如，如果Brand Portal中的搜索表单使用属性谓词对资源标题进行部分搜索，则指定climb*会返回其标题短语中含有以字符climb开头的单词的所有资源。

同样，指定：

* \*climb返回所有资产，其中标题短语以字符climb结尾。
* \*climb\*返回所有包含单词的资产，这些单词包含在其标题短语中climb的字符。

#### 启用文件夹层次结构

管理员现在可以配置在登录时如何向非管理员用户（编辑者、查看者和访客用户）显示文件夹。
[启用文件夹层次结构](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 配置会添加到管理工具面板的“常规设置”中。 如果配置为：

* 启用后，非管理员用户将能够看到从根文件夹开始的文件夹树。 因此，可以授予他们类似于管理员的导航体验。
* 已禁用，登录页上仅显示共享文件夹。

[启用文件夹层次结构](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 功能（启用时）可帮助您区分名称相同的文件夹与不同的层次结构共享。 登录时，非管理员用户现在可以看到共享文件夹的虚拟父文件夹（和祖先文件夹）。

共享文件夹在虚拟文件夹中的相应目录中组织。 您可以使用锁图标来识别这些虚拟文件夹。

请注意，虚拟文件夹的默认缩略图是第一个共享文件夹的缩略图图像。

### Dynamic Media视频呈现版本支持

AEM创作实例处于Dynamic Media混合模式的用户除了可以预览和下载原始视频文件之外，还可以预览和下载Dynamic Media演绎版。

要允许预览和下载特定租户帐户上的Dynamic Media演绎版，管理员需要在“管理工具”面板的“视频配置”中指定Dynamic Media配置(视频服务URL （DM网关URL）和注册ID以获取动态视频)。

可以在以下位置预览Dynamic Media视频：

* 资源详细信息页面
* 资产的卡片视图
* 链接共享预览页面

Dynamic Media视频编码可从以下位置下载：

* Brand Portal
* 共享链接

### 已计划发布到Brand Portal

资源（和文件夹）发布工作流程 [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) Brand Portal的创作实例可以安排在稍后的日期和时间执行。

同样，在稍后的日期（时间），可以通过计划从Brand Portal取消发布工作流程，从门户中删除已发布的资源。

### URL中可配置的租户别名

通过在URL中使用备用前缀，组织可以自定义其门户URL。 要在现有门户URL中获取租户名称的别名，组织需要联系Adobe支持部门。

请注意，只能自定义Brand Portal URL的前缀，而不能自定义整个URL。
例如，具有现有域的组织 `wknd.brand-portal.adobe.com` 可以获取 `wkndinc.brand-portal.adobe.com` 已应请求创建。

但是，AEM创作实例可以是 [已配置](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) 仅使用租户ID URL，不使用租户别名（替代）URL。

**用例** ：组织可以通过自定义门户URL来满足其品牌需求，而不是停留在Adobe提供的URL上。

## Brand Portal 2018年12月功能和增强功能{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### 访客访问

AEM Brand Portal允许来宾访问该门户。 访客用户无需凭据即可进入门户，并且可以访问和下载所有公共文件夹和收藏集。 访客用户可以将资产添加到其Light-Box（专用收藏集）并下载它们。 他们还可以查看智能标记搜索和管理员设置的搜索谓词。 来宾会话不允许用户创建收藏集和保存的搜索或进一步共享它们，不允许访问文件夹和收藏集设置，不允许将资产作为链接共享。

### 加速下载

Brand Portal用户可以利用基于Aspera的快速下载，获得高达25倍的速度并享受无缝下载体验，无论他们位于全球何处。 要更快地从Brand Portal或共享链接下载资源，用户需要在下载对话框中选择启用下载加速选项，前提是他们的组织已启用下载加速。

* [加快从Brand Portal下载的指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera连接测试服务器](https://test-connect.asperasoft.com/)

### 用户登录报表

引入了一个用于跟踪用户登录情况的新报表。 “用户登录”报表可以帮助组织审核并检查Brand Portal的委派管理员和其他用户。

报表日志显示每个用户的名称、电子邮件ID、角色（管理员、查看者、编辑者、来宾）、组、上次登录、活动状态和登录计数。

### 访问原始演绎版

管理员可以限制用户对原始图像文件(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-pixmap、x-rgb、x-xbitmap、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、psd、image/vnd.adobe.photoshop)的访问，并授予对从Brand Portal或共享链接下载的低分辨率演绎版的访问权限。 可以从管理工具面板中“用户角色”页面的“组”选项卡在用户组级别控制此访问权限。

### 新配置

为管理员添加了六个新配置，以启用/禁用特定租户的以下功能：

* 允许来宾访问
* 允许用户请求访问Brand Portal
* 允许管理员从Brand Portal中删除资源
* 允许创建公共收藏集
* 允许创建公共智能收藏集
* 允许下载加速

### 其他增强功能

* *卡片视图和列表视图上的文件夹层次结构路径*  — 使用户能够知道存储在Brand Portal实例中的文件夹的位置。 帮助用户区分不同文件夹层次结构中同名的文件夹。
* *“概述”选项*  — 通过选择资源/文件夹，然后从工具栏中选择概述选项，提供有关资源/文件夹的非管理员用户元数据。 当前，显示标题、创建日期和路径

### Adobe I/O承载用于配置oAuth集成的UI

Brand Portal使用Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) 界面创建JWT应用程序，该应用程序允许配置oAuth集成以允许AEM Assets与Brand Portal集成。 以前，用于配置OAuth集成的UI托管在 `https://marketing.adobe.com/developer/`. 要了解有关将AEM Assets与Brand Portal集成以将资产和收藏集发布到Brand Portal的更多信息，请参阅 [配置AEM Assets与Brand Portal的集成](https://helpx.adobe.com/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Brand Portal 2018年2月功能和增强功能{#brand-portal-features-and-enhancements-632}

新增功能增强了旨在将Brand Portal与AEM保持一致的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### 导航改进

* 升级后的用户界面，与AEM保持一致，并使用Coral3 UI。
* 通过新的Adobe徽标快速轻松地访问管理工具。
* 通过叠加进行产品导航
* 从子文件夹快速导航到父文件夹。
* 用于导航到管理工具和内容的Omnisearch选项。
* 卡片视图、列表视图和列视图帮助您轻松浏览嵌套文件夹。
* 列表视图中的资源排序不再限制在屏幕上显示的资源数。 对文件夹中的所有资源进行排序。

### 搜索改进

* 通过Omnisearch功能，您可以在Brand Portal中快速搜索资源和文件。
* Omnisearch还提供了在特定文件夹或位置中搜索资源的选项
* 自动提供关键词建议，使搜索更轻松
* 通过额外的过滤器改进您的Omnisearch。 用于将搜索结果保存到Smart Collection中的选项，以供您稍后重新访问搜索。
* 支持智能标记的资源搜索
* AEM智能标记资源可以从AEM共享到Brand Portal，并在Brand Portal中使用智能标记进行资源搜索。

### 文件共享改进

* 用户可以使用链接共享选项共享资产。
* 在共享资产时，用户可设置每个资产的到期日期。 让用户对共享的资源拥有更多控制权。
* 具有资产共享链接的外部用户可以下载图像并查看其属性。
* 对于下载的资源文件夹，将保留原始嵌套文件夹层次结构。

### 报告和管理功能

* AEM Assets中的元数据架构现在可以从AEM发布到Brand Portal。
* 管理员可以创建和管理三种类型的报表 — 已下载、已过期和已发布的资产
* 能够配置需要包含在报告中的列。
* 在Brand Portal中为资源创建图像预设。
* 能够修改管理员搜索边栏表单或搜索Forms以包含其他筛选选项。
* 更新和预览品牌的自定壁纸
* 使用情况报表，用于了解用户数量、已用存储空间和资产总数。

## 其他资源{#additional-resources}

* [Brand Portal的新增功能](https://helpx.adobe.com/cn/experience-manager/brand-portal/using/whats-new.html)
* [AEM创作复制代理](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [加速下载指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand PortalAdobe文档](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic MediaAdobe文档](https://experienceleague.adobe.com/docs/)
* [下载Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera连接测试服务器](https://test-connect.asperasoft.com/)
