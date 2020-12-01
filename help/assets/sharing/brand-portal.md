---
title: 使用Brand Portal
seo-title: 将Brand Portal与AEM Assets一起使用
description: AEM作者与AEM Assets品牌门户集成的视频演练。
seo-description: AEM作者与AEM Assets品牌门户集成的视频演练。
feature: brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions, administration
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
team: tm
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1788'
ht-degree: 1%

---


# 将Brand Portal与AEM Assets一起使用{#using-brand-portal-with-aem-assets}

Adobe Experience Manager(AEM)资产品牌门户集成的视频指南。

## Brand Portal 2019年9月功能和增强功能

Brand Portal于2019年9月推出了最引人注目的资产采购，它提高了内容速度，并允许Experience Manager作者与第三方创意人员和投稿人轻松、快速地交换资产。

### 品牌门户资产来源补充{#asset-sourcing}

Brand Portal的资产来源收集功能用于从第三方代理和团队收集资产，将资产无缝同步回Experience Manager作者，供其审阅和使用。

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager作者6.5 SP2(6.5.2)或更高版本才能使用资产来源补充*

有关如何配置和设置Experience Manager作者资产来源补充的说明，请查阅[为资产来源补充启用Experience Manager作者](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html)。

## Brand Portal 2019年2月功能和增强{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portal 2019年2月发布的版本侧重于文本搜索和热门客户请求的增强。

### 搜索增强功能

Brand Portal通过筛选窗格中属性谓词的部分文本搜索增强了搜索。 要允许部分文本搜索，您需要在搜索表单中启用属性谓词中的部分搜索。

阅读以进一步了解部分文本搜索和通配符搜索。

#### 部分短语搜索

您现在可以通过在筛选窗格中仅指定搜索短语的一个或两个部分来搜索资产。

**用例** :当您不确定搜索的短语中出现的词的确切组合时，部分短语搜索会很有帮助。

例如，如果您在Brand Portal中的搜索表单使用属性谓词对资产标题进行部分搜索，则指定术语营地将返回标题短语中带有单词营地的所有资产。

#### 通配符搜索

品牌门户允许在搜索查询中使用星号(*)，并在搜索的短语中使用部分单词。

**用例** ：如果您不确定搜索的短语中出现的确切词语，可使用通配符搜索来填补搜索查询中的间隙。

例如，指定clim*会返回所有资产，如果品牌门户中的搜索表单使用属性谓词对资产标题进行部分搜索，则其标题短语中包含以字符clamp开头的词的所有资产。

同样，指定：

* \*climb返回标题短语中带有以字符结尾的所有资产。
* \*clibm\*返回所有包含标题短语中字符爬升的单词的资产。

#### 启用文件夹层次结构

