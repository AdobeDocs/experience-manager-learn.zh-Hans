---
title: 作为Cloud Service的AEM本地开发环境
description: Adobe Experience Manager(AEM)当地发展环境概述。
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---


# 本地开发环境设置

本教程将逐步介绍如何使用AEM作为Cloud ServiceSDK为Adobe Experience Manager(AEM)设置本地开发环境。 其中包括开发、构建和编译AEM项目所需的开发工具，以及本地运行时间，使开发人员能在本地快速验证新功能，然后再通过Adobe云管理器将新功能部署到AEM作为Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM作为Cloud Service本地开发环境技术堆栈](./assets/overview/aem-sdk-technology-stack.png)

AEM的本地开发环境可分为三个逻辑组：

+ AEM __Project__ 包含自定义的代码、配置和内容，这些是自定义的AEM应用程序。
+ 本 __地AEM__ 运行时，它在本地运行AEM作者服务和发布服务的本地版本。
+ 运 __行Apache__ HTTP Web Server和Dispatcher的本地版本的本地调度程序运行时。

本教程将介绍如何安装和设置上图中突出显示的项目，为AEM开发提供稳定的本地开发环境。

## 文件系统组织

本教程将AEM的位置确立为Cloud ServiceSDK对象和AEM项目代码，如下所示：

+ `~/aem-sdk` 是包含AEM作为Cloud ServiceSDK提供的各种工具的组织文件夹
+ `~/aem-sdk/author` 包含AEM作者服务
+ `~/aem-sdk/publish` 包含AEM发布服务
+ `~/aem-sdk/dispatcher` 包含调度程序工具
+ `~/code/<project name>` 包含自定义AEM项目源代码

请注 `~` 意，用户目录的简写。 在Windows中，这相当于 `%HOMEPATH%`;

## AEM项目开发工具

AEM项目是包含通过云管理器部署到AEM作为Cloud Service的代码、配置和内容的自定义代码库。 基线项目结构通过AEM Project [Maven Archetype生成](https://github.com/adobe/aem-project-archetype)。

本教程的本节将介绍如何：

+ 安装 [!DNL Java]
+ 安 [!DNL Node.js] 装（和npm）
+ 安装 [!DNL Maven]
+ 安装 [!DNL Git]

[为AEM Projects设置开发工具](./development-tools.md)

## 本地AEM运行时

AEM作为Cloud ServiceSDK提供 [!DNL QuickStart Jar] 了运行AEM的本地版本。 可 [!DNL QuickStart Jar] 用于在本地运行AEM作者服务或AEM发布服务。 请注意，尽管提供 [!DNL QuickStart Jar] 了本地开发体验，但AEM作为Cloud Service提供的所有功能并不都包含在中 [!DNL QuickStart Jar]。

本教程的本节将介绍如何：

+ 安装 [!DNL Java]
+ 下载AEM SDK
+ 运行 [!DNL AEM Author Service]
+ 运行 [!DNL AEM Publish Service]

[设置本地AEM运行时](./aem-runtime.md)

## 本地运 [!DNL Dispatcher] 行时

AEM作为Cloud ServiceSDK的调度程序工具提供设置本地运行时所需的一 [!DNL Dispatcher] 切。 [!DNL Dispatcher] 工具基 [!DNL Docker]于并提供命令行工具，可将Web服 [!DNL Apache HTTP] 务器和配置文 [!DNL Dispatcher] 件传输为兼容格式并将它们部署到在 [!DNL Dispatcher] 容器中 [!DNL Docker] 运行。

本教程的本节将介绍如何：

+ 下载AEM SDK
+ 安装工 [!DNL Dispatcher] 具
+ 运行本地运 [!DNL Dispatcher] 行时

[设置LocalRuntime [!DNL Dispatcher] ](./dispatcher-tools.md)
