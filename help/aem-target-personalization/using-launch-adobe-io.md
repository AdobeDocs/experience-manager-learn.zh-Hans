---
title: 使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
seo-title: 使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
description: 逐步介绍如何使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
seo-description: 逐步介绍如何使用Experience Platform Launch和Adobe I/O将Adobe Experience Manager与Adobe Target集成
feature: 体验片段
topic: 个性化
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1103'
ht-degree: 2%

---


# 通过Adobe Experience Platform Launch Console使用Adobe I/O

## 前提条件

* [AEM author和publish ](./implementation.md#set-up-aem) instancerunning分别位于localhost端口4502和4503上
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O控制台](https://console.adobe.io)

      >[!NOTE]
      >您应具有在Launch中开发、批准、发布、管理扩展和管理环境的权限。 如果由于用户界面选项不可用而无法完成这些步骤，请联系您的Experience Cloud管理员以请求访问权限。 有关“启动”权限的详细信息，请参阅文档](https://docs.adobelaunch.com/administration/user-permissions)。[


* **浏览器插件**
   * Adobe Experience Cloud调试器([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * 启动和DTM交换机([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 涉及的用户

对于此集成，需要参与以下受众，要执行某些任务，您可能需要管理访问权限。

* 开发人员
* AEM管理员
* Experience Cloud管理员

## 简介

AEM优惠与Experience Platform Launch的开箱即用集成。 此集成允许AEM管理员通过易用的界面轻松配置Experience Platform Launch，从而减少了配置这两个工具时的工作量和错误数量。 只需将Adobe Target扩展添加到Experience Platform Launch，就可以帮助我们使用AEM网页上Adobe Target的所有功能。

本节将介绍以下集成步骤：

* 启动
   * 创建启动项属性
   * 添加目标扩展
   * 创建数据元素
   * 创建页面规则
   * 设置环境
   * 构建和发布
* AEM
   * 创建Cloud Service
   * 创建

### 启动

#### 创建启动项属性

属性是一种容器，在将标签部署到站点时，您会使用扩展、规则、数据元素和库来填充。

1. 导航到您的组织[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. 使用Adobe ID登录，并确保您处于正确的组织。
3. 在解决方案切换器中，单击&#x200B;**启动**，然后选择&#x200B;**转到启动**&#x200B;按钮。

   ![Experience Cloud — 启动](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 确保您位于正确的组织中，然后继续创建启动项属性。
   ![Experience Cloud — 启动](assets/using-launch-adobe-io/launch-create-property.png)

   *有关创建属性的详细信息，请参 [阅产](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) 品文档中的创建属性。*
5. 单击&#x200B;**新建属性**&#x200B;按钮
6. 为您的属性提供名称(例如，*AEM 目标 Tutorial*)
7. 作为域，请输入&#x200B;*localhost.com*，因为这是运行WKND演示站点的域。 尽管“*域*”字段是必需的，但Launch属性将在实现它的任何域上工作。 此字段的主要用途是预填充规则生成器中的菜单选项。
8. 单击&#x200B;**保存**&#x200B;按钮。

   ![启动项 — 新属性](assets/using-launch-adobe-io/exc-launch-property.png)

9. 打开刚刚创建的属性，然后单击“扩展”选项卡。

#### 添加目标扩展

Adobe Target扩展支持使用目标 JavaScript SDK进行现代Web的客户端实现，`at.js`。 仍在使用目标旧库`mbox.js`、[的客户应升级到at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)以使用Launch。

该目标扩展由两个主要部分组成：

* 扩展配置，用于管理核心库设置
* 要执行以下操作的规则操作：
   * 加载目标(at.js)
   * 向所有Mbox添加参数
   * 将参数添加到全局Mbox
   * Fire Global Mbox

1. 在&#x200B;**Extensions**&#x200B;下，您可以看到已为Launch属性安装的扩展的列表。 (默认情况下安装了Experience Platform Launch核心扩展](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html))[
2. 单击&#x200B;**扩展目录**&#x200B;选项，并在筛选器中搜索目标。
3. 选择Adobe Target at.js的最新版本，然后单击&#x200B;**安装**选项。
   ![启动 — 新建属性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 单击&#x200B;**配置**按钮，您会注意到配置窗口中已导入目标帐户凭据，以及此扩展的at.js版本。
   ![目标 — 扩展配置](assets/using-launch-adobe-io/launch-target-extension-2.png)

   当通过异步启动嵌入代码部署目标时，您应该在启动嵌入代码之前将预隐藏的片段硬编码在页面上，以便管理内容闪烁。 我们稍后会进一步了解预藏狙击手。 您可以下载预隐藏的片段[此处](assets/using-launch-adobe-io/prehiding.js)

5. 单击&#x200B;**保存**&#x200B;以完成将目标扩展添加到您的Launch属性，您现在应能看到在&#x200B;**Installed**&#x200B;扩展列表下列出的目标扩展。

6. 重复上述步骤以搜索“Experience CloudID服务”扩展并安装它。
   ![扩展 — Experience CloudID服务](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 设置环境

1. 单击站点属性的&#x200B;**环境**&#x200B;选项卡，您可以看到为站点属性创建的环境的列表。 默认情况下，我们为开发、升级和生产分别创建了一个实例。

![数据元素 — 页面名称](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 构建和发布

1. 单击站点属性的&#x200B;**发布**&#x200B;选项卡，让我们创建一个库来构建更改，并将更改（数据元素、规则）部署到开发环境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 将您的更改从开发发布到暂存环境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 运行&#x200B;**为暂存构建选项**。
4. 生成完成后，运行&#x200B;**批准发布**，将您的更改从暂存环境移动到生产环境。
   ![暂存到生产](assets/using-launch-adobe-io/build-staging.png)
5. 最后，运行&#x200B;**构建并发布到生产**选项，将更改推送到生产。
   ![构建并发布到生产](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予Adobe I/O集成对具有相应[角色的选定工作区的访问权限，以允许中央团队仅在几个工作区](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)中进行API驱动的更改。

1. 在AEM中使用来自Adobe I/O的凭据创建IMS集成。（01:12到03:55）
2. 在Experience Platform Launch中，创建属性。 （覆盖于[以上](#create-launch-property)）
3. 使用步骤1中的IMS集成，创建Experience Platform Launch集成以导入您的Launch属性。
4. 在AEM中，使用浏览器配置将Experience Platform Launch集成映射到站点。 （05:28至06:14）
5. 手动验证集成。 （06:15至06:33）
6. 使用Launch/DTM浏览器插件。 （06:34至06:50）
7. 使用Adobe Experience Cloud Debugger浏览器插件。 （06:51至07:22）

目前，您已使用Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options)成功将[AEM与Adobe Target集成，详见选项1。

为了使用AEM Experience Fragment优惠支持您的个性化活动，让我们继续阅读下一章，并使用旧版云服务将AEM与Adobe Target集成。
