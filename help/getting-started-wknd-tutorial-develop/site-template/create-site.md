---
title: 创建网站 | AEM 快速网站创建
description: 了解如何使用网站创建向导生成一个新网站。Adobe 提供的标准网站模板是新网站的起点。
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 198
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '959'
ht-degree: 100%

---

# 创建网站 {#create-site}

作为快速网站创建的一部分，使用 Adobe Experience Manager (AEM) 中的网站创建向导来生成一个新网站。Adobe 提供的标准网站模板可用作新网站的起点。

## 先决条件 {#prerequisites}

本章中的步骤在 Adobe Experience Manager as a Cloud Service 环境中进行。确保您具有 AEM 环境的管理访问权限。建议使用一个[沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html?lang=zh-Hans)和[开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=zh-Hans)完成本教程。

本教程也可以使用[生产程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=zh-Hans)环境，但是请确保本教程的活动不会影响在目标环境上执行的工作，因为本教程会将内容和代码部署到目标 AEM 环境。

本教程的有些部分可以使用 [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hans)。本教程中一些依赖云服务的方面，例如[使用 Cloud Manager 的前端管道部署主题](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=zh-Hans)，无法在 AEM SDK 上执行。

查看[加入文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=zh-Hans)，了解更多详情。

## 目标 {#objective}

1. 了解使用网站创建向导生成新网站。
1. 了解网站模板的作用。
1. 浏览生成的 AEM 网站。

## 登录 Adobe Experience Manager Author {#author}

第一步，请登录到您的 AEM as a Cloud Service 环境。AEM 环境分为&#x200B;**作者服务**&#x200B;和&#x200B;**发布服务**。

* **作者服务** - 这是创建、管理和更新网站内容的地方。通常只有内部用户可以访问&#x200B;**作者服务**，并且需要登录。
* **发布服务** - 托管上线网站。这是最终用户将看到的服务，通常是对大众公开的。

教程中的大部分内容将使用&#x200B;**作者服务**&#x200B;完成。

1. 导航到 Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)。使用您的个人帐户或公司/学校帐户登录。
1. 确保在菜单中选择正确的组织，然后点击 **Experience Manager**。

   ![Experience Cloud 主页](assets/create-site/experience-cloud-home-screen.png)

1. 在 **Cloud Manager** 中点击&#x200B;**启动**。
1. 将鼠标悬停在您想使用的程序上，然后点击 **Cloud Manager 程序**&#x200B;图标。

   ![Cloud Manager 程序图标](assets/create-site/cloud-manager-program-icon.png)

1. 在顶部菜单中点击&#x200B;**环境**，查看已配置的环境。

1. 找到您想使用的环境，然后点击&#x200B;**作者 URL**。

   ![访问开发作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建议为本教程使用&#x200B;**开发**&#x200B;环境。

1. AEM **作者服务**&#x200B;中启动了一个新的选项卡。点击&#x200B;**使用 Adobe 登录**，您会通过相同的 Experience Cloud 凭据自动登录。

1. 重定向和身份验证之后，您现在会看到 AEM 开始屏幕。

   ![AEM 开始屏幕](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 访问 Experience Manager 时遇到问题？查看[加入文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=zh-Hans)

## 下载基本网站模板

网站模板为新网站提供了一个起点。网站模板包括一些基本主题、页面模板、配置和示例内容。网站模板中具体包含什么，这由开发人员决定。Adobe 提供了一个&#x200B;**基本网站模板**，以加速新的实施。

1. 打开一个新的浏览器选项卡，导航到 GitHub 上的基本网站模板项目：[https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)。此项目是开源的，任何人都可以获得使用许可证。
1. 点击&#x200B;**发行版本**，然后导航至[最新发行版本](https://github.com/adobe/aem-site-template-standard/releases/latest)。
1. 展开&#x200B;**资产**&#x200B;下拉菜单，下载模板 zip 文件：

   ![基本网站模板 Zip](assets/create-site/template-basic-zip-file.png)

   此 zip 文件将在下一个练习中使用。

   >[!NOTE]
   >
   > 本教程编写时使用版本 **1.1.0** 的基本网站模板。启动一个新项目用于生产时，始终建议使用最新版本。

## 创建一个新网站

接下来，使用上一个练习中的网站模板生成一个新网站。

1. 返回到 AEM 环境。从 AEM 开始屏幕导航到 **Sites**。
1. 点击右上角的&#x200B;**创建** > **网站（模板）**。现在会打开&#x200B;**创建网站向导**。
1. 在&#x200B;**选择网站模板**&#x200B;中点击&#x200B;**导入**&#x200B;按钮。

   上传在上一个练习中下载的 **.zip** 模板文件。

1. 选择&#x200B;**基本 AEM 网站模板**，然后点击&#x200B;**下一步**。

   ![选择网站模板](assets/create-site/select-site-template.png)

1. 在&#x200B;**网站详情** > **网站标题**&#x200B;中输入 `WKND Site`。

   在实际实施中，“WKND 网站”将替换成您的公司或组织的品牌名称。在本教程中，我们模拟为虚构的生活方式品牌“WKND”创建一个网站。

1. 在&#x200B;**网站名称**&#x200B;中输入 `wknd`。

   ![网站模板详细信息](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用一个共享 AEM 环境，请在&#x200B;**网站名称**&#x200B;上附加一个唯一标识符。例如 `wknd-site-johndoe`。这可以确保多个用户完成相同的教程时不会发生任何冲突。

1. 点击&#x200B;**创建**，生成网站。当 AEM 完成网站创建后，在&#x200B;**成功**&#x200B;对话框中点击&#x200B;**完成**。

## 浏览新网站

1. 导航到 AEM Sites 控制台。
1. 一个新的 **WKND 网站** 已生成。它将包含一个具有多语言层级的网站结构。
1. 打开&#x200B;**英语** > **主页**，选择此页面并点击菜单栏中的&#x200B;**编辑**&#x200B;按钮：

   ![WKND 网站层级](assets/create-site/wknd-site-starter-hierarchy.png)

1. 我们现在已创建了入门内容，有多个组件可供添加到页面上。试用这些组件，了解各种功能。您将在下一章中学习一个组件的基本知识。

   ![开始主页](assets/create-site/start-home-page.png)

   *网站模板提供的示例内容*

## 恭喜！ {#congratulations}

祝贺您创建了您的第一个 AEM 网站！

### 后续步骤 {#next-steps}

使用 Adobe Experience Manager (AEM) 中的页面编辑器更新[创作内容和发布](author-content-publish.md)一章中的网站内容。了解如何配置原子组件来更新内容。了解 AEM 作者与发布环境之间的区别，学习如何将更新发布到上线网站。
