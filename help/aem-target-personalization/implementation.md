---
title: 将Adobe Experience Manager与Adobe Target整合
seo-title: 一篇文章介绍了将Adobe Experience Manager(AEM)与Adobe Target集成以提供个性化内容的不同方法。
description: 一篇文章介绍如何在不同场景下与Adobe Target建立Adobe Experience Manager。
seo-description: 一篇文章介绍如何在不同场景下与Adobe Target建立Adobe Experience Manager。
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 3%

---


# 将Adobe Experience Manager与Adobe Target整合

在本节中，我们将讨论如何针对不同情况与Adobe Target建立Adobe Experience Manager。 根据您的方案和组织要求。

* **添加Adobe TargetJavaScript库（所有场景都需要）**
对于托管在AEM上的站点，您可以使用Launch将目标库添加到 [您的站点](https://docs.adobe.com/content/help/en/launch/using/overview.html)。Launch提供了一种部署和管理支持相关客户体验所需的所有标记的简单方法。
* **为AEM客户添加Adobe TargetCloud Services（体验片段场景所需）**
，如果希望使用体验片段优惠在Adobe Target创建活动，您需要使用旧Cloud Services将Adobe Target与AEM集成。需要进行此集成，才能将体验片段从AEM推送到目标，作为HTML/JSON优惠，并使优惠与AEM保持同步。 
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
   * 访问您的组织Adobe Experience Cloud- <https://>`<yourcompany>`.experienccloud.adobe.com
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
> 客户需要从[Experience Platform Launch支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html)向和Adobe I/O提供Adobe或联系系统管理员

### 设置AEM{#set-up-aem}

AEM作者和发布实例是完成本教程所必需的。 我们在`http://localhost:4502`上运行创作实例，在`http://localhost:4503`上运行发布实例。 有关详细信息，请参阅：[设置本地AEM开发环境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)。

#### 设置AEM作者实例和发布实例

1. 获取[AEM快速启动Jar和许可证的副本。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在计算机上创建文件夹结构，如下所示：
   ![文件夹结构](assets/implementation/aem-setup-1.png)
3. 将快速启动程序jar重命名为`aem-author-p4502.jar`，并将其放在`/author`目录下。 在`/author`目录下添加`license.properties`文件。
   ![AEM作者实例](assets/implementation/aem-setup-author.png)
4. 制作快速启动程序jar的副本，将其重命名为`aem-publish-p4503.jar`，并将其放在`/publish`目录下。 在`/publish`目录下添加`license.properties`文件的副本。
   ![AEM Publish实例](assets/implementation/aem-setup-publish.png)
5. 多次单击`aem-author-p4502.jar`文件以安装作者实例。 这将开始在本地计算机上的端口4502上运行的作者实例。
6. 使用以下凭据登录，成功登录后，您将进入AEM主页屏幕。
用户名：**admin**
密码：**admin**
   ![AEM Publish实例](assets/implementation/aem-author-home-page.png)
7. 多次单击`aem-publish-p4503.jar`文件以安装发布实例。 您可以注意到在您的发布实例的浏览器中打开了一个新选项卡，该选项卡运行于端口4503并显示WeRetail主页。 我们将使用本教程的WKND参考站点，让我们在创作实例上安装这些包。
8. 在您的Web浏览器中导航到AEM作者，网址为`http://localhost:4502`。 在AEM开始屏幕上，导航到&#x200B;*[工具>部署>包](http://localhost:4502/crx/packmgr/index.jsp)*。
9. 下载并上传AEM的包(上面列在&#x200B;*[先决条件> AEM](#aem)*&#x200B;下)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM作者上安装包后，在AEM Package Manager中选择每个已上传的包，然后选择&#x200B;**更多>复制**&#x200B;以确保将包部署到AEM发布。
11. 此时，您已成功安装了WKND参考站点和本教程所需的所有其他软件包。

[下一章](./using-launch-adobe-io.md):在下一章中，您将将Launch与AEM集成。
