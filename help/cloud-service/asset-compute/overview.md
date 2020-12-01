---
title: asset compute作为Cloud Service的AEM的微服务可扩展性
description: 本教程将逐步介绍如何创建一个简单的Asset compute工作者，该工作者通过将原始资产裁剪成圆形来创建资产演绎版，并应用可配置的对比度和亮度。 虽然工作人员本身是基本的，但本教程使用它来探索如何创建、开发和部署自定义Asset compute工作人员，以便与AEM一起用作Cloud Service。
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 0%

---


# asset compute微服务可扩展性

AEM作为Cloud Service的Asset computemicroservices支持开发和部署自定义工作器，这些工作器用于读取和操作存储在AEM（最常用）中的资产的二进制数据，以创建自定义资产再现。

而在AEM 6.x中，自定义AEM工作流流程用于读取、转换和回写资产演绎版，而在AEM中，它作为Cloud ServiceAsset compute工作器来满足这一需求。

## 您将做什么

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程将逐步介绍如何创建一个简单的Asset compute工作者，该工作者通过将原始资产裁剪成圆形来创建资产演绎版，并应用可配置的对比度和亮度。 虽然工作人员本身是基本的，但本教程使用它来探索如何创建、开发和部署自定义Asset compute工作人员，以便与AEM一起用作Cloud Service。

### 目标{#objective}

1. 配置和设置必要的帐户和服务以构建和部署Asset compute工作者
1. 创建和配置Asset compute项目
1. 开发Asset compute工作人员，以生成自定义再现
1. 为编写测试并学习如何调试自定义Asset compute工作器
1. 部署Asset compute工作者，并通过处理用户档案将其作为Cloud Service作者服务集成在一起

## 设置

了解如何正确准备扩展Asset compute工作者，并了解哪些服务和帐户必须配置和配置，以及本地安装的软件进行开发。

### 帐户和服务配置{#accounts-and-services}

以下帐户和服务需要设置和访问，以完成教程AEM(作为Cloud Service开发环境或沙箱项目)、访问Adobe项目Firefly和Microsoft Azure Blob存储。

+ [提供帐户和服务](./set-up/accounts-and-services.md)

### 本地开发环境

本地开发Asset compute项目需要一个与传统AEM开发不同的特定开发人员工具集，包括：Microsoft Visual Studio代码、Docker Desktop、Node.js和支持npm模块。

+ [设置本地开发环境](./set-up/development-environment.md)

### Adobe项目Firefly

asset compute项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发者控制台中的Adobe项目Firefly才能设置和部署它们。

+ [设置Adobe项目Firefly](./set-up/firefly.md)

## 开发

了解如何创建和配置Asset compute项目，然后开发可生成定制资产演绎版的自定义工作人员。

### 创建新Asset compute项目

asset compute项目(包含一个或多个Asset compute工作者)使用交互式Adobe I/OCLI生成。 asset compute项目是特殊结构化的Adobe项目Firefly项目，而Node.js项目又是Node.js项目。

+ [创建新Asset compute项目](./develop/project.md)

### 配置环境变量

环境变量在`.env`文件中进行维护，用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。

+ [配置环境变量](./develop/environment-variables.md)

### 配置manifest.yml

asset compute项目包含清单，它定义项目中包含的所有Asset compute工作人员，以及部署到Adobe I/O Runtime执行时可用的资源。

+ [配置manifest.yml](./develop/manifest.md)

### 开发员工

开发Asset compute工作器是扩展Asset compute微服务的核心，因为该工作器包含生成或编排生成资产再现的自定义代码。

+ [培养Asset compute工](./develop/worker.md)

### 使用Asset compute开发工具

asset compute开发工具提供了用于部署、执行和预览工作人员生成的再现的本地Web工具，支持快速、迭代的Asset compute工作人员开发。

+ [使用Asset compute开发工具](./develop/development-tool.md)

## 测试和调试

了解如何测试自定义Asset compute工作者对其操作的信心，以及如何调试Asset compute工作者以了解和解决如何执行自定义代码的问题。

### 测试工作人员

asset compute为创建Worker测试套件提供了一个测试框架，用于定义测试，确保行为正确易行。

+ [测试工作人员](./test-debug/test.md)

### 调试工作者

asset computeWorker提供从传统的`console.log(..)`输出到与&#x200B;__VS代码__&#x200B;和&#x200B;__wskdebug__&#x200B;集成的各种调试级别，允许开发人员在工作代码实时执行时遍历它。

+ [调试工作者](./test-debug/debug.md)

## 部署

了解如何将自定义Asset compute工作者与AEM作为Cloud Service集成，方法是先将它们部署到Adobe I/O Runtime，然后通过AEM资产的处理用户档案从AEM作为Cloud Service作者进行调用。

### 部署到Adobe I/O Runtime

asset compute工作人员必须部署到Adobe I/O Runtime，以便与AEM一起作为Cloud Service。

+ [使用处理用户档案](./deploy/runtime.md)

### 通过AEM处理用户档案集成工作人员

部署到Adobe I/O Runtime后，Asset compute工作人员可通过[资产处理用户档案](../../assets/configuring/processing-profiles.md)在AEM注册为Cloud Service。 处理用户档案反过来会应用于应用于其中资产的资产文件夹。

+ [与AEM处理用户档案集成](./deploy/processing-profiles.md)

## 高级

这些简略教程以前各章所确立的基础知识为基础，处理更高级的使用案例。

+ [开发可将元数](./advanced/metadata.md) 据写回Asset compute元数据的

## Github上的代码库

教程的代码库在Github上提供，网址为：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主控分支

源代码不包含必需的`.env`或`config.json`文件。 必须使用您的[帐户和服务](#accounts-and-services)信息添加和配置这些帐户。

## 其他资源

以下是各种Adobe资源，它们为开发Asset compute工作者提供了更多信息和有用的API和SDK。

### 文档

+ [asset compute服务文档](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute开发工具自述文件](https://github.com/adobe/asset-compute-devtool)
+ [asset compute示例工作人员](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute公域](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe云Blobstore包装器库](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe节点提取重试库](https://github.com/adobe/node-fetch-retry)
+ [asset compute示例工作人员](https://github.com/adobe/asset-compute-example-workers)
