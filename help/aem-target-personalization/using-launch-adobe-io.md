---
title: 使用Experience Platform Launch和AdobeI/O将Adobe Experience Manager与Adobe Target集成
seo-title: 使用Experience Platform Launch和AdobeI/O将Adobe Experience Manager与Adobe Target集成
description: 分步演练如何使用Experience Platform Launch和AdobeI/O将Adobe Experience Manager与Adobe Target整合
seo-description: 分步演练如何使用Experience Platform Launch和AdobeI/O将Adobe Experience Manager与Adobe Target整合
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# 通过AdobeI/O控制台使用Adobe Experience Platform Launch

## 前提条件

* [AEM author和publish实例](./implementation.md#set-up-aem) （分别运行在localhost端口4502和4503上）
* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud <https://>`<yourcompany>`-.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [AdobeI/O控制台](https://console.adobe.io)

      >[!NOTE]
      >您应该有权在Launch中开发、批准、发布、管理扩展和管理环境。 如果由于用户界面选项不可用而无法完成这些步骤，请联系您的Experience Cloud管理员以请求访问权限。 有关启动权限的更多信息， [请参阅文档](https://docs.adobelaunch.com/administration/user-permissions)。


* **浏览器插件**
   * Adobe Experience Cloud调试器([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * 启动和DTM交换机([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 涉及的用户

对于此集成，需要参与以下受众，要执行某些任务，您可能需要管理访问权限。

* 开发人员
* AEM管理员
* Experience Cloud管理员

## 简介

AEM优惠与Experience Platform Launch的开箱即用集成。 此集成允许AEM管理员通过易于使用的界面轻松配置Experience Platform Launch，从而减少配置这两个工具时的工作量和错误数。 而且，只要将Adobe Target扩展添加到Experience Platform Launch，就可以帮助我们在AEM网页上使用Adobe Target的所有功能。

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

属性是在将标记部署到站点时用扩展、规则、数据元素和库填充的容器。

1. 导航到您的组织 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. 使用您的Adobe ID登录，并确保您处于正确的组织。
3. 从解决方案切换器中，单 **击启动** ，然后选择 **转到启动** 按钮。

   ![Experience Cloud-启动](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 确保您位于正确的组织中，然后继续创建启动项属性。
   ![Experience Cloud-启动](assets/using-launch-adobe-io/launch-create-property.png)

   *有关创建属性的详细信息，请[参阅产品文档中](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property)的创建属性。*
5. 单击“新建 **属性”按钮** 。
6. 为属性提供名称(例如，AEM *目标教程*)
7. 作为域，请输 *入localhost.com* ，因为这是运行WKND演示站点的域。 尽管“*域*”字段是必填字段，但Launch属性将在实现该域的任何域上工作。 此字段的主要用途是预填充规则生成器中的菜单选项。
8. 单击“保 **存** ”按钮。

   ![启动项——新属性](assets/using-launch-adobe-io/exc-launch-property.png)

9. 打开刚刚创建的属性，然后单击“扩展”选项卡。

#### 添加目标扩展

Adobe Target扩展支持使用目标JavaScript SDK进行现代Web的客户端实现 `at.js`。 仍在使用目标旧库的 `mbox.js`客 [户应升级到at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html) 以使用Launch。

目标扩展由两个主要部分组成：

* 扩展配置，用于管理核心库设置
* 用于执行以下操作的规则操作：
   * 加载目标(at.js)
   * 将参数添加到所有Mbox
   * 将参数添加到全局Mbox
   * Fire Global Mbox

1. 在“ **扩展**”下，您可以看到已为Launch属性安装的扩展的列表。 ([默认情况下](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) ,Experience Platform Launch核心扩展安装)
2. 单击“扩 **展目录** ”选项，并在筛选器中搜索目标。
3. 选择最新版本的Adobe Targetat.js并单击“安 **装** ”选项。
   ![启动——新属性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 单击“ **配置** ”按钮，您会注意到配置窗口中已导入目标帐户凭据，以及此扩展的at.js版本。
   ![目标-扩展配置](assets/using-launch-adobe-io/launch-target-extension-2.png)

   通过异步启动嵌入代码部署目标时，您应在启动嵌入代码之前，在页面上硬编码预隐藏的片段，以管理内容闪烁。 我们稍后会进一步了解预藏狙击手。 您可以在此处下载预隐藏的代 [码片段](assets/using-launch-adobe-io/prehiding.js)

5. 单击 **保存** ，以完成将目标扩展添加到您的Launch属性中，您现在应能看到已安装的扩展列表下列 **出的** 目标扩展。

6. 重复上述步骤以搜索“Experience CloudID服务”扩展并安装它。
   ![扩展-Experience CloudID服务](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 设置环境

1. 单击站 **点属性** 的环境选项卡，您可以看到为站点属性创建的环境的列表。 默认情况下，我们为开发、暂存和生产分别创建了一个实例。

![数据元素——页面名称](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 构建和发布

1. 单击站 **点属性** 的“发布”选项卡，让我们创建一个库，以构建更改并将其部署到开发环境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 将您的更改从开发发布到临时环境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 运行“ **生成暂存”选项**。
4. 构建完成后，运行“ **批准发布**”，将更改从暂存环境移到生产环境。
   ![暂存到生产](assets/using-launch-adobe-io/build-staging.png)
5. 最后，运行“ **构建并发布到生产** ”选项，将更改推送到生产。
   ![构建并发布到生产](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予AdobeI/O集成对具有相应角色的选定工作区的 [访问权限，以便让中央团队仅在几个工作区中进行API驱动的更改](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)。

1. 使用来自AdobeI/O的凭据在AEM中创建IMS集成（01:12至03:55）
2. 在Experience Platform Launch中，创建属性。 (上 [述](#create-launch-property))
3. 使用步骤1中的IMS集成创建Experience Platform Launch集成以导入您的Launch属性。
4. 在AEM中，使用浏览器配置将Experience Platform Launch集成映射到站点。 （05:28至06:14）
5. 手动验证集成。 （06:15至06:33）
6. 使用Launch/DTM浏览器插件。 （06:34至06:50）
7. 使用Adobe Experience Cloud调试器浏览器插件。 （06:51至07:22）

此时，您已使用Adobe Experience Platform Launch成功 [将AEM与Adobe Target集成](./using-aem-cloud-services.md#integrating-aem-target-options) ，详见选项1。

为了使用AEM Experience Fragment优惠支持您的个性化活动，让我们继续阅读下一章，并使用旧版云服务将AEM与Adobe Target集成。
