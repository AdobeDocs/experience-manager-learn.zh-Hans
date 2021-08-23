---
title: 使用Brand Portal
description: AEM创作和AEM Assets Brand Portal集成的视频演练。
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: 内容管理
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1767'
ht-degree: 2%

---


# 将Brand Portal与AEM Assets结合使用{#using-brand-portal-with-aem-assets}

有关Adobe Experience Manager(AEM)Assets Brand Portal集成的视频指南。

## Brand Portal 2019年9月版功能和增强功能

Brand Portal于2019年9月推出了资产源，其中最引人注目的一项功能是引入了资产源，该功能可提高内容速度，并允许Experience Manager作者与第三方创意人员和参与者之间轻松快速地交换资产。

### Brand Portal资产源{#asset-sourcing}

Brand Portal的资产源用于从第三方代理和团队收集资产，并将它们无缝地同步回Experience Manager作者，以供审阅和使用。

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager作者6.5 SP2(6.5.2)或更高版本才能使用资产源*

请查看[为Experience Manager源启用Experience Manager创作](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html) ，以获取有关如何在资产创作中配置和设置资产源的说明。

## Brand Portal 2019年2月版功能和增强功能{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portal 2019年2月版重点关注对文本搜索和主要客户请求的增强。

### 搜索增强功能

Brand Portal通过在过滤窗格中对属性谓词进行部分文本搜索来增强搜索。 要允许部分文本搜索，您需要在搜索表单中启用属性谓词中的部分搜索。

请阅读以了解有关部分文本搜索和通配符搜索的更多信息。

#### 部分短语搜索

现在，您可以通过在筛选窗格中仅指定搜索短语的一个部分（即一两个单词）来搜索资产。

**用例** :当您不确定搜索的短语中出现的词语的确切组合时，部分短语搜索会很有帮助。

例如，如果您在Brand Portal中的搜索表单使用“属性谓词”对资产标题进行部分搜索，则指定术语camp将返回所有资产，其标题短语中带有单词camp 。

#### 通配符搜索

Brand Portal允许在搜索查询中使用星号(*)以及搜索短语中单词的一部分。

**用例** ：如果您不确定搜索的短语中出现的确切词语，可以使用通配符搜索来填补搜索查询中的空白。

例如，如果Brand Portal中的搜索表单使用属性谓词对资产标题进行部分搜索，则指定climb*会返回所有资产，其标题短语中的单词以字符climb开头。

同样，指定：

* \*climb会返回标题短语中单词以字符结尾的所有资产。
* \*climb\*会返回所有包含字符所包含词语的资产，其标题短语即会包含字符所爬升的词语。

#### 启用文件夹层次结构

