---
title: AEM as a A Service的本地开发环境Cloud Service
description: Adobe Experience Manager(AEM)本地开发环境概述。
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 1%

---


# 本地开发环境设置

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概述"
>abstract="为AEM as aCloud Service设置本地开发环境包括开发、构建和编译AEM项目所需的开发工具，以及允许开发人员在本地运行时快速在本地验证新功能，然后再通过Adobe Cloud Manager将新功能部署到AEM as aCloud Service。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="开发准则"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="开发基础知识"

本教程将演示如何使用AEM as a Adobe Experience Manager SDK为Cloud Service(AEM)设置本地开发环境。 其中包括开发、构建和编译AEM项目所需的开发工具，以及本地运行时间，这些工具允许开发人员在本地快速验证新功能，然后再通过AdobeCloud Manager将新功能部署到AEM作为Cloud Service。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM as a Local开发环境技术堆栈](./assets/overview/aem-sdk-technology-stack.png)

AEM的本地开发环境可以划分为三个逻辑组：

+ __AEM Project__&#x200B;包含自定义AEM应用程序的自定义代码、配置和内容。
+ __本地AEM运行时__，它在本地运行AEM创作和发布服务的本地版本。
+ 运行Apache HTTP Web Server和Dispatcher本地版本的&#x200B;__本地Dispatcher运行时__。

本教程将指导如何安装和设置上图中突出显示的项目，从而为AEM开发提供稳定的本地开发环境。

## 文件系统组织

本教程已将AEM作为Cloud ServiceSDK对象和AEM项目代码的位置建立如下：

+ `~/aem-sdk` 是一个组织文件夹，其中包含AEM as a Cloud ServiceSDK提供的各种工具
+ `~/aem-sdk/author` 包含AEM创作服务
+ `~/aem-sdk/publish` 包含AEM发布服务
+ `~/aem-sdk/dispatcher` 包含Dispatcher工具
+ `~/code/<project name>` 包含自定义AEM项目源代码

请注意，`~`是用户目录的简写形式。 在Windows中，这等同于`%HOMEPATH%`;

## AEM项目开发工具

AEM项目是一个自定义代码库，其中包含通过Cloud Manager部署到AEM作为Cloud Service的代码、配置和内容。 基线项目结构通过[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)生成。

本教程的此部分将演示如何：

+ 安装 [!DNL Java]
+ 安装[!DNL Node.js]（和npm）
+ 安装 [!DNL Maven]
+ 安装 [!DNL Git]

[为AEM项目设置开发工具](./development-tools.md)

## 本地AEM运行时

AEM as a Cloud ServiceSDK提供了一个[!DNL QuickStart Jar]，用于运行AEM的本地版本。 [!DNL QuickStart Jar]可用于在本地运行AEM创作服务或AEM发布服务。 请注意，虽然[!DNL QuickStart Jar]提供了本地开发体验，但并非AEM as a Cloud Service中提供的所有功能都包含在[!DNL QuickStart Jar]中。

本教程的此部分将演示如何：

+ 安装 [!DNL Java]
+ 下载AEM SDK
+ 运行[!DNL AEM Author Service]
+ 运行[!DNL AEM Publish Service]

[设置本地AEM运行时](./aem-runtime.md)

## 本地[!DNL Dispatcher]运行时

AEM as a Cloud ServiceSDK的Dispatcher工具提供了设置本地[!DNL Dispatcher]运行时所需的一切功能。 [!DNL Dispatcher] 工具基于 [!DNL Docker]并提供了命令行工具，可将Web服 [!DNL Apache HTTP] 务器和配 [!DNL Dispatcher] 置文件传输为兼容格式，并将它们部署到容 [!DNL Dispatcher] 器中运 [!DNL Docker] 行。

本教程的此部分将演示如何：

+ 下载AEM SDK
+ 安装[!DNL Dispatcher]工具
+ 运行本地[!DNL Dispatcher]运行时

[设置Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)
