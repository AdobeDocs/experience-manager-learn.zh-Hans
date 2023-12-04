---
title: AEM SPA Editor 和 Angular 快速入门
description: 创建您的首个 Angular 单页面应用程序 (SPA)，该应用程序可在带 WKND SPA 的 Adobe Experience Manager (AEM) 中编辑。
version: Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 166
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 8%

---

# 在AEM中创建第一个AngularSPA {#introduction}

{{edge-delivery-services}}

欢迎使用专为新加入的开发人员设计的多部分教程 **SPA编辑器** Adobe Experience Manager (AEM)中的功能。 本教程介绍了虚拟生活风格AngularWKND的品牌应用程序的实施。 angular应用程序是使用AEM SPA Editor开发和设计的，该编辑器将Angular组件映射到AEM组件。 部署到AEM的已完成SPA可以使用传统的AEM内联编辑工具动态创作。

![已实施的最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

此多部分教程的目标是教开发人员如何实施Angular应用程序以使用AEM的SPA编辑器功能。 在现实世界中，开发活动按角色细分，通常涉及 **前端开发人员** 和 **后端开发人员**. 我们相信任何参与AEM SPA Editor项目的开发人员在完成本教程时都会受益。

本教程专门设计用于 **AEMas a Cloud Service** 并且向后兼容 **AEM 6.5.4+** 和 **AEM 6.4.8+**. 通过以下方式实施SPA：

* [Maven AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA编辑器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [angular](https://angular.io/)

*预计约需1-2小时完成教程的每个部分。*

## 最新代码

所有教程代码均可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

此 [最新的代码库](https://github.com/adobe/aem-guides-wknd-spa/releases) 作为可下载的AEM包提供。

## 前提条件

在开始本教程之前，您需要满足以下条件：

* HTML、CSS和JavaScript的基础知识
* 对的基本了解 [angular](https://angular.io/)
* [AEMAS A CLOUD SERVICESDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)， [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

*虽然没有必要，但基本了解以下内容会很有帮助 [开发传统AEM Sites组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans).*

## 本地开发环境 {#local-dev-environment}

需要本地开发环境来完成本教程。 使用在Mac OS环境中运行的AEMas a Cloud ServiceSDK捕获屏幕截图和视频，环境为 [Visual Studio代码](https://code.visualstudio.com/) 作为IDE。 除非另有说明，否则命令和代码应独立于本地操作系统。

>[!NOTE]
>
> **还不熟悉AEMas a Cloud Service？** 查看 [以下指南介绍了如何使用AEMas a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).
>
> **还不熟悉AEM 6.5？** 查看 [以下指南介绍了如何设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans).

## 后续步骤 {#next-steps}

你在等什么?!导航到 [SPA编辑器项目](create-project.md) 章中，并了解如何使用SPA项目原型生成启用AEM编辑器的项目。

## 向后兼容性 {#compatibility}

本教程的项目代码是针对AEMas a Cloud Service生成的。 为了使项目代码向后兼容 **6.5.4+** 和 **6.4.8+** 已进行了一些修改。

此 [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** 已作为依赖项纳入：

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

额外的Maven配置文件，名为 `classic` 已添加以修改内部版本并将其定位到AEM 6.x环境：

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

此 `classic` 配置文件默认处于禁用状态。 如果使用AEM 6.x完成教程，请添加 `classic` 每当收到指示要执行Maven构建时的配置文件：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

在为AEM实施生成新项目时，始终使用最新版本的 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 并更新 `aemVersion` 来定位您的AEM预期版本。