管理员现在可以配置在登录时向非管理员用户（编辑者、查看者和来宾用户）显示文件夹的方式。
[“启用文件夹](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 层次配置”将添加在“常规设置”中的“管理工具”面板中。如果配置为：

* 启用后，从根文件夹开始的文件夹树对非管理员用户可见。 因此，应向他们授予与管理员类似的导航体验。
* 禁用后，登陆页面上只显示共享文件夹。

[启用文件](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 夹分层功能（启用后）有助于区分从不同层级共享的具有相同名称的文件夹。现在，非管理员用户在登录时会看到共享文件夹的虚拟父（和上级）文件夹。

共享文件夹在虚拟文件夹的相应目录内组织。 您可以使用锁定图标识别这些虚拟文件夹。

请注意，虚拟文件夹的默认缩略图是第一个共享文件夹的缩略图。

### Dynamic Media视频演绎版支持

除了原始视频文件之外，AEM创作实例处于Dynamic Media混合模式的用户还可以预览和下载Dynamic Media演绎版。

要允许在特定租户帐户上预览和下载Dynamic Media演绎版，管理员需要在“管理工具”面板的视频配置中指定Dynamic Media配置(视频服务URL(DM-Gateway URL)和注册ID以获取动态视频)。

Dynamic Media视频可在以下位置预览：

* 资产详细信息页面
* 资产的卡片视图
* 链接共享预览页面

Dynamic Media视频编码可从以下位置下载：

* Brand Portal
* 共享链接

### 计划发布到Brand Portal

可以安排在稍后的日期和时间将资产（和文件夹）从[AEM(6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)创作实例发布到Brand Portal的工作流。

同样，可以通过计划从Brand Portal取消发布工作流，在以后的日期（时间）从门户中删除已发布的资产。

### URL中的可配置租户别名

组织可以通过在URL中使用替代前缀来自定义其门户URL。 要在其现有门户URL中获取租户名称的别名，组织需要联系Adobe支持。

请注意，只能自定义Brand Portal URL的前缀，而不能自定义整个URL。
例如，现有域为`wknd.brand-portal.adobe.com`的组织可以根据请求创建`wkndinc.brand-portal.adobe.com`。

但是，AEM创作实例只能[配置](https://helpx.adobe.com/cn/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) ，且只能使用租户ID URL，而不能使用租户别名（替代）URL。

**用例** :组织可以通过自定义门户URL来满足其品牌需求，而不是坚持使用由Adobe提供的URL。

## Brand Portal 2018年12月版功能和增强功能{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### 来宾访问

AEM Brand Portal允许来宾访问该门户。 来宾用户无需凭据即可进入门户，并且可以访问和下载所有公共文件夹和收藏集。 来宾用户可以将资产添加到其简易箱（专用收藏集）并下载该资产。 他们还可以查看由管理员设置的智能标记搜索和搜索谓词。 来宾会话不允许用户创建收藏集和保存的搜索，或进一步共享它们、访问文件夹和收藏集设置，以及将资产共享为链接。

### 加速下载

Brand Portal用户可以利用基于Aspera的快速下载，将下载速度提高25倍，并且无论其位于何处，都可以享受无缝的下载体验。 要更快地从Brand Portal或共享链接下载资产，用户需要在其组织中启用下载加速时，在下载对话框中选择启用下载加速选项。

* [加速从Brand Portal下载的指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera连接测试服务器](https://test-connect.asperasoft.com/)

### 用户登录报表

引入了新报表来跟踪用户登录情况。 “用户登录”报表有助于组织审核和检查委派的管理员和Brand Portal的其他用户。

报表记录每个用户的显示名称、电子邮件ID、角色（管理员、查看者、编辑者、来宾）、群组、上次登录、活动状态和登录计数。

### 访问原始演绎版

管理员可以限制用户访问原始图像文件(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-portable-pixmap、x-rgb、x-xbitmap、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、psd、image/vnd.adobe.photoshop)，并允许访问从Brand Portal或共享链接下载的低分辨率演绎版。 此访问权限可以在用户组级别的“管理工具”面板中“用户角色”页面的“组”选项卡中进行控制。

### 新配置

为管理员添加了六项新配置，以针对特定租户启用/禁用以下功能：

* 允许来宾访问
* 允许用户请求访问Brand Portal
* 允许管理员从Brand Portal中删除资产
* 允许创建公共集合
* 允许创建公共智能收藏集
* 允许下载加速

### 其他增强功能

* *卡片视图和列表视图上的文件夹层次结构路径*  — 使用户能够知道存储在Brand Portal实例中的文件夹的位置。帮助用户区分不同文件夹层次结构中具有相同名称的文件夹。
* *概述选项*  — 通过选择资产/文件夹，然后从工具栏中选择概述选项，提供有关资产/文件夹的非管理员用户元数据。当前，显示标题、创建日期和路径

### Adobe I/O主机UI以配置oAuth集成

Brand Portal使用Adobe I/O[https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)界面创建JWT应用程序，该应用程序支持配置oAuth集成以允许AEM Assets与Brand Portal集成。 以前，用于配置OAuth集成的UI托管在`https://marketing.adobe.com/developer/`中。 有关将AEM Assets与Brand Portal集成以将资产和收藏集发布到Brand Portal的更多信息，请参阅[配置AEM Assets与Brand Portal的集成](https://helpx.adobe.com/cn/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)。

## Brand Portal 2018年2月版功能和增强功能{#brand-portal-features-and-enhancements-632}

新增功能增强了旨在将Brand Portal与AEM相协调的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### 导航改进

* 升级了与AEM一致且使用Coral3 UI的用户界面。
* 通过新的Adobe徽标快速、轻松地访问管理工具。
* 通过叠加浏览产品
* 从子文件夹快速导航到父文件夹。
* 用于导航到管理工具和内容的Omnisearch选项。
* 卡片视图、列表视图和列视图可帮助您轻松浏览嵌套文件夹。
* 列表视图中的资产排序不再受屏幕上显示的资产数量的限制。 文件夹中的所有资产都会进行排序。

### 搜索改进

* Omnisearch功能允许您在Brand Portal中快速搜索资产和文件。
* Omnisearch还提供了一个选项，用于搜索特定文件夹或位置内的资产
* 自动关键词建议以简化搜索
* 使用其他过滤器改进您的Omnisearch。 用于将搜索结果保存到智能收藏集的选项，以便您稍后再次访问搜索。
* 支持智能标记资产搜索
* AEM智能标记资产可从AEM共享到Brand Portal，并在Brand Portal中使用智能标记进行资产搜索。

### 文件共享改进

* 用户可以使用链接共享选项共享资产。
* 共享资产时，用户将为每个资产设置一个到期日期。 为用户提供对共享资产的更多控制。
* 具有资产共享链接的外部用户可以下载图像并查看其属性。
* 对于下载的资产文件夹，将保留原始的嵌套文件夹层次结构。

### 报告和管理功能

* 现在，可以将AEM Assets中的元数据架构从AEM发布到Brand Portal。
* 管理员可以创建和管理三种类型的报表：已下载、已过期和已发布的资产
* 能够配置需要包含在报表中的列。
* 在Brand Portal中为资产创建图像预设。
* 能够修改“管理员搜索边栏表单”或“搜索Forms”以包含其他过滤选项。
* 更新和预览品牌的自定义墙纸
* “使用情况”报表，用于了解有关用户数量、已用存储空间和总资产的信息。

## 其他资源{#additional-resources}

* [Brand Portal的新增功能](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM创作复制代理](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [加速下载指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand PortalAdobe文档](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic MediaAdobe文档](https://experienceleague.adobe.com/docs/)
* [下载Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera连接测试服务器](https://test-connect.asperasoft.com/)