---
title: AEM as a Cloud Service的本地开发环境
description: Adobe Experience Manager (AEM)本地开发环境概述。
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 12%

---

# 设置本地开发环境 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概述"
>abstract="为 AEM as a Cloud Service 设置本地开发环境包括开发、构建和编译 AEM 项目所需的开发工具，以及允许开发人员在通过 Adobe Cloud Manager 将新功能部署到 AEM as a Cloud Service 之前在本地快速验证这些新功能的本地运行时。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=zh-Hans" text="开发准则"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hans" text="开发基础"

本教程介绍如何使用AEM as a Cloud Service SDK为Adobe Experience Manager (AEM)设置本地开发环境。 其中包括开发、构建和编译AEM项目所需的开发工具，以及允许开发人员在通过AdobeCloud Manager将新功能部署到AEM as a Cloud Service之前，在本地快速验证这些功能的本地运行时间。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service本地开发环境技术栈栈](./assets/overview/aem-sdk-technology-stack.png)

AEM的本地开发环境可以划分为三个逻辑组：

+ __AEM项目__&#x200B;包含自定义代码、配置和自定义AEM应用程序内容。
+ 本地运行AEM Author和Publish服务的本地版本的&#x200B;__本地AEM运行时__。
+ __本地Dispatcher运行时__，它运行Apache HTTP Web Server和Dispatcher的本地版本。

本教程将介绍如何安装和设置上图中突出显示的项目，为AEM开发提供稳定的本地开发环境。

## 文件系统组织

本教程建立了AEM as a Cloud Service SDK工件和AEM项目代码的位置，如下所示：

+ `~/aem-sdk`是一个组织文件夹，其中包含由AEM as a Cloud Service SDK提供的各种工具
+ `~/aem-sdk/author`包含AEM作者服务
+ `~/aem-sdk/publish`包含AEM Publish服务
+ `~/aem-sdk/dispatcher`包含Dispatcher工具
+ `~/code/<project name>`包含自定义AEM项目源代码

请注意，`~`是用户目录的简写。 在Windows中，这等同于`%HOMEPATH%`；

## AEM项目开发工具

AEM项目是一个自定义代码库，其中包含通过Cloud Manager部署到AEM as a Cloud Service的代码、配置和内容。 基线项目结构是通过[AEM项目Maven原型](https://github.com/adobe/aem-project-archetype)生成的。

本教程的此部分将演示如何：

+ 安装[!DNL Java]
+ 安装[!DNL Node.js]（和npm）
+ 安装[!DNL Maven]
+ 安装[!DNL Git]

[为AEM项目设置开发工具](./development-tools.md)

## 本地 AEM 运行时

AEM as a Cloud Service SDK提供了一个运行本地版本的AEM的[!DNL QuickStart Jar]。 [!DNL QuickStart Jar]可用于在本地运行AEM Author Service或AEM Publish Service。 请注意，虽然[!DNL QuickStart Jar]提供了本地开发体验，但并非所有AEM as a Cloud Service中可用的功能都包含在[!DNL QuickStart Jar]中。

本教程的此部分将演示如何：

+ 安装[!DNL Java]
+ 下载AEM SDK
+ 运行[!DNL AEM Author Service]
+ 运行[!DNL AEM Publish Service]

[设置本地AEM运行时](./aem-runtime.md)

## 本地[!DNL Dispatcher]运行时

AEM as a Cloud Service SDK的Dispatcher Tools提供了设置本地[!DNL Dispatcher]运行时所需的一切。 [!DNL Dispatcher]工具基于[!DNL Docker]，并提供命令行工具以将[!DNL Apache HTTP] Web服务器和[!DNL Dispatcher]配置文件转换为兼容格式并将它们部署到[!DNL Docker]容器中运行的[!DNL Dispatcher]。

本教程的此部分将演示如何：

+ 下载AEM SDK
+ 安装[!DNL Dispatcher]工具
+ 运行本地[!DNL Dispatcher]运行时

[设置本地 [!DNL Dispatcher] 运行时](./dispatcher-tools.md)
