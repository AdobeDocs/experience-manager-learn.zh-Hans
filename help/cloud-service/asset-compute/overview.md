---
title: AEM as a Cloud Service 的 Asset Compute 微服务可扩展性
description: 本教程将逐步介绍如何创建一个简单的 Asset Compute 工作程序，该程序通过将原始资产裁剪成圆形，并应用可配置的对比度和亮度，来创建资产演绎版。虽然该工作程序本身较为基础，但本教程将使用它来探索如何创建、开发和部署自定义的 Asset Compute 工作程序，以便与 AEM as a Cloud Service 一起使用。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '986'
ht-degree: 100%

---

# Asset Compute 微服务可扩展性

AEM 云服务的 Asset Compute 微服务支持开发和部署自定义工作程序，用于读取和操作存储在 AEM 中的资产的二进制数据，最常用的操作是创建自定义资产演绎版。

在 AEM 6.x 中，使用自定义 AEM 工作流流程来读取、转换和写回资产演绎版，而在 AEM as a Cloud Service 中，Asset Compute 工作程序可满足这一需求。

## 您会做什么

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程将逐步介绍如何创建一个简单的 Asset Compute 工作程序，该程序通过将原始资产裁剪成圆形，并应用可配置的对比度和亮度，来创建资产演绎版。虽然该工作程序本身较为基础，但本教程将使用它来探索如何创建、开发和部署自定义的 Asset Compute 工作程序，以便与 AEM as a Cloud Service 一起使用。

### 目标 {#objective}

1. 提供并设置必要的帐户和服务，以构建和部署 Asset Compute 工作程序
1. 创建并配置 Asset Compute 项目
1. 开发一个可生成自定义演绎版的 Asset Compute 工作程序
1. 为自定义的 Asset Compute 工作程序创作测试，并学习如何对其进行调试
1. 部署 Asset Compute 工作程序，并通过处理轮廓将其集成到 AEM 作为云服务作者服务

## 设置

了解如何为扩展 Asset Compute 工作程序做好充分准备，并明确哪些服务和账户需要预先配置，以及为了开发需要在本地安装哪些软件。

### 帐户和服务配置{#accounts-and-services}

为了完成本教程，需要配置以下账户和服务并获取访问权限：AEM as a Cloud Service 开发环境或沙盒程序、App Builder 访问权限以及 Microsoft Azure Blob 存储。

+ [提供帐户和服务](./set-up/accounts-and-services.md)

### 本地开发环境

本地开发 Asset Compute 项目需要一套特定的开发人员工具集，这与传统的 AEM 开发有所不同，具体包括：Microsoft Visual Studio Code、Docker Desktop、Node.js 以及配套的 npm 模块。

+ [设置本地开发环境](./set-up/development-environment.md)

### App Builder

Asset Compute 项目是专门定义的 App Builder 项目，因此，需要访问 Adobe Developer Console 中的 App Builder 才能进行设置和部署。

+ [设置 App Builder](./set-up/app-builder.md)

## 开发

学习如何创建和配置 Asset Compute 项目，然后开发一个自定义工作程序，用于生成定制的资产演绎版。

### 创建新的 Asset Compute 项目

包含一个或多个 Asset Compute 工作程序的 Asset Compute 项目，是使用交互式 Adobe I/O CLI 生成的。Asset Compute 项目是经过特殊构建的 App Builder 项目，而这些项目又是 Node.js 项目。

+ [创建新的 Asset Compute 项目](./develop/project.md)

### 配置环境变量

环境变量在 `.env` 文件中进行维护以用于本地开发，并用于提供本地开发所需的 Adobe I/O 凭据和云存储凭据。

+ [配置环境变量](./develop/environment-variables.md)

### 配置 manifest.yml

Asset Compute 项目包含清单，该清单定义了项目中包含的所有 Asset Compute 工作程序，以及在部署到 Adobe I/O Runtime 进行执行时它们可用的资源。

+ [配置 manifest.yml](./develop/manifest.md)

### 开发一个工作程序

开发 Asset Compute 工作程序是扩展 Asset Compute 微服务的核心，因为该工作程序包含生成或编排生成的结果资产演绎版的自定义代码。

+ [开发 Asset Compute 工作程序](./develop/worker.md)

### 使用 Asset Compute 开发工具

Asset Compute 开发工具提供了一个本地 Web 环境，用于部署、执行和预览工作程序生成的演绎版，支持快速且可迭代的 Asset Compute 工作程序开发。

+ [使用 Asset Compute 开发工具](./develop/development-tool.md)

## 测试和调试

学习如何测试自定义的 Asset Compute 工作程序，以确保其正常运行，并调试 Asset Compute 工作程序，以了解自定义代码的执行方式并排除故障。

### 测试工作程序

Asset Compute 提供了一个测试框架，可用于为工作程序创建测试套件，从而轻松定义可确保正确行为的测试。

+ [测试工作程序](./test-debug/test.md)

### 调试工作程序

Asset Compute 工作程序提供从传统 `console.log(..)` 输出到与 __VS Code__ 和 __wskdebug__ 集成的各种调试级别，允许开发人员在实时执行工作程序代码时逐步调试。

+ [调试工作程序](./test-debug/debug.md)

## 部署

了解如何将自定义的 Asset Compute 工作程序与 AEM as a Cloud Service 进行集成，具体步骤是首先将它们部署到 Adobe I/O Runtime，然后通过 AEM Assets 的处理轮廓从 AEM as a Cloud Service Author 进行调用。

### 部署到 Adobe I/O Runtime

Asset Compute 工作程序必须部署到 Adobe I/O Runtime，才能与 AEM as a Cloud Service 一起使用。

+ [使用处理轮廓](./deploy/runtime.md)

### 通过 AEM 处理轮廓集成工作程序

一旦部署到 Adobe I/O Runtime，Asset Compute 工作程序即可通过[资产处理轮廓](../../assets/configuring/processing-profiles.md)在 AEM as a Cloud Service 中注册。处理轮廓又会应用于资产文件夹，进而应用于其中的资产。

+ [与 AEM 处理轮廓集成](./deploy/processing-profiles.md)

## 高级

这些精简教程基于前几章所建立的基础知识，进一步探讨更高级的应用案例。

+ [开发一个 Asset Compute 元数据工作程序](./advanced/metadata.md)，该工作程序可以将元数据写回

## Github 上的代码库

本教程的代码库可在 Github 上找到：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ 主分支

源代码不包含所需的 `.env` 或 `config.json` 文件。这些必须使用您的[账户和服务](#accounts-and-services)信息进行添加和配置。

## 其他资源

以下是各种 Adobe 资源，它们为开发 Asset Compute 工作程序提供了更多信息和有用的 API 和 SDK。

### 文档

+ [Asset Compute 服务文档](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [Asset Compute 开发工具自述文件](https://github.com/adobe/asset-compute-devtool)
+ [Asset Compute 示例工作程序](https://github.com/adobe/asset-compute-example-workers)

### API 和 SDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper 库](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe Node Fetch Retry 库](https://github.com/adobe/node-fetch-retry)
+ [Asset Compute 示例工作程序](https://github.com/adobe/asset-compute-example-workers)
