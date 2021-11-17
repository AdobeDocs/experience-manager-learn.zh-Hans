---
title: 创建网站 | AEM快速网站创建
description: 在“快速创建网站”中，可使用AEM Adobe Experience Manager中的“网站创建向导”来生成新网站。 由Adobe提供的标准网站模板用作新网站的起点。
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 0%

---

# 创建网站 {#create-site}

>[!CAUTION]
>
> 快速网站创建工具当前为技术预览。 它可用于测试和评估目的，并且除非与Adobe支持部门达成协议，否则不会用于生产。

在“快速创建网站”中，可使用AEM Adobe Experience Manager中的“网站创建向导”来生成新网站。 由Adobe提供的标准网站模板用作新网站的起点。

## 前提条件 {#prerequisites}

本章中的步骤将在Adobe Experience Manager as a Cloud Service环境中进行。 确保您拥有AEM环境的管理访问权限。 建议使用 [沙盒项目](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) 和 [开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) 完成本教程时。

查看 [入门文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) 以了解更多详细信息。

## 目标 {#objective}

1. 了解如何使用站点创建向导生成新站点。
1. 了解网站模板的角色。
1. 浏览生成的AEM网站。

## 登录Adobe Experience Manager作者 {#author}

首先，登录到AEMas a Cloud Service环境。 AEM环境在 **创作服务** 和 **发布服务**.

* **创作服务**  — 创建、管理和更新网站内容的位置。 通常，只有内部用户才有权访问 **创作服务** 和位于登录屏幕后面。
* **发布服务**  — 托管实时网站。 这是最终用户将看到并且通常公开提供的服务。

大部分教程将使用 **创作服务**.

1. 导航到Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). 使用您的个人帐户或公司/学校帐户登录。
1. 确保在菜单中选择了正确的组织，然后单击 **Experience Manager**.

   ![Experience Cloud主页](assets/create-site/experience-cloud-home-screen.png)

1. 在 **Cloud Manager** 单击 **Launch**.
1. 将鼠标悬停在要使用的程序上，然后单击 **Cloud Manager计划** 图标。

   ![Cloud Manager项目图标](assets/create-site/cloud-manager-program-icon.png)

1. 在顶部菜单中，单击 **环境** 查看已配置的环境。

1. 查找要使用的环境，然后单击 **创作URL**.

   ![访问开发人员作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建议使用 **开发** 环境。

1. 将向AEM启动新选项卡 **创作服务**. 单击 **使用Adobe登录** 并且您应使用相同的Experience Cloud凭据自动登录。

1. 经过重定向和身份验证后，您现在应会看到AEM开始屏幕。

   ![AEM开始屏幕](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 访问Experience Manager时遇到问题？ 查看 [入门文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下载基本网站模板

网站模板为新网站提供了起点。 网站模板包括一些基本主题、页面模板、配置和示例内容。 网站模板中包含的具体内容取决于开发人员。 Adobe提供 **基本网站模板** 以加快新实施。

1. 打开新的浏览器选项卡，然后导航到GitHub上的“基本站点模板”项目： [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). 该项目是开放源的，并且许可供任何人使用。
1. 单击 **版本** 并导航到 [最新版本](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. 展开 **资产** 下拉菜单并下载模板zip文件：

   ![基本网站模板Zip](assets/create-site/template-basic-zip-file.png)

   此zip文件将用在下一个练习中。

   >[!NOTE]
   >
   > 本教程是使用版本编写的 **1.1.0** “基本网站”模板的“页面查看次数”。 启动新项目以供生产使用时，始终建议使用最新版本。

## 创建新站点

接下来，使用上一个练习中的“网站模板”生成一个新网站。

1. 返回AEM环境。 从AEM开始屏幕中，导航到 **站点**.
1. 在右上角，单击 **创建** > **网站（模板）**. 这将显示 **创建站点向导**.
1. 在 **选择网站模板** 单击 **导入** 按钮。

   上传 **.zip** 模板文件。

1. 选择 **基本AEM网站模板** 单击 **下一个**.

   ![选择网站模板](assets/create-site/select-site-template.png)

1. 在 **网站详细信息** > **网站标题** enter `WKND Site`.

   在真实实施中，“WKND Site”将被公司或组织的品牌名称替换。 在本教程中，我们将模拟创建一个网站，以打造一个虚幻的生活方式品牌“WKND”。

1. 在 **网站名称** enter `wknd`.

   ![网站模板详细信息](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共享AEM环境，请将唯一标识符附加到 **网站名称**. 例如 `wknd-site-johndoe`. 这将确保多个用户可以完成同一教程，而不会出现任何冲突。

1. 单击 **创建** 以生成网站。 单击 **完成** 在 **成功** 对话框。

## 浏览新站点

1. 导航到AEM Sites控制台（如果尚未导航）。
1. 新 **WKND站点** 已生成。 它将包括具有多语言层次结构的站点结构。
1. 打开 **英语** > **主页** 页面，方法是选择页面并单击 **编辑** 按钮：

   ![WKND站点层次结构](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已创建起始内容，可向页面添加多个组件。 试用这些组件，了解功能。 您将在下一章中学习组件的基础知识。

   ![开始主页](assets/create-site/start-home-page.png)

   *网站模板提供的示例内容*

## 恭喜！ {#congratulations}

恭喜，您刚刚创建了第一个AEM网站！

### 后续步骤 {#next-steps}

在AEM的Adobe Experience Manager中使用页面编辑器，更新网站的 [创作内容和发布](author-content-publish.md) 章节。 了解如何配置原子组件以更新内容。 了解AEM创作和发布环境之间的差异，并了解如何将更新发布到实时站点。
