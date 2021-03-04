---
title: asset computeAEM作为Cloud Service的微服务可扩展性
description: 本教程将逐步介绍如何创建一个简单的Asset compute工作人员，该工作人员通过将原始资产裁剪成圆形来创建资产演绎版，并应用可配置的对比度和亮度。 虽然Worker本身是基本的，但本教程使用它来探索如何创建、开发和部署自定义Asset computeWorker，以便与AEM一起用作Cloud Service。
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 集成、开发
role: 开发人员
level: 中级，经验丰富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 0%

---


# asset compute微服务可扩展性

AEM作为Cloud Service的Asset compute microservices支持开发和部署自定义worker，这些自定义worker用于读取和操作存储在AEM中的资产的二进制数据，最常用的操作是创建自定义资产演绎版。

而在AEM 6.x中，自定义AEM工作流流程用于读取、转换和回写资产演绎版，而在AEM中，作为Cloud ServiceAsset compute工作者，它满足这一需求。

## 您将做什么

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程将逐步介绍如何创建一个简单的Asset compute工作人员，该工作人员通过将原始资产裁剪成圆形来创建资产演绎版，并应用可配置的对比度和亮度。 虽然Worker本身是基本的，但本教程使用它来探索如何创建、开发和部署自定义Asset computeWorker，以便与AEM一起用作Cloud Service。

### 目标{#objective}

1. 配置和设置必要的帐户和服务以构建和部署Asset compute工作人员
1. 创建和配置Asset compute项目
1. 开发生成自定义再现的Asset compute工作人员
1. 为编写测试，并学习如何调试自定义Asset compute工作器
1. 部署Asset compute工作者，并通过处理用户档案将其作为Cloud Service作者服务集成

## 设置

了解如何为扩展Asset computeWorker做好适当准备，了解哪些服务和帐户必须配置和配置，以及本地安装以用于开发的软件。

### 帐户和服务设置{#accounts-and-services}

以下帐户和服务需要设置和访问，以便完成教程(作为Cloud Service开发环境或沙箱项目的AEM)，访问Adobe Project Firefly和Microsoft Azure Blob存储。

+ [提供帐户和服务](./set-up/accounts-and-services.md)

### 本地开发环境

本地开发Asset compute项目需要一个特定的开发人员工具集，这与传统的AEM开发不同，包括：Microsoft Visual Studio代码、Docker Desktop、Node.js和支持npm模块。

+ [设置本地开发环境](./set-up/development-environment.md)

### AdobeProject Firefly

asset compute项目是特别定义的Adobe Project Firefly项目，因此，需要访问Adobe开发人员控制台中的Adobe Project Firefly才能设置和部署它们。

+ [设置Adobe项目Firefly](./set-up/firefly.md)

## 开发

了解如何创建和配置Asset compute项目，然后开发生成定制资产演绎版的自定义工作人员。

### 创建新Asset compute项目

asset compute项目(包含一个或多个Asset computeWorker)使用交互式Adobe I/OCLI生成。 asset compute项目是特别结构化的Adobe项目Firefly项目，而Node.js项目又是这些项目。

+ [创建新Asset compute项目](./develop/project.md)

### 配置环境变量

环境变量将保留在`.env`文件中以用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。

+ [配置环境变量](./develop/environment-variables.md)

### 配置manifest.yml

asset compute项目包含清单，用于定义项目中包含的所有Asset compute工作者，以及部署到Adobe I/O Runtime执行时可用的资源。

+ [配置manifest.yml](./develop/manifest.md)

### 开发员工

开发Asset compute工作器是扩展Asset compute微服务的核心，因为该工作器包含生成或安排生成资产再现的自定义代码。

+ [开发Asset compute工作者](./develop/worker.md)

### 使用Asset compute开发工具

asset compute Development Tool提供本地Web工具，用于部署、执行和预览工作人员生成的再现，支持快速、迭代的Asset compute工作人员开发。

+ [使用Asset compute开发工具](./develop/development-tool.md)

## 测试和调试

了解如何测试自定义Asset compute工作者对操作的信心，以及如何调试Asset compute工作者以了解和排除如何执行自定义代码的问题。

### 测试工作人员

asset compute提供了一个测试框架，用于为Worker创建测试套件，从而定义测试，确保行为正确易行。

+ [测试工作人员](./test-debug/test.md)

### 调试工作人员

asset computeWorker提供从传统的`console.log(..)`输出到与&#x200B;__VS Code__&#x200B;和&#x200B;__wskdebug__&#x200B;集成的各种调试级别，使开发人员能够在工作代码实时执行时遍历它。

+ [调试工作人员](./test-debug/debug.md)

## 部署

了解如何将自定义Asset computeWorker与AEM集成为Cloud Service，方法是首先将它们部署到Adobe I/O Runtime，然后通过AEM资产的处理用户档案从AEM作为Cloud Service作者进行调用。

### 部署到Adobe I/O Runtime

必须将asset computeWorker部署到Adobe I/O Runtime，以便与AEM一起用作Cloud Service。

+ [使用处理用户档案](./deploy/runtime.md)

### 通过AEM处理用户档案集成工作人员

部署到Adobe I/O Runtime后，Asset compute Worker可以通过[资产处理用户档案](../../assets/configuring/processing-profiles.md)在AEM中注册为Cloud Service。 处理用户档案反过来会应用于应用于其中资产的资产文件夹。

+ [与AEM处理用户档案集成](./deploy/processing-profiles.md)

## 高级

这些简略教程以前各章所确立的基础知识为基础，处理更高级的使用案例。

+ [开发可将元数](./advanced/metadata.md) 据写回到Asset compute的元数据工具

## Github上的代码库

在Github上，可以找到该教程的代码库：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主控分支

源代码不包含必需的`.env`或`config.json`文件。 必须使用您的[帐户和服务](#accounts-and-services)信息添加和配置这些帐户。

## 其他资源

以下是各种Adobe资源，它们为开发Asset computeWorker提供了更多信息和有用的API和SDK。

### 文档

+ [asset compute服务文档](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute开发工具自述文件](https://github.com/adobe/asset-compute-devtool)
+ [asset compute示例工作人员](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore包装器库](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe节点提取重试库](https://github.com/adobe/node-fetch-retry)
+ [asset compute示例工作人员](https://github.com/adobe/asset-compute-example-workers)
