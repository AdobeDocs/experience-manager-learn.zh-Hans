---
title: 创建站点 | AEM快速站点创建
description: 了解如何使用站点创建向导生成新网站。 由Adobe提供的标准站点模板是新站点的起点。
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 0%

---

# 创建站点 {#create-site}

作为快速站点创建的一部分，请使用Adobe Experience Manager AEM中的“站点创建向导”来生成新网站。 由Adobe提供的标准站点模板用作新站点的起点。

## 先决条件 {#prerequisites}

本章中的步骤将在Adobe Experience Manager as a Cloud Service环境中进行。 确保您对AEM环境具有管理访问权限。 完成本教程时，建议使用[沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html)和[开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)。

[生产程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html)环境也可以用于本教程；但是，请确保本教程的活动不会影响正在目标环境中执行的工作，因为本教程会将内容和代码部署到目标AEM环境。

[AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)可用于本教程的部分内容。 无法在AEM SDK上执行本教程依赖云服务的各个方面，例如[使用Cloud Manager的前端管道](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html)部署主题。

有关更多详细信息，请查看[入门文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)。

## 目标 {#objective}

1. 了解如何使用站点创建向导生成新站点。
1. 了解站点模板的作用。
1. 浏览生成的AEM站点。

## 登录Adobe Experience Manager Author {#author}

第一步，登录到您的AEM as a Cloud Service环境。 AEM环境在&#x200B;**创作服务**&#x200B;和&#x200B;**Publish服务**&#x200B;之间拆分。

* **作者服务** — 在其中创建、管理和更新网站内容。 通常只有内部用户有权访问&#x200B;**作者服务**，并且处于登录屏幕后面。
* **Publish服务** — 托管实时网站。 这是最终用户将看到的服务，通常公开可用。

大部分教程将使用&#x200B;**作者服务**&#x200B;进行。

1. 导航到Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/)。 使用您的个人帐户或公司/学校帐户登录。
1. 确保在菜单中选择正确的组织，然后单击&#x200B;**Experience Manager**。

   ![Experience Cloud主页](assets/create-site/experience-cloud-home-screen.png)

1. 在&#x200B;**Cloud Manager**&#x200B;下，单击&#x200B;**启动**。
1. 将鼠标悬停在要使用的程序上，然后单击&#x200B;**Cloud Manager程序**&#x200B;图标。

   ![Cloud Manager项目图标](assets/create-site/cloud-manager-program-icon.png)

1. 在顶部菜单中，单击&#x200B;**环境**&#x200B;以查看已设置的环境。

1. 找到要使用的环境，然后单击&#x200B;**作者URL**。

   ![访问开发作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建议在此教程中使用&#x200B;**开发**&#x200B;环境。

1. 已向AEM **创作服务**&#x200B;启动新选项卡。 单击&#x200B;**使用Adobe**&#x200B;登录，您应该使用相同的Experience Cloud凭据自动登录。

1. 在重定向并验证之后，您现在应该会看到AEM开始屏幕。

   ![AEM开始屏幕](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 访问Experience Manager时遇到问题？ 查看[入门文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下载基本站点模板

站点模板为新站点提供起点。 站点模板包括一些基本主题、页面模板、配置和示例内容。 确切地说，站点模板中包含的内容取决于开发人员。 Adobe提供了&#x200B;**Basic站点模板**&#x200B;以加速新实施。

1. 打开新的浏览器选项卡，并导航到GitHub上的基本站点模板项目： [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)。 该项目是开放源码的，并许可任何人使用。
1. 单击&#x200B;**版本**&#x200B;并导航到[最新版本](https://github.com/adobe/aem-site-template-standard/releases/latest)。
1. 展开&#x200B;**Assets**&#x200B;下拉菜单并下载模板zip文件：

   ![基本站点模板Zip](assets/create-site/template-basic-zip-file.png)

   此zip文件将在下一个练习中使用。

   >[!NOTE]
   >
   > 本教程使用基本站点模板版本&#x200B;**1.1.0**&#x200B;编写。 在启动新项目以供生产使用时，始终建议使用最新版本。

## 创建新站点

接下来，使用上一个练习中的站点模板生成新站点。

1. 返回到AEM环境。 从AEM开始屏幕导航到&#x200B;**站点**。
1. 单击右上角的&#x200B;**创建** > **站点（模板）**。 这将显示&#x200B;**创建站点向导**。
1. 在&#x200B;**选择站点模板**&#x200B;下，单击&#x200B;**导入**&#x200B;按钮。

   上传从上一个练习下载的&#x200B;**.zip**&#x200B;模板文件。

1. 选择&#x200B;**基本AEM站点模板**&#x200B;并单击&#x200B;**下一步**。

   ![选择站点模板](assets/create-site/select-site-template.png)

1. 在&#x200B;**网站详细信息** > **网站标题**&#x200B;下，输入`WKND Site`。

   在现实实施中，“WKND站点”将由您公司或组织的品牌名称替代。 在本教程中，我们模拟为一个虚构的生活方式品牌“WKND”创建站点。

1. 在&#x200B;**站点名称**&#x200B;下，输入`wknd`。

   ![站点模板详细信息](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共享AEM环境，请将唯一标识符附加到&#x200B;**站点名称**。 例如 `wknd-site-johndoe`。这将确保多个用户可以完成相同的教程，而不会产生任何冲突。

1. 单击&#x200B;**创建**&#x200B;以生成站点。 当AEM完成创建网站后，在&#x200B;**成功**&#x200B;对话框中单击&#x200B;**完成**。

## 浏览新站点

1. 导航到AEM Sites控制台（如果尚未找到）。
1. 已生成新的&#x200B;**WKND站点**。 它将包含一个具有多语言层次结构的站点结构。
1. 选择页面，然后单击菜单栏中的&#x200B;**编辑**&#x200B;按钮，打开&#x200B;**英语** > **主页**&#x200B;页面：

   ![WKND站点层级](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已创建入门内容，并且有多个组件可供添加到页面中。 尝试这些组件以了解其功能。 您将在下一章中学习组件的基础知识。

   ![启动主页](assets/create-site/start-home-page.png)

   *由站点模板提供的示例内容*

## 恭喜！ {#congratulations}

恭喜，您刚刚创建了您的第一个AEM站点！

### 后续步骤 {#next-steps}

使用Adobe Experience Manager AEM中的页面编辑器在[创作内容和发布](author-content-publish.md)章节中更新网站的内容。 了解如何配置原子组件以更新内容。 了解AEM Author环境和Publish环境之间的区别，并了解如何将更新发布到实时网站。
