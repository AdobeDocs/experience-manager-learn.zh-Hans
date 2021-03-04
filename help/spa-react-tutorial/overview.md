---
title: AEM SPA Editor 和 React 快速入门
description: 创建您的第一个可在Adobe Experience Manager AEM中使用WKND SPA编辑的React Single Page Application(SPA)。 了解如何借助AEM SPA Editor使用React JS框架创建SPA。 本教程由多部分组成，其中将介绍虚拟生活方式品牌WKND的React应用程序的实施。 本教程介绍了SPA的端到端创建以及与AEM的集成。
sub-product: 站点
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 7%

---


# 在AEM {#overview}中创建您的第一个React SPA

欢迎使用为Adobe Experience Manager(AEM)中&#x200B;**SPA Editor**&#x200B;功能新手的开发人员设计的多部分教程。 本教程将介绍虚拟生活品牌WKND的React应用程序的实施。 React应用程序将开发并设计为与AEM SPA Editor一起部署，后者将React组件映射到AEM组件。 部署到AEM的完整SPA可以使用AEM的传统在线编辑工具动态创作。

![最终SPA已实施](assets/wknd-spa-implementation.png)

*WKND SPA实施*

## 关于

此多部分教程旨在教授开发人员如何实施React应用程序以与AEM的SPA编辑器功能结合使用。 在真实场景中，开发活动按角色划分，通常涉及&#x200B;**前端开发者**&#x200B;和&#x200B;**后端开发者**。 我们认为，对于将参与AEM SPA Editor项目的任何开发人员来说，完成本教程都是有益的。

本教程设计为将&#x200B;**AEM用作Cloud Service**，并向后兼容&#x200B;**AEM 6.5.4+**&#x200B;和&#x200B;**AEM 6.4.8+**。 SPA的实施方式：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)
* [React JS](https://reactjs.org/)
* [创建React应用程序](https://create-react-app.dev/)

*估计需要1-2个小时来完成教程的每个部分。*

## 最新代码

所有教程代码都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到。

[最新代码库](https://github.com/adobe/aem-guides-wknd-spa/releases)可作为可下载的AEM包提供。

## 前提条件

在开始本教程之前，您需要：

* HTML、CSS和JavaScript的基本知识
* 基本熟悉[反应](https://reactjs.org/tutorial/tutorial.html)
* [AEM作为Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、 [AEM 6.5.4](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) +或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更高版本）
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*虽然不需要，但对开发传统AEM Sites组件有 [基本了解是有益的](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 本地开发环境{#local-dev-environment}

完成本教程需要一个本地开发环境。 屏幕截图和视频是使用AEM作为Cloud ServiceSDK捕获的，Mac OS环境上运行，IDE为[Visual Studio代码](https://code.visualstudio.com/)。 除非另有说明，否则命令和代码应独立于本地操作系统。

>[!NOTE]
>
> **初次使用AEM作为Cloud Service?** 请参阅以 [下指南，使用AEM作为Cloud Service SDK设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **初次使用AEM 6.5?** 请参阅以 [下指南以设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 后续步骤{#next-steps}

你在等什么！开始教程，方法是导航到[SPA Editor Project](create-project.md)一章，并学习如何使用AEM Project Archetype生成启用了SPA Editor的项目。

## 向后兼容性{#compatibility}

本教程的项目代码是作为Cloud Service为AEM构建的。 为了使项目代码向后兼容&#x200B;**6.5.4+**&#x200B;和&#x200B;**6.4.8+**，对教程的POM文件进行了若干修改。

[UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4**&#x200B;已作为依赖项包含：

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

已添加一个名为`classic`的额外Maven用户档案，以将内部版本修改为目标 AEM 6.x环境:

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

默认情况下，`classic`用户档案处于禁用状态。 如果按照AEM 6.x的教程进行，请每当指示您执行Maven内部版本时，添加`classic`用户档案，即：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

为AEM实施生成新项目时，始终使用最新版的[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)并更新`aemVersion`以目标您的AEM预期版本。
