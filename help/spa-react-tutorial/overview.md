---
title: AEM SPA编辑器入门和反应
description: 创建您的首个React Single Page Application(SPA)，它可在Adobe Experience ManagerAEM和WKND SPA中编辑。 了解如何使用AEM SPA Editor的React JS框架创建SPA。 本教程由多部分组成，其中将逐步介绍虚拟生活方式品牌WKND的React应用程序的实施。 本教程介绍SPA的端到端创建以及与AEM的集成。
sub-product: 站点
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 5%

---


# 在AEM中创建您的第一个React SPA {#overview}

欢迎使用专为Adobe Experience Manager(AEM)SPA编辑器功能新 **手开发人** 员设计的多部分教程。 本教程将介绍如何为虚构的生活方式品牌WKND实施React应用程序。 React应用程序将开发并设计为与AEM SPA Editor一起部署，后者将React组件映射到AEM组件。 部署到AEM的完整SPA可以使用AEM的传统在线编辑工具动态地进行创作。

![已实施最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

此多部分教程旨在教授开发人员如何实施React应用程序以与AEM的SPA编辑器功能结合使用。 在现实场景中，开发活动按角色细分，通常涉及前端 **开发人员** 和后 **端开发人员**。 我们认为，对于任何将参与AEM SPA Editor项目的开发人员来完成本教程都是有益的。

本教程设计为将AEM **作为Cloud Service使** 用 **，并向后兼容** AEM 6.5.4+ ****&#x200B;和AEM 6.4.8+。 SPA是通过以下方式实现的：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [反应JS](https://reactjs.org/)
* [创建React App](https://create-react-app.dev/)

*估计1-2小时即可完成教程的每个部分。*

## 最新代码

所有教程代码都可以在GitHub上 [找到](https://github.com/adobe/aem-guides-wknd-spa)。

最新 [的代码库](https://github.com/adobe/aem-guides-wknd-spa/releases) ，可作为可下载的AEM软件包提供。

## 前提条件

在开始本教程之前，您需要：

* HTML、CSS和JavaScript的基本知识
* 基本熟悉 [React](https://reactjs.org/tutorial/tutorial.html)
* [AEM作为Cloud ServiceSDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 4+或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

*虽然不需要，但对开发传统AEM Sites部件有[基本的了解](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 本地开发环境 {#local-dev-environment}

必须具备本地开发环境才能完成本教程。 截屏和视频是使用AEM作为Cloud ServiceSDK捕获的，该SDK在Mac OS环境上运行， [Visual Studio](https://code.visualstudio.com/) Code作为IDE。 除非另有说明，否则命令和代码应独立于本地操作系统。

>[!NOTE]
>
> **刚从AEM当Cloud Service?** 查看以 [下指南，使用AEM作为环境SDK设置本地开发Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **初次使用AEM 6.5?** 请参阅以 [下指南以设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 后续步骤 {#next-steps}

你在等什么！开始本教程，方法是导 [航到“SPA编辑器项目](create-project.md) ”一章，并学习如何使用AEM Project Archetype生成启用SPA编辑器的项目。

## 向后兼容性 {#compatibility}

本教程的项目代码是作为Cloud Service为AEM构建的。 为了使项目代码向后兼 **容6.5.4+****和6.4.8+** ，对教程的POM文件进行了几处修改。

Uber [Jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) v **6.4.4已作为依赖项包含** :

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

已添加一个名为的Maven用户档案 `classic` ，以将内部版本修改为目标AEM 6.x环境:

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

默 `classic` 认情况下用户档案处于禁用状态。 如果遵循AEM 6.x的教程，请每当被指 `classic` 示执行Maven内部版本时添加用户档案，即：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

为AEM实施生成新项目时，始终使用最新版 [AEM Project](https://github.com/adobe/aem-project-archetype) Archetype并将 `aemVersion` 其更新为目标您的预定版AEM。
