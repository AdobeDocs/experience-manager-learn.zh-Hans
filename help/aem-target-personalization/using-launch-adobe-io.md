---
title: 使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
seo-title: 使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
description: 分步说明如何使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
seo-description: 分步说明如何使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
feature: 体验片段
topic: 个性化
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# 通过Adobe Experience Platform Launch控制台使用Adobe I/O

## 前提条件

* [AEM创作和发](./implementation.md#set-up-aem) 布实例在localhost端口4502和4503上取消
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

      >[!NOTE]
      >您应该有权在Launch中开发、批准、发布、管理扩展和管理环境。 如果由于用户界面选项不可用而无法完成其中的任何步骤，请联系Experience Cloud管理员以请求获取访问权限。 有关Launch权限的更多信息，请[参阅此文档](https://docs.adobelaunch.com/administration/user-permissions)。


* **浏览器插件**
   * Adobe Experience Cloud Debugger([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Launch和DTM交换机([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 涉及的用户

对于此集成，需要涉及以下受众，要执行某些任务，您可能需要管理访问权限。

* 开发人员
* AEM管理员
* Experience Cloud管理员

## 简介

AEM提供了与Experience Platform Launch的开箱即用集成。 通过此集成，AEM管理员可以通过易于使用的界面轻松配置Experience Platform Launch，从而减少了配置这两个工具时的工作量和错误数。 而且，只需将Adobe Target扩展添加到Experience Platform Launch，就可帮助我们在AEM网页上使用Adobe Target的所有功能。

在本节中，我们将介绍以下集成步骤：

* 启动
   * 创建Launch资产
   * 添加Target扩展
   * 创建数据元素
   * 创建页面规则
   * 设置环境
   * 生成和发布
* AEM
   * 创建Cloud Service
   * 创建

### 启动

#### 创建Launch资产

资产是一个容器，在将标记部署到网站时可在其中填充扩展、规则、数据元素和库。

1. 导航到您的组织[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. 使用Adobe ID登录，并确保您所在的组织正确。
3. 在解决方案切换器中，单击&#x200B;**Launch**，然后选择&#x200B;**转到Launch**&#x200B;按钮。

   ![Experience Cloud — 启动](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 确保您位于正确的组织中，然后继续创建Launch资产。
   ![Experience Cloud — 启动](assets/using-launch-adobe-io/launch-create-property.png)

   *有关创建资产的更多信息，请参 [阅产](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) 品文档中的创建资产。*
5. 单击&#x200B;**New Property**&#x200B;按钮
6. 为您的资产提供名称(例如，*AEM Target Tutorial*)
7. 对于域，输入&#x200B;*localhost.com*，因为这是运行WKND演示网站的域。 尽管“*Domain*”字段是必需的，但Launch属性将在实施该属性的任何域上工作。 此字段的主要用途是在规则生成器中预填充菜单选项。
8. 单击&#x200B;**Save**&#x200B;按钮。

   ![Launch — 新资产](assets/using-launch-adobe-io/exc-launch-property.png)

9. 打开之前创建的资产，然后单击扩展选项卡。

#### 添加Target扩展

Adobe Target扩展支持使用适用于新版Web的Target JavaScript SDK的客户端实施，`at.js`。 仍在使用Target旧库`mbox.js`、[的客户应升级到at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html)以使用Launch。

Target扩展包含两个主要部分：

* 扩展配置，用于管理核心库设置
* 用于执行以下操作的规则操作：
   * 加载Target(at.js)
   * 将参数添加到所有Mbox
   * 将参数添加到全局Mbox
   * Fire Global Mbox

1. 在&#x200B;**Extensions**&#x200B;下，您可以看到已为Launch资产安装的扩展列表。 (默认情况下，安装[Experience Platform Launch核心扩展](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html))
2. 单击&#x200B;**扩展目录**&#x200B;选项，然后在筛选器中搜索Target。
3. 选择最新版本的Adobe Target at.js ，然后单击&#x200B;**Install**选项。
   ![Launch — 新建属性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 单击&#x200B;**配置**按钮，您会注意到配置窗口中已导入您的Target帐户凭据，以及此扩展的at.js版本。
   ![Target — 扩展配置](assets/using-launch-adobe-io/launch-target-extension-2.png)

   当通过异步Launch嵌入代码部署Target时，您应当在页面上的Launch嵌入代码之前对预隐藏代码片段进行硬编码，以管理内容闪烁。 我们稍后会了解关于预隐藏狙击手的更多信息。 您可以在此处下载预隐藏代码片段[](assets/using-launch-adobe-io/prehiding.js)

5. 单击&#x200B;**Save**&#x200B;以完成将Target扩展添加到Launch资产中的操作，此时您应该能够看到在&#x200B;**Installed**&#x200B;扩展列表下列出的Target扩展。

6. 重复上述步骤以搜索“Experience CloudID服务”扩展并安装该扩展。
   ![扩展 — Experience CloudID服务](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 设置环境

1. 单击网站资产的&#x200B;**Environment**&#x200B;选项卡，您可以看到为网站资产创建的环境列表。 默认情况下，我们为开发、暂存和生产分别创建了一个实例。

![数据元素 — 页面名称](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 生成和发布

1. 单击网站资产的&#x200B;**Publishing**&#x200B;选项卡，然后让我们创建一个库以生成更改并将其部署到开发环境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 将您所做的更改从开发发布到暂存环境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 运行&#x200B;**Build for Staging选项**。
4. 生成完成后，运行&#x200B;**Approve for Publishing**，这会将您所做的更改从暂存环境移动到生产环境。
   ![暂存到生产环境](assets/using-launch-adobe-io/build-staging.png)
5. 最后，运行&#x200B;**生成并发布到生产**选项，以将更改推送到生产环境。
   ![构建并发布到生产环境](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予Adobe I/O集成使用相应的[角色选择工作区的权限，以允许中心团队仅在少数几个工作区中进行API驱动的更改](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)。

1. 在AEM中使用来自Adobe I/O的凭据创建IMS集成。（01:12到03:55）
2. 在Experience Platform Launch中，创建资产。 （覆盖于[上方](#create-launch-property)）
3. 使用步骤1中的IMS集成，创建Experience Platform Launch集成以导入Launch资产。
4. 在AEM中，使用浏览器配置将Experience Platform Launch集成映射到网站。 （05时28分至06时14分）
5. 手动验证集成。 （06时15分至06时33分）
6. 使用Launch/DTM浏览器插件。 （06时34分至06时50分）
7. 使用Adobe Experience Cloud Debugger浏览器插件。 （06时51分至07时22分）

此时，您已使用Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options)成功将[AEM与Adobe Target集成，如选项1中所述。

要使用AEM Experience Fragment选件来支持您的个性化活动，请继续阅读下一章，并使用旧版云服务将AEM与Adobe Target集成。