管理员现在可以配置文件夹在登录时向非管理员用户（编辑者、查看者和来宾用户）显示的方式。
[“启用文](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 件夹层次配置”将添加到“常规设置”中的“管理工具”面板中。如果配置为：

* 启用后，非管理员用户将看到从根文件夹开始的文件夹树。 因此，为他们提供类似于管理员的导航体验。
* 禁用后，登陆页上只显示共享文件夹。

[启用文](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 件夹分层功能（启用时）有助于区分从不同层次共享的名称相同的文件夹。登录时，非管理员用户现在可以看到共享文件夹的虚拟父（和上级）文件夹。

共享文件夹在虚拟文件夹的各个目录中进行组织。 您可以通过锁定图标识别这些虚拟文件夹。

请注意，虚拟文件夹的默认缩略图是第一个共享文件夹的缩略图。

### 动态媒体视频演绎版支持

AEM作者实例处于Dynamic Media混合模式的用户除了可以下载原始视频文件外，还可以预览和下载Dynamic Media演绎版。

要允许在特定租户帐户上预览和下载动态媒体演绎版，管理员需要从管理工具面板的视频配置中指定Dynamic Media Configuration(视频服务URL(DM-Gateway URL)和注册ID以获取动态视频)。

Dynamic Media视频可在以下位置预览：

* 资产详细信息页面
* 资产卡视图
* 链接共享预览页

Dynamic Media视频编码可从以下位置下载：

* Brand Portal
* 共享链接

### 计划发布到品牌门户

资产（和文件夹）发布工作流从[AEM(6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)创作实例发布到Brand Portal，可以计划在以后的日期、时间内进行。

同样，可以通过计划从Brand Portal中取消发布工作流，在以后的日期（时间）从门户中删除已发布的资产。

### URL中的可配置租户别名

组织可以通过在URL中具有替代前缀来自定义其门户URL。 要在其现有门户URL中获取租户名称的别名，组织需要联系Adobe支持。

请注意，只能自定义品牌门户URL的前缀，而不能自定义整个URL。
例如，现有域`wknd.brand-portal.adobe.com`的组织可以在请求时创建`wkndinc.brand-portal.adobe.com`。

但是，AEM作者实例只能[配置](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)（仅使用租户ID URL），而不能使用租户别名（备用）URL。

**用例** :组织可以通过自定义门户URL而不是坚持Adobe提供的URL来满足其品牌需求。

## Brand Portal 2018年12月功能和增强{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### 客人访问

AEM Brand Portal允许客人访问门户。 客人用户不需要凭据即可进入门户，并且可以访问和下载所有公共文件夹和集合。 客人用户可以将资源添加到其灯箱（专用集合）并下载相同的资源。 他们还可以视图管理员设置的智能标记搜索和搜索谓词。 客人会话不允许用户创建收藏集和保存的搜索或进一步共享它们、访问文件夹和收藏集设置以及将资产共享为链接。

### 加速下载

Brand Portal用户可以利用基于Aspera的快速下载，将下载速度提高25倍，无论他们身处全球，都能享受无缝的下载体验。 要更快地从Brand Portal或共享链接下载资产，用户需要在下载对话框中选择“启用下载加速”选项，前提是其组织已启用下载加速。

* [加速从Brand Portal下载的指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### 用户登录报告

已引入跟踪用户登录的新报告。 “用户登录”报告有助于组织审核和检查品牌门户的授权管理员和其他用户。

报告记录每个用户的显示姓名、电子邮件ID、角色（管理员、查看器、编辑者、客人）、用户组、上次登录、活动状态和登录计数。

### 访问原始演绎版

管理员可以限制用户访问原始图像文件(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-rgb、x-xbitmap、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、psd.image/vnd.adobe.photoshop))，并允许用户访问低分辨率再现门户或共享链接。 此访问权限可以在用户组级别控制，该级别位于管理工具面板中“用户角色”页面的“组”选项卡中。

### 新配置

新增了六种配置，供管理员在特定租户上启用／禁用以下功能：

* 允许来宾访问
* 允许用户请求访问Brand Portal
* 允许管理员从Brand Portal中删除资产
* 允许创建公共集合
* 允许创建公共智能集合
* 允许下载加速

### 其他增强功能

* *卡和列表视图上的文件夹层次结构路径* —使用户能够了解存储在Brand Portal实例中的文件夹的位置。帮助用户区分不同文件夹层次结构中具有相同名称的文件夹。
* *概述选项* —通过选择资产／文件夹，然后从工具栏中选择概述选项，提供有关资产／文件夹的非管理员用户元数据。当前，显示标题、创建日期和路径

### Adobe I/O承载UI以配置身份验证集成

Brand Portal使用Adobe I/O[https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)接口创建JWT应用程序，该应用程序支持配置Auth集成以允许AEM Assets与Brand Portal集成。 以前，用于配置OAuth集成的UI托管在`https://marketing.adobe.com/developer/`中。 要进一步了解如何将AEM Assets与品牌门户集成，以将资产和集合发布到品牌门户，请参阅[配置AEM Assets与品牌门户的集成](https://helpx.adobe.com/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)。

## Brand Portal 2018年2月功能和增强{#brand-portal-features-and-enhancements-632}

新增功能增强了面向将Brand Portal与AEM协调的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### 导航改进

* 升级的用户界面，与AEM对齐并使用Coral3 UI。
* 通过新的Adobe徽标快速、轻松地访问管理工具。
* 通过叠加进行产品导航
* 从子文件夹快速导航到父文件夹。
* 用于导航到管理工具和内容的全搜索选项。
* 卡视图、列表视图和列视图可帮助您轻松浏览嵌套文件夹。
* 列表视图中的资产排序不再受屏幕上显示的资产数量的限制。 文件夹中的所有资产都会进行排序。

### 搜索改进

* Omnisearch功能允许您在Brand Portal中快速搜索资产和文件。
* Omnisearch还提供一个选项，用于搜索特定文件夹或位置内的资产
* 自动关键字建议可简化搜索
* 通过其他过滤器改善您的全名搜索。 将搜索结果保存到智能收藏集中，以便您以后重新访问搜索。
* 支持智能标记资产搜索
* AEM可以将标记智能的资产从AEM共享到Brand Portal，并在Brand Portal中使用智能标记进行资产搜索。

### 文件共享改进

* 用户可以使用链接共享选项共享资产。
* 在共享资产时，用户将获取为每个资产设置到期日期。 为用户提供对共享资源的更多控制。
* 具有资产共享链接的外部用户可以下载图像并视图其属性。
* 下载的资产文件夹会保留原始嵌套文件夹层次结构。

### 报告和管理能力

* 现在可以从AEM Assets发布元数据模式到品牌门户。
* 管理员可以创建和管理三种类型的报告——已下载、过期和已发布的资产
* 能够配置需要包含在报告中的列。
* 在Brand Portal中为资产创建图像预设。
* 能够修改管理员搜索边栏表单或搜索Forms以包含其他筛选选项。
* 更新和预览品牌的自定义壁纸
* 使用情况报告，以了解用户数、已使用的存储空间和资产总数。

## 其他资源{#additional-resources}

* [Brand Portal的新增功能](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM作者复制代理](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [加速下载指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets品牌门户Adobe文档](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets动态媒体Adobe文档](https://docs.adobe.com/docs/cn/aem/6-3/author/assets/dynamic-media.html)
* [下载Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)