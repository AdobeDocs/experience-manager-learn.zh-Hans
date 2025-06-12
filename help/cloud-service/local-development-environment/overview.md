---
title: AEM as a Cloud Service 的本地开发环境
description: Adobe Experience Manager (AEM) 本地开发环境概述。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '530'
ht-degree: 100%

---

# 设置本地开发环境 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概述"
>abstract="为 AEM as a Cloud Service 设置本地开发环境包括开发、构建和编译 AEM 项目所需的开发工具，以及允许开发人员在通过 Adobe Cloud Manager 将新功能部署到 AEM as a Cloud Service 之前在本地快速验证这些新功能的本地运行时。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=zh-Hans" text="开发准则"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hans" text="开发基础"

本教程将指导您使用 AEM as a Cloud Service SDK 为 Adobe Experience Manager (AEM) 设置本地开发环境。其中包括开发、构建和编译 AEM 项目所需的开发工具，以及本地运行时环境，使开发人员能够在通过 Adobe Cloud Manager 将新功能部署到 AEM as a Cloud Service 之前，在本地快速验证这些功能。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service 本地开发环境技术堆栈](./assets/overview/aem-sdk-technology-stack.png)

AEM 的本地开发环境可分为三个逻辑组：

+ __AEM 项目__&#x200B;包含自定义代码、配置和内容，即自定义 AEM 应用程序。
+ __本地 AEM 运行时__&#x200B;在本地运行 AEM Author 和 Publish 服务的本地版本。
+ __本地 Dispatcher 运行时__&#x200B;运行本地版本的 Apache HTTP Web 服务器和 Dispatcher。

本教程将逐步介绍如何安装和设置上图中突出显示的项目，以为 AEM 开发提供稳定的本地开发环境。

## 文件系统组织

本教程确定了 AEM as a Cloud Service SDK 工件和 AEM 项目代码的位置，具体如下：

+ `~/aem-sdk` 是一个组织文件夹，其中包含由 AEM as a Cloud Service SDK 提供的各种工具
+ `~/aem-sdk/author` 包含 AEM Author 服务
+ `~/aem-sdk/publish` 包含 AEM Publish 服务
+ `~/aem-sdk/dispatcher` 包含 Dispatcher 工具
+ `~/code/<project name>` 包含自定义 AEM 项目源代码

请注意， `~` 是用户目录的简写。在 Windows 中，这相当于 `%HOMEPATH%`；

## AEM 项目的开发工具

AEM 项目是一个包含代码、配置和内容的自定义代码库，这些内容通过 Cloud Manager 部署到 AEM as a Cloud Service。基线项目结构是通过 [AEM 项目 Maven 原型](https://github.com/adobe/aem-project-archetype)生成的。

本教程的这一部分将介绍如何：

+ 安装 [!DNL Java]
+ 安装 [!DNL Node.js]（和 npm）
+ 安装 [!DNL Maven]
+ 安装 [!DNL Git]

[为 AEM 项目设置开发工具](./development-tools.md)

## 本地 AEM 运行时

AEM as a Cloud Service SDK 提供了一个 [!DNL QuickStart Jar]，可运行 AEM 的本地版本。[!DNL QuickStart Jar] 可用于在本地运行 AEM Author 服务或 AEM Publish 服务。请注意，虽然 [!DNL QuickStart Jar] 提供了本地开发体验，但并非 AEM as a Cloud Service 中的所有功能都包含在 [!DNL QuickStart Jar] 中。

本教程的这一部分将介绍如何：

+ 安装 [!DNL Java]
+ 下载 AEM SDK
+ 运行 [!DNL AEM Author Service]
+ 运行 [!DNL AEM Publish Service]

[设置本地 AEM 运行时](./aem-runtime.md)

## 本地 [!DNL Dispatcher] 运行时

AEM as a Cloud Service SDK 的 Dispatcher 工具提供了设置本地 [!DNL Dispatcher] 运行时所需的一切。[!DNL Dispatcher] 工具基于 [!DNL Docker]，并提供命令行工具，用于将 [!DNL Apache HTTP] Web 服务器和 [!DNL Dispatcher] 配置文件转换为兼容格式，并将其部署到 [!DNL Docker] 容器中运行的 [!DNL Dispatcher]。

本教程的这一部分将介绍如何：

+ 下载 AEM SDK
+ 安装 [!DNL Dispatcher] 工具
+ 运行本地 [!DNL Dispatcher] 运行时

[设置本地  [!DNL Dispatcher]  运行时](./dispatcher-tools.md)
