---
title: 作为Cloud Service的AEM本地开发环境
description: Adobe Experience Manager(AEM)本地开发环境概述。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: 开发
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 2%

---


# 本地开发环境设置

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概述"
>abstract="将AEM的本地开发环境设置为Cloud Service包括开发、构建和编译AEM项目所需的开发工具，以及本地运行时，使开发人员能在通过Adobe Cloud Manager将新功能部署到AEM作为Cloud Service之前快速在本地验证新功能。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="开发指南"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="开发基础"

本教程将指导您使用AEM作为Cloud Service SDK为Adobe Experience Manager(AEM)设置本地开发环境。 其中包括开发、构建和编译AEM项目所需的开发工具，以及本地运行时间，使开发人员能在通过Adobe Cloud Manager将新功能作为Cloud Service部署到AEM之前，在本地快速验证新功能。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM作为Cloud Service本地开发环境技术堆栈](./assets/overview/aem-sdk-technology-stack.png)

AEM的本地开发环境可分为三个逻辑组：

+ __AEM Project__&#x200B;包含自定义AEM应用程序的自定义代码、配置和内容。
+ __本地AEM运行时__，它在本地运行AEM作者服务和发布服务的本地版本。
+ 运行Apache HTTP Web Server和Dispatcher本地版本的&#x200B;__本地调度程序运行时__。

本教程将介绍如何安装和设置上图中突出显示的项目，为AEM开发提供稳定的本地开发环境。

## 文件系统组织

本教程将AEM的位置确定为Cloud Service SDK项目和AEM项目代码，如下所示：

+ `~/aem-sdk` 是包含AEM作为Cloud Service SDK提供的各种工具的组织文件夹
+ `~/aem-sdk/author` 包含AEM作者服务
+ `~/aem-sdk/publish` 包含AEM发布服务
+ `~/aem-sdk/dispatcher` 包含Dispatcher Tools
+ `~/code/<project name>` 包含自定义AEM Project源代码

请注意，`~`是用户目录的速记。 在Windows中，这等效于`%HOMEPATH%`;

## AEM项目开发工具

AEM项目是包含通过Cloud Manager部署到AEM作为Cloud Service的代码、配置和内容的自定义代码库。 基线项目结构通过[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)生成。

本教程的本节说明如何：

+ 安装 [!DNL Java]
+ 安装[!DNL Node.js]（和npm）
+ 安装 [!DNL Maven]
+ 安装 [!DNL Git]

[设置AEM项目的开发工具](./development-tools.md)

## 本地AEM Runtime

AEM作为Cloud Service SDK提供了运行AEM本地版本的[!DNL QuickStart Jar]。 [!DNL QuickStart Jar]可用于在本地运行AEM作者服务或AEM发布服务。 请注意，尽管[!DNL QuickStart Jar]提供本地开发体验，但并非AEM中作为Cloud Service提供的所有功能都包含在[!DNL QuickStart Jar]中。

本教程的本节说明如何：

+ 安装 [!DNL Java]
+ 下载AEM SDK
+ 运行[!DNL AEM Author Service]
+ 运行[!DNL AEM Publish Service]

[设置本地AEM运行时](./aem-runtime.md)

## 本地[!DNL Dispatcher]运行时

AEM作为Cloud Service SDK的Dispatcher Tools提供了设置本地[!DNL Dispatcher]运行时所需的一切。 [!DNL Dispatcher] 工具基于 [!DNL Docker]并提供命令行工具，可将Web服 [!DNL Apache HTTP] 务器和配 [!DNL Dispatcher] 置文件传输为兼容格式，并将它们部 [!DNL Dispatcher] 署到容器中 [!DNL Docker] 运行。

本教程的本节说明如何：

+ 下载AEM SDK
+ 安装[!DNL Dispatcher]工具
+ 运行本地[!DNL Dispatcher]运行时

[设置LocalRuntime [!DNL Dispatcher] ](./dispatcher-tools.md)
