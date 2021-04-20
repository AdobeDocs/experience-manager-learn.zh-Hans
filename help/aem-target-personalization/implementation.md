---
title: 将Adobe Experience Manager与Adobe Target集成
seo-title: 一篇文章介绍了将Adobe Experience Manager(AEM)与Adobe Target集成以交付个性化内容的不同方式。
description: 一篇文章，其中介绍如何针对不同场景使用Adobe Target设置Adobe Experience Manager。
seo-description: 一篇文章，其中介绍如何针对不同场景使用Adobe Target设置Adobe Experience Manager。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 4%

---


# 将Adobe Experience Manager与Adobe Target集成

在本节中，我们将讨论如何针对不同的场景与Adobe Target一起设置Adobe Experience Manager。 根据您的方案和组织要求。

* **添加Adobe Target JavaScript库（所有场景都需要）对**
于AEM上托管的站点，可以使用Launch将目标库添加到 [站点](https://docs.adobe.com/content/help/en/launch/using/overview.html)。Launch提供了部署和管理支持相关客户体验所需的所有标记的简单方式。
* **添加Adobe TargetCloud Services（体验片段方案所需）**
对于希望使用体验片段优惠在Adobe Target中创建活动的AEM客户，您需要使用旧Cloud Services将Adobe Target与AEM集成。需要此集成，才能将Experience Fragments从AEM推送到HTML/JSON优惠目标，并使优惠与AEM保持同步。 
*实施方案1需要此集成。*

## 前提条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5（建议使用&#x200B;*最新的Service Pack*）
   * 下载AEM WKND参考站点包
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
   * Java 1.8或Java 11(仅AEM 6.5+)
   * Apache Maven（3.3.9或更高版本）
   * 铬黄

>[!NOTE]
>
> 客户需要从[Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html)向Experience Platform Launch和Adobe I/O进行配置，或联系您的系统管理员

### 设置AEM{#set-up-aem}

AEM作者和发布实例是完成本教程所必需的。 我们在`http://localhost:4502`上运行创作实例，在`http://localhost:4503`上运行发布实例。 有关详细信息，请参阅：[设置本地AEM开发环境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 设置AEM作者实例和发布实例

1. 获取[AEM快速启动程序Jar和许可证的副本。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在计算机上创建文件夹结构，如下所示：
   ![文件夹结构](assets/implementation/aem-setup-1.png)
3. 将快速启动程序jar重命名为`aem-author-p4502.jar`，并将其放在`/author`目录下。 在`/author`目录下添加`license.properties`文件。
   ![AEM作者实例](assets/implementation/aem-setup-author.png)
4. 制作快速启动程序jar的副本，将其重命名为`aem-publish-p4503.jar`，并将其放在`/publish`目录下。 在`/publish`目录下添加`license.properties`文件的副本。
   ![AEM Publish实例](assets/implementation/aem-setup-publish.png)
5. 多次单击`aem-author-p4502.jar`文件以安装Author实例。 这将开始在本地计算机上的端口4502上运行的作者实例。
6. 使用以下凭据登录，成功登录后，您将被定向到AEM主页屏幕。
用户名：**admin**
密码：**admin**
   ![AEM Publish实例](assets/implementation/aem-author-home-page.png)
7. 多次单击`aem-publish-p4503.jar`文件以安装发布实例。 您可以看到在您的发布实例的浏览器中打开了一个新选项卡，它运行在端口4503上并显示WeRetail主页。 我们将使用本教程的WKND参考站点，并让我们在创作实例上安装包。
8. 在您的Web浏览器中导航到`http://localhost:4502`的AEM作者。 在AEM开始屏幕上，导航到&#x200B;*[工具>部署>包](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下载并上传AEM的包(在&#x200B;*[先决条件> AEM](#aem)*&#x200B;下列出)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM作者上安装包后，在AEM包管理器中选择每个已上传的包，然后选择&#x200B;**更多>复制**&#x200B;以确保将包部署到AEM发布。
11. 此时，您已成功安装了WKND参考站点和本教程所需的所有其他包。

[下一章](./using-launch-adobe-io.md):在下一章，您将将Launch与AEM集成。
