---
title: 创建站点
seo-title: AEM Sites入门 — 创建站点
description: 使用AEM Adobe Experience Manager中的站点创建向导生成新网站。 由Adobe提供的标准站点模板用作新站点的起点。
sub-product: 站点
version: Cloud Service
type: Tutorial
topic: 内容管理
feature: 核心组件，页面编辑器
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---


# 创建站点{#create-site}

>[!CAUTION]
>
> 此处展示的快速站点创建功能将在2021年下半年发布。 相关文档可供预览使用。

本章介绍在Adobe Experience Manager中创建新站点。 标准站点模板由Adobe提供，它用作起点。

## 前提条件 {#prerequisites}

本章中的步骤将作为Cloud Service环境在Adobe Experience Manager中进行。 确保您对AEM环境具有管理访问权限。 完成本教程时，建议使用[沙箱项目](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html)和[开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)。

有关详细信息，请查阅[入门文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)。

## 目标 {#objective}

1. 了解如何使用站点创建向导生成新站点。
1. 了解网站模板的角色。
1. 浏览生成的AEM站点。

## 登录到Adobe Experience Manager作者{#author}

首先，以Cloud Service环境身份登录AEM。 AEM环境在&#x200B;**作者服务**&#x200B;和&#x200B;**发布服务**&#x200B;之间拆分。

* **创作服务**  — 创建、管理和更新站点内容的位置。通常，只有内部用户才能访问&#x200B;**作者服务**&#x200B;并位于登录屏幕后。
* **发布服务**  — 托管实时网站。这是最终用户将看到的服务，通常公开提供。

大部分教程将使用&#x200B;**作者服务**&#x200B;进行。

1. 导航到Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)。 使用您的个人帐户或公司/学校帐户登录。
1. 确保在菜单中选择了正确的“组织”，然后单击&#x200B;**Experience Manager**。

   ![Experience Cloud主页](assets/create-site/experience-cloud-home-screen.png)

1. 在&#x200B;**云管理器**&#x200B;下，单击&#x200B;**启动**。
1. 将鼠标悬停在要使用的项目上，然后单击&#x200B;**Cloud Manager项目**&#x200B;图标。

   ![Cloud Manager项目图标](assets/create-site/cloud-manager-program-icon.png)

1. 在顶部菜单中，单击&#x200B;**环境**&#x200B;以视图设置的环境。

1. 查找要使用的环境并单击&#x200B;**作者URL**。

   ![访问开发人员作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建议对本教程使用&#x200B;**开发**&#x200B;环境。

1. 将启动一个新选项卡至AEM **作者服务**。 单击&#x200B;**使用Adobe**&#x200B;登录，您应使用相同的Experience Cloud凭据自动登录。

1. 在重定向和验证后，您现在应当看到AEM开始屏幕。

   ![AEM 开始屏幕](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 访问Experience Manager时遇到问题？ 查看[入门文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下载基本站点模板

“站点模板”为新站点提供起点。 网站模板包括一些基本主题、页面模板、配置和示例内容。 站点模板中包含的具体内容由开发人员决定。 Adobe提供&#x200B;**基本站点模板**&#x200B;以加速新的实施。

1. 打开新的浏览器选项卡，然后导航到GitHub上的“基本站点模板”项目：[https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic)。 该项目是开源的，并且许可供任何人使用。
1. 单击“**发行版**”并导航至[最新发行版](https://github.com/adobe/aem-site-template-basic/releases/latest)。
1. 展开&#x200B;**资产**&#x200B;下拉列表并下载模板zip文件：

   ![基本网站模板Zip](assets/create-site/template-basic-zip-file.png)

   此zip文件将用于下一个练习。

   >[!NOTE]
   >
   > 本教程是使用“基本站点模板”版本&#x200B;**5.0.0**&#x200B;编写的。 如果启动新项目，则始终建议使用最新版本。

## 创建新站点

然后，使用上一个练习中的站点模板生成新站点。

1. 返回AEM环境。 从AEM开始屏幕导航到&#x200B;**Sites**。
1. 在右上角，单击&#x200B;**创建** > **站点（模板）**。 这将显示&#x200B;**创建站点向导**。
1. 在&#x200B;**选择站点模板**&#x200B;下，单击&#x200B;**导入**&#x200B;按钮。

   上传从上一个练习下载的&#x200B;**.zip**&#x200B;模板文件。

1. 选择&#x200B;**基本AEM站点模板**，然后单击&#x200B;**下一步**。

   ![选择站点模板](assets/create-site/select-site-template.png)

1. 在&#x200B;**站点详细信息** > **站点标题**&#x200B;下，输入`WKND Site`。
1. 在&#x200B;**站点名称**&#x200B;下，输入`wknd`。

   ![网站模板详细信息](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共享AEM环境，请向&#x200B;**站点名称**&#x200B;附加唯一标识符。 例如`wknd-johndoe`。 这将确保多个用户可以完成同一教程，而不会发生任何冲突。

1. 单击&#x200B;**创建**&#x200B;以生成网站。 当AEM完成创建网站后，单击&#x200B;**成功**&#x200B;对话框中的&#x200B;**完成**。

## 浏览新站点

1. 导航到AEM Sites控制台（如果尚未访问）。
1. 已生成新的&#x200B;**WKND站点**。 它将包括具有多语言层次结构的站点结构。
1. 通过选择页面并单击菜单栏中的&#x200B;**编辑**&#x200B;按钮，打开&#x200B;**英语** > **主页**&#x200B;页面：

   ![WKND站点层次](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已创建起动内容，可向页面添加多个组件。 尝试这些组件，了解其功能。 您将在下一章中学习组件的基础知识。

   ![开始主页](assets/create-site/start-home-page.png)

   *网站模板提供的示例内容*

## 恭喜！{#congratulations}

祝贺您，您刚刚创建了您的第一个AEM站点！

### 后续步骤{#next-steps}

使用AEM Adobe Experience Manager中的页面编辑器更新[创作内容和发布](author-content-publish.md)章中站点的内容。 了解如何配置原子组件以更新内容。 了解AEM作者环境和发布应用程序之间的差异，并了解如何将更新发布到实时站点。
