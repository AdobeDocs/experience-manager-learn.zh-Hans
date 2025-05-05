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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 8%

---

# 在AEM中创建您的第一个Angular SPA {#introduction}

{{edge-delivery-services}}

欢迎使用专为不熟悉Adobe Experience Manager (AEM)中的&#x200B;**SPA编辑器**&#x200B;功能的开发人员设计的多部分教程。 本教程介绍了虚拟生活方式品牌WKND的Angular应用程序的实施。 Angular应用程序是使用AEM的SPA Editor开发和设计的，该编辑器将Angular组件映射到AEM组件。 部署到AEM的已完成SPA可以使用AEM的传统内联编辑工具动态创作。

![已实施的最终SPA](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

此多部分教程的目标是教开发人员如何实施Angular应用程序以使用AEM的SPA编辑器功能。 在真实情景中，开发活动按角色细分，通常涉及&#x200B;**前端开发人员**&#x200B;和&#x200B;**后端开发人员**。 我们相信，任何参与AEM SPA Editor项目的开发人员填写本教程都将受益。

此教程设计为使用&#x200B;**AEM as a Cloud Service**，并且向后兼容&#x200B;**AEM 6.5.4+**&#x200B;和&#x200B;**AEM 6.4.8+**。 使用以下方式实施SPA：

* [Maven AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hans)
* [AEM SPA编辑器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=zh-Hans#content-editing-experience-with-spa)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hans)
* [Angular](https://angular.io/)

*预计需要1-2个小时来完成教程的每个部分。*

## 最新代码

可在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到所有教程代码。

[最新代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)可用作可下载的AEM包。

## 先决条件

在开始本教程之前，您需要满足以下条件：

* HTML、CSS和JavaScript的基础知识
* 对[Angular](https://angular.io/)有基本的了解
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hans#download-the-aem-as-a-cloud-service-sdk)、[AEM 6.5.4+](https://helpx.adobe.com/cn/experience-manager/aem-releases-updates.html#65)或[AEM 6.4.8+](https://helpx.adobe.com/cn/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.js](https://nodejs.org/en/)和[npm](https://www.npmjs.com/)

*虽然不是必需的，但对[开发传统AEM Sites组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)有基本的了解是有益的。*

## 本地开发环境 {#local-dev-environment}

需要本地开发环境来完成本教程。 使用在Mac OS环境中运行的AEM as a Cloud Service SDK捕获屏幕截图和视频，并将[Visual Studio Code](https://code.visualstudio.com/)用作IDE。 除非另有说明，否则命令和代码应独立于本地操作系统。

>[!NOTE]
>
> **是AEM as a Cloud Service的新用户？**&#x200B;请查看以下[指南，了解如何使用AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)设置本地开发环境。
>
> **是AEM 6.5的新手吗？**&#x200B;请查看以下[指南以设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-hans)。

## 后续步骤 {#next-steps}

你在等什么?!导航到[SPA Editor项目](create-project.md)章以启动该教程，并了解如何使用AEM项目原型生成启用SPA Editor的项目。

## 向后兼容性 {#compatibility}

本教程的项目代码是为AEM as a Cloud Service构建的。 为了使项目代码向后兼容&#x200B;**6.5.4+**&#x200B;和&#x200B;**6.4.8+**，已进行了若干修改。

[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=zh-Hans#what-is-the-uberjar) **v6.4.4**&#x200B;已作为依赖项包括在内：

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

已添加名为`classic`的其他Maven配置文件，以修改内部版本并将其定位到AEM 6.x环境：

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

默认情况下，`classic`配置文件处于禁用状态。 如果使用AEM 6.x完成教程，则在收到指示要执行Maven构建时，请添加`classic`配置文件：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

在为AEM实施生成新项目时，请始终使用最新版本的[AEM项目原型](https://github.com/adobe/aem-project-archetype)并更新`aemVersion`以定向您的AEM预期版本。
