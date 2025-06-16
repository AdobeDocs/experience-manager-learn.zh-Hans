---
title: AEM SPA Editor 和 Angular 快速入门
description: 创建您的首个 Angular 单页面应用程序 (SPA)，该应用程序可在带 WKND SPA 的 Adobe Experience Manager (AEM) 中编辑。
version: Experience Manager as a Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 100%

---

# 在 AEM 中创建您的第一个 Angular SPA {#introduction}

{{spa-editor-deprecation}}

欢迎来到本系列教程，本教程专为刚接触 Adobe Experience Manager (AEM) 中 **SPA 编辑器**&#x200B;功能的新手开发者设计。本教程将逐步介绍如何为虚构的生活方式品牌 WKND 实施一个 Angular 应用程序。该 Angular 应用程序的开发和设计旨在与 AEM 的 SPA 编辑器一起部署，该编辑器会将 Angular 组件映射到 AEM 组件。部署到 AEM 的完整 SPA 可以使用 AEM 的传统内联编辑工具进行动态创作。

![最终实施的 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 实施*

## 关于

本系列教程旨在教授开发人员如何实现一个与 AEM 的 SPA 编辑器功能配合使用的 Angular 应用程序。在实际场景中，开发活动会按角色进行分工，其通常涉及&#x200B;**前端开发者**&#x200B;和&#x200B;**后端开发者**。我们相信，任何参与 AEM SPA 编辑器项目的开发人员完成本教程后都将受益匪浅。

本教程旨在配合 **AEM as a Cloud Service** 使用，且向后兼容 **AEM 6.5.4+** 和 **AEM 6.4.8+**。SPA 的实现方式如下：

* [Maven AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hans)
* [AEM SPA 编辑器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=zh-Hans#content-editing-experience-with-spa)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [Angular](https://angular.io/)

*预计完成本教程的每个部分需要 1-2 小时。*

## 最新代码

所有教程代码均可在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa) 上找到。

[最新的代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)已作为可下载的 AEM 包提供。

## 先决条件

在开始本教程之前，您需要具备以下内容：

* HTML、CSS 和 JavaScript 的基础知识
* 对 [Angular](https://angular.io/) 有基本的了解
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hans#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/cn/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/cn/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 或更新版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

*虽然不是必需的，但对[开发传统 AEM Sites 组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)有基本的了解是有益的。*

## 本地开发环境 {#local-dev-environment}

完成本教程需要本地开发环境。屏幕截图和视频是使用 AEM as a Cloud Service SDK 在 Mac OS 环境下捕获的，其中 [Visual Studio Code](https://code.visualstudio.com/) 作为 IDE。除非另有说明，否则命令和代码应与本地操作系统无关。

>[!NOTE]
>
> **刚接触 AEM as a Cloud Service？**&#x200B;请参阅[以下使用 AEM as a Cloud Service SDK 搭建本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)。
>
> **刚接触 AEM 6.5？** 请参考[以下搭建本地开发环境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-hans)。

## 后续步骤 {#next-steps}

你还在等什么？！请导航至 [SPA 编辑器项目](create-project.md)章节，开始学习如何使用 AEM 项目原型生成支持 SPA 编辑器的项目。

## 向后兼容性 {#compatibility}

本教程的项目代码是为 AEM as a Cloud Service 而构建的。为了使项目代码向后兼容 **6.5.4+** 和 **6.4.8+**，已进行了若干修改。

已将 [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=zh-Hans#what-is-the-uberjar) **v6.4.4** 作为依赖项包含在内：

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

已添加名为 `classic` 的额外 Maven 轮廓，用于修改版本，以适应 AEM 6.x 环境：

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

默认情况下，`classic` 轮廓是禁用的。如果按照 AEM 6.x 的教程进行操作，请在需要执行 Maven 构建时添加 `classic` 轮廓：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

在为 AEM 实施生成新项目时，请始终使用最新版本的 [AEM 项目原型](https://github.com/adobe/aem-project-archetype)，并将 `aemVersion` 更新为您期望的 AEM 版本。
