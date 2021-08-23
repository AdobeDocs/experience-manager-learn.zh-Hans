---
title: 将Adobe Experience Manager与Adobe Target集成
seo-title: 一篇文章介绍了将Adobe Experience Manager(AEM)与Adobe Target集成以提供个性化内容的不同方式。
description: 一篇文章，介绍如何针对不同情景使用Adobe Target设置Adobe Experience Manager。
seo-description: 一篇文章，介绍如何针对不同情景使用Adobe Target设置Adobe Experience Manager。
feature: 体验片段
topic: 个性化
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '700'
ht-degree: 4%

---


# 将Adobe Experience Manager与Adobe Target集成

在本节中，我们将讨论如何针对不同的情况与Adobe Target一起设置Adobe Experience Manager。 根据您的情景和组织要求。

* **添加Adobe Target JavaScript库（所有方案都需要此库）**
对于在AEM上托管的网站，您可以使用Launch将Target库添加到您的 [网站](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)。Launch提供了一种简单的方式来部署和管理所有用来改善相关客户体验的标记。
* **添加Adobe TargetCloud Services（体验片段方案的必需选项）**
对于希望使用体验片段选件在Adobe Target中创建活动的AEM客户，您将需要使用旧版Cloud Services将Adobe Target与AEM集成。要将体验片段作为HTML/JSON选件从AEM推送到Target，并使选件与AEM保持同步，需要此集成。 
*实施情景1需要此集成。*

## 前提条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5（建议&#x200B;*最新的Service Pack*）
   * 下载AEM WKND引用站点包
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心组件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [数字数据层](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

* **环境**
   * Java 1.8或Java 11(仅限AEM 6.5+)
   * Apache Maven（3.3.9或更高版本）
   * 铬黄

>[!NOTE]
>
> 需要从[Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html)为客户配置Experience Platform Launch和Adobe I/O，或联系系统管理员

### 设置AEM{#set-up-aem}

AEM创作和发布实例是完成本教程所必需的。 我们在`http://localhost:4502`上运行创作实例，并在`http://localhost:4503`上运行发布实例。 有关更多信息，请参阅：[设置本地AEM开发环境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 设置AEM创作实例和发布实例

1. 获取[AEM快速入门Jar和许可证的副本。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在计算机上创建文件夹结构，如下所示：
   ![文件夹结构](assets/implementation/aem-setup-1.png)
3. 将快速入门Jar重命名为`aem-author-p4502.jar`，并将其放在`/author`目录下。 在`/author`目录下添加`license.properties`文件。
   ![AEM创作实例](assets/implementation/aem-setup-author.png)
4. 制作快速入门Jar的副本，将其重命名为`aem-publish-p4503.jar`，然后将其放在`/publish`目录下。 在`/publish`目录下添加`license.properties`文件的副本。
   ![AEM发布实例](assets/implementation/aem-setup-publish.png)
5. 双击`aem-author-p4502.jar`文件以安装Author实例。 这将启动在本地计算机上的端口4502上运行的创作实例。
6. 使用以下凭据登录，成功登录后，将会将您定向到AEM主页屏幕。
用户名：**admin**
密码：**admin**
   ![AEM发布实例](assets/implementation/aem-author-home-page.png)
7. 双击`aem-publish-p4503.jar`文件以安装发布实例。 您可能会注意到在您的浏览器中为发布实例打开了一个新选项卡，该选项卡在端口4503上运行并显示WeRetail主页。 我们将在本教程中使用WKND引用站点，接下来让我们在创作实例上安装包。
8. 在Web浏览器的`http://localhost:4502`处导航到AEM作者。 在“AEM开始”屏幕上，导航至&#x200B;*[工具>部署>包](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下载并上传AEM的包(上面在&#x200B;*[先决条件> AEM](#aem)*&#x200B;下列出)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM创作上安装包后，在AEM包管理器中选择每个已上传的包，然后选择&#x200B;**更多>复制**，以确保将包部署到AEM发布。
11. 此时，您已成功安装了WKND参考站点以及本教程所需的所有其他包。

[下一章](./using-launch-adobe-io.md):在下一章中，您将将Launch与AEM集成。
