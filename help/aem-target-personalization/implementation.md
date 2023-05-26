---
title: 将Adobe Experience Manager与Adobe Target集成
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: 一篇介绍如何针对不同场景使用Adobe Target设置Adobe Experience Manager的文章。
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 3%

---

# 将Adobe Experience Manager与Adobe Target集成

在此部分中，我们将讨论如何为各种场景使用Adobe Target设置Adobe Experience Manager。 根据您的方案和组织要求。

* **添加Adobe Target JavaScript库（所有方案均需要）**
对于在AEM上托管的站点，您可以使用将Target库添加到您的站点， [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch提供了一种简单的方式来部署和管理所有必要的标记，以便加强相关客户体验。
* **添加Adobe TargetCloud Services（体验片段方案所必需的）**
对于希望使用体验片段选件在Adobe Target中创建活动的AEM客户，您需要使用旧版Cloud Services将Adobe Target与AEM集成。 要将体验片段作为HTML/JSON选件从AEM推送到Target，并保持选件与AEM同步，需要此集成。 
*实施场景1需要此集成。*

## 前提条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*建议使用最新的服务包*)
   * 下载AEM WKND引用站点包
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心组件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [数字数据层](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 使用以下解决方案配置的Experience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

* **环境**
   * Java 1.8或Java 11(仅限AEM 6.5+)
   * Apache Maven （3.3.9或更高版本）
   * 铬黄

>[!NOTE]
>
> 客户需要从中配置Experience Platform Launch和Adobe I/O [Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html) 或联系您的系统管理员

### 设置AEM{#set-up-aem}

AEM创作和发布实例是完成本教程所必需的。 我们已在运行创作实例 `http://localhost:4502` 和发布实例运行于 `http://localhost:4503`. 有关详细信息，请参阅： [设置本地AEM开发环境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### 设置AEM创作和发布实例

1. 获取 [AEM Quickstart Jar和许可证。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在计算机上创建文件夹结构，如下所示：
   ![文件夹结构](assets/implementation/aem-setup-1.png)
3. 将快速入门jar重命名为 `aem-author-p4502.jar` 然后放在下面 `/author` 目录。 添加 `license.properties` 文件位于 `/author` 目录。
   ![AEM创作实例](assets/implementation/aem-setup-author.png)
4. 制作快速入门jar的副本，将其重命名为 `aem-publish-p4503.jar` 然后放在下面 `/publish` 目录。 添加副本 `license.properties` 文件位于 `/publish` 目录。
   ![AEM发布实例](assets/implementation/aem-setup-publish.png)
5. 双击 `aem-author-p4502.jar` 文件以安装创作实例。 这将启动创作实例，该实例在本地计算机上的端口4502上运行。
6. 使用下面的凭据登录，在成功登录后，您将被定向到AEM主页屏幕。
用户名： **管理员**
密码： **管理员**
   ![AEM发布实例](assets/implementation/aem-author-home-page.png)
7. 双击 `aem-publish-p4503.jar` 文件，以安装发布实例。 您会注意到，您的发布实例在浏览器中打开了一个新选项卡，该选项卡在端口4503上运行，并显示WeRetail主页。 我们在本教程中使用WKND引用站点，并在创作实例上安装包。
8. 在Web浏览器中导航到AEM作者，网址为 `http://localhost:4502`. 在AEM开始屏幕上，导航到 *[“工具”>“部署”>“包”](http://localhost:4502/crx/packmgr/index.jsp)*.
9. 下载并上传AEM包(如上面所列，位于 *[先决条件> AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM作者上安装包后，在AEM包管理器中选择每个上传的包，然后选择 **更多>复制** 以确保将包部署到AEM发布。
11. 此时，您已成功安装WKND引用站点以及本教程所需的所有其他包。

[下一章节](./using-launch-adobe-io.md)：在下一章中，您将将Launch与AEM集成。
