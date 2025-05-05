---
title: 将AEM Sites与Adobe Target集成
description: 一篇介绍如何为各种场景使用Adobe Target设置Adobe Experience Manager的文章。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# 将AEM Sites与Adobe Target集成

在此部分中，我们将讨论如何为各种场景使用Adobe Target设置Adobe Experience Manager Sites。 根据您的方案和组织要求。

* **添加Adobe Target JavaScript库（所有方案均需要）**
对于在AEM上托管的网站，您可以使用Adobe Experience Platform[&#128279;](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)中的标记将Target库添加到您的网站。 标记提供了一种简单的方式来部署和管理所有加强相关客户体验所需的标记。
* **添加Adobe TargetCloud Service（体验片段方案所需）**
对于希望使用体验片段选件在Adobe Target中创建活动的AEM客户，您需要使用旧版Cloud Service将Adobe Target与AEM集成。 要将体验片段作为HTML/JSON选件从AEM推送到Target，并保持这些选件与AEM同步，需要此集成。 *实施方案1.*&#x200B;需要此集成

## 先决条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 （建议使用&#x200B;*最新的Service Pack*）
   * 下载AEM WKND引用站点包
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心组件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [数字数据层](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解决方案
      * [数据收集](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

* **环境**
   * Java 1.8或Java 11(仅限AEM 6.5+)
   * Apache Maven （3.3.9或更高版本）
   * Chrome

>[!NOTE]
>
> 客户需要从[Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html)中配置数据收集和Adobe I/O，或与您的系统管理员联系

### 设置AEM{#set-up-aem}

AEM创作和发布实例是完成本教程所必需的。 作者实例在`http://localhost:4502`上运行，发布实例在`http://localhost:4503`上运行。 有关详细信息，请参阅：[设置本地AEM开发环境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 设置AEM创作和Publish实例

1. 获取[AEM Quickstart Jar的副本和许可证。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在计算机上创建文件夹结构，如下所示：
   ![文件夹结构](assets/implementation/aem-setup-1.png)
3. 将快速入门Jar重命名为`aem-author-p4502.jar`，并将其放在`/author`目录下。 在`/author`目录下添加`license.properties`文件。
   ![AEM创作实例](assets/implementation/aem-setup-author.png)
4. 制作快速入门Jar的副本，将其重命名为`aem-publish-p4503.jar`，并将其放在`/publish`目录下。 在`/publish`目录下添加`license.properties`文件的副本。
   ![AEM Publish实例](assets/implementation/aem-setup-publish.png)
5. 双击`aem-author-p4502.jar`文件以安装创作实例。 这将启动创作实例，该实例在本地计算机上的端口4502上运行。
6. 使用下面的凭据登录，在成功登录后，您将被定向到AEM主页屏幕。
用户名：**管理员**
密码： **管理员**
   ![AEM Publish实例](assets/implementation/aem-author-home-page.png)
7. 双击`aem-publish-p4503.jar`文件以安装发布实例。 您会注意到发布实例在浏览器中打开了一个新选项卡，该选项卡在端口4503上运行，并显示WeRetail主页。 我们在本教程中使用WKND参考站点，并在创作实例上安装包。
8. 在Web浏览器的`http://localhost:4502`处导航到AEM作者。 在AEM开始屏幕上，导航到&#x200B;*[工具>部署>包](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下载并上传AEM程序包(上面列在&#x200B;*[先决条件> AEM](#aem)*&#x200B;下)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM Author上安装包后，在AEM Package Manager中选择每个已上载的包，然后选择&#x200B;**更多>复制**，以确保将包部署到AEM Publish。
11. 此时，您已成功安装WKND引用站点以及本教程所需的所有其他包。

[下一章](./using-launch-adobe-io.md)：在下一章中，您将标记与AEM集成。
