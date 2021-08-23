---
title: AEM SPA Editor 和 Angular 快速入门
description: 创建您的首个Angular单页应用程序(SPA)，该应用程序可在Adobe Experience Manager和AEM中使用WKND SPA进行编辑。 了解如何在AEM SPA编辑器中使用AngularJS框架创建SPA。 本多部分教程将演示如何实施Angular应用程序，以打造虚构的生活方式品牌WKND。 本教程涵盖SPA的端到端创建以及与AEM的集成。
sub-product: 站点
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA编辑器
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 3%

---


# 在AEM中创建您的第一个AngularSPA {#introduction}

欢迎使用多部分教程，该教程专为不熟悉Adobe Experience Manager(AEM)中&#x200B;**SPA Editor**&#x200B;功能的开发人员设计。 本教程将指导您实施一个Angular应用程序，以打造一个虚构的生活方式品牌WKND。 将开发并设计该Angular应用程序以与AEM SPA Editor一起部署，后者将Angular组件映射到AEM组件。 部署到AEM的已完成SPA可以使用AEM的传统在线编辑工具进行动态创作。

![已实施最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

此多部分教程的目标是教开发人员如何实施Angular应用程序以使用AEM的SPA编辑器功能。 在现实场景中，开发活动按角色划分，通常涉及&#x200B;**前端开发人员**&#x200B;和&#x200B;**后端开发人员**。 我们认为，对于任何将参与AEM SPA Editor项目的开发人员来说，完成本教程将会有所帮助。

本教程旨在将&#x200B;**AEM用作Cloud Service**，并且向后兼容&#x200B;**AEM 6.5.4+**&#x200B;和&#x200B;**AEM 6.4.8+**。 SPA的实施方式如下：

* [Maven AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [Angular](https://angular.io/)

*估计需要1-2小时才能完成教程的每个部分。*

## 最新代码

所有教程代码都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到。

[最新代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)可作为可下载的AEM包提供。

## 前提条件

在启动本教程之前，您需要满足以下条件：

* HTML、CSS和JavaScript的基本知识
* 对[Angular](https://angular.io/)的基本熟悉
* [AEM as a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、 [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*虽然不需要，但对开发传统的AEM Sites组件有基 [本的了解](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 本地开发环境 {#local-dev-environment}

要完成本教程，需要本地开发环境。 屏幕截图和视频是使用AEM作为Cloud ServiceSDK捕获的，该SDK在Mac OS环境中运行，并且IDE为[Visual Studio代码](https://code.visualstudio.com/)。 除非另有说明，否则命令和代码应独立于本地操作系统。

>[!NOTE]
>
> **初次使用AEM as aCloud Service?** 请参阅以 [下指南，以使用AEM as a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5的新增功能？** 请参阅以 [下指南以设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 后续步骤 {#next-steps}

你在等什么?!启动本教程的方法是导航到[SPA Editor项目](create-project.md)章节，并了解如何使用AEM项目原型生成启用SPA Editor的项目。

## 向后兼容性 {#compatibility}

本教程的项目代码是为AEM as a Cloud Service构建的。 为了使项目代码向后兼容&#x200B;**6.5.4+**&#x200B;和&#x200B;**6.4.8+**，做了若干修改。

[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4**&#x200B;已作为依赖项包含在内：

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

添加了名为`classic`的其他Maven配置文件，用于修改到目标AEM 6.x环境的内部版本：

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

默认情况下，`classic`配置文件处于禁用状态。 如果遵循AEM 6.x教程，请在指示您执行Maven内部版本时添加`classic`配置文件：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

为AEM实施生成新项目时，始终使用最新版本的[AEM项目原型](https://github.com/adobe/aem-project-archetype)并更新`aemVersion`以定位您的目标AEM版本。
