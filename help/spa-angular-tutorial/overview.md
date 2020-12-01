---
title: AEM SPA Editor 和 Angular 快速入门
description: 创建您的第一个角度单页应用程序(SPA)，它可在Adobe Experience Manager、AEM和WKND SPA中进行编辑。 了解如何使用具有SPA  AEM编辑器的Angular JS框架创建SPA。 本教程由多部分组成，其中将介绍虚拟生活品牌WKND的Angular应用程序的实施。 本教程介绍SPA的端到端创建以及与AEM的集成。
sub-product: 站点
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 7%

---


# 在AEM {#introduction}中创建您的第一个角SPA

欢迎学习为Adobe Experience Manager(AEM)**SPA Editor**&#x200B;功能新手的开发人员设计的多部分教程。 本教程将介绍如何为虚构的生活方式品牌WKND实施Angular应用程序。 Angular应用程序将开发并设计为与AEM SPA编辑器一起部署，后者将Angular组件映射到AEM组件。 已部署到AEM的完整SPA可以使用AEM的传统在线编辑工具动态地进行创作。

![最终实施SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

此多部分教程的目标是教给开发人员如何实施Angular应用程序以与AEM的SPA Editor功能配合使用。 在现实场景中，开发活动按角色细分，通常涉及&#x200B;**前端开发者**&#x200B;和&#x200B;**后端开发者**。 我们认为，对于任何将参与AEM SPA Editor项目的开发人员来完成本教程都是有益的。

本教程设计为将&#x200B;**AEM用作Cloud Service**，并向后兼容&#x200B;**AEM 6.5.4+**&#x200B;和&#x200B;**AEM 6.4.8+**。 SPA的实现方式：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA编辑器](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [角度](https://angular.io/)

*估计1-2小时即可完成教程的每个部分。*

## 最新代码

所有教程代码都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到。

[最新代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)可作为可下载的AEM软件包提供。

## 前提条件

在开始本教程之前，您需要：

* HTML、CSS和JavaScript的基本知识
* 对[Angular](https://angular.io/)的基本熟悉
* [AEM作为Cloud ServiceSDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65)  6.5.4+ [或AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*虽然不需要，但对开发传统AEM Sites部件有 [基本的了解](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 本地开发环境{#local-dev-environment}

必须具备本地开发环境才能完成本教程。 截屏和视频使用AEM作为Cloud ServiceSDK捕获，该环境在Mac OS上运行，IDE为[Visual Studio Code](https://code.visualstudio.com/)。 除非另有说明，否则命令和代码应独立于本地操作系统。

>[!NOTE]
>
> **刚从AEM当Cloud Service?** 查看以 [下指南，使用AEM作为环境SDK设置本地开发Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **初次使用AEM 6.5?** 请参阅以 [下指南以设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 后续步骤{#next-steps}

你在等什么！开始教程，方法是导航到[SPA Editor Project](create-project.md)一章，并学习如何使用AEM Project Archetype生成启用SPA的项目。

## 向后兼容性{#compatibility}

本教程的项目代码是作为Cloud Service为AEM构建的。 为了使项目代码向后兼容&#x200B;**6.5.4+**&#x200B;和&#x200B;**6.4.8+**，已经做了若干修改。

[UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4**&#x200B;已作为依赖项包含在内：

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

已添加一个名为`classic`的额外Maven用户档案，以将内部版本修改为目标AEM 6.x环境:

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

默认情况下，`classic`用户档案处于禁用状态。 如果按照教程与AEM 6.x一起学习，请每当指示执行Maven内部版本时，添加`classic`用户档案:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

为AEM实施生成新项目时，始终使用[AEM项目Archetype](https://github.com/adobe/aem-project-archetype)的最新版本，并更新`aemVersion`以目标您的预期AEM版本。
