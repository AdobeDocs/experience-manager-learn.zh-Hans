---
title: 作为Cloud Service的AEM的资产计算微服务可扩展性
description: 本教程将逐步介绍如何创建一个简单的资产计算工作人员，该工作人员通过将原始资产裁剪成圆形来创建资产演绎版，并应用可配置的对比度和亮度。 虽然工作人员本身是基本的，但本教程使用它来探索创建、开发和部署自定义资产计算工作人员，以便与AEM一起作为Cloud Service。
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 0%

---


# 资产计算微服务可扩展性

AEM作为Cloud Service的Asset Compute Microservices支持开发和部署自定义工作器，这些工作器用于读取和操作存储在AEM中的资产的二进制数据，最常用的操作是创建自定义资产演绎版。

而在AEM 6.x中，自定义AEM工作流流程用于读取、转换和回写资产演绎版，在AEM中，作为Cloud Service资产计算工作程序，它满足了这一需求。

## 您将做什么

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程将逐步介绍如何创建一个简单的资产计算工作人员，该工作人员通过将原始资产裁剪成圆形来创建资产演绎版，并应用可配置的对比度和亮度。 虽然工作人员本身是基本的，但本教程使用它来探索创建、开发和部署自定义资产计算工作人员，以便与AEM一起作为Cloud Service。

### 目标 {#objective}

1. 配置和设置必要的帐户和服务以构建和部署资产计算工作人员
1. 创建和配置资产计算项目
1. 开发资产计算工作人员，以生成自定义演绎版
1. 为编写测试，并了解如何调试自定义资产计算工作器
1. 部署资产计算工作者，并通过处理用户档案将其作为Cloud Service作者服务集成在一起

## 设置

了解如何正确准备扩展资产计算工作器，并了解必须配置和配置哪些服务和帐户以及在本地安装软件进行开发。

### 帐户和服务配置{#accounts-and-services}

以下帐户和服务需要设置和访问，以完成教程AEM(作为Cloud Service开发环境或沙箱项目)、访问Adobe项目Firefly和Microsoft Azure Blob存储。

+ [提供帐户和服务](./set-up/accounts-and-services.md)

### 本地开发环境

资产计算项目的本地开发需要一个特定的开发人员工具集，这与传统的AEM开发不同，包括：Microsoft Visual Studio代码、Docker Desktop、Node.js和支持npm模块。

+ [设置本地开发环境](./set-up/development-environment.md)

### Adobe项目Firefly

资产计算项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发者控制台中的Adobe项目Firefly才能设置和部署它们。

+ [设置Adobe项目Firefly](./set-up/firefly.md)

## 开发

了解如何创建和配置资产计算项目，然后开发生成定制资产演绎版的自定义工作人员。

### 创建新的资产计算项目

资产计算项目（包含一个或多个资产计算工作程序）使用交互式AdobeI/O CLI生成。 资产计算项目是特殊结构化的Adobe项目Firefly项目，而Node.js项目又是Node.js项目。

+ [创建新的资产计算项目](./develop/project.md)

### 配置环境变量

环境变量在文件中进 `.env` 行维护，用于提供本地开发所需的AdobeI/O凭据和云存储凭据。

+ [配置环境变量](./develop/environment-variables.md)

### 配置manifest.yml

资产计算项目包含清单，它定义项目中包含的所有资产计算工作人员，以及部署到Adobe I/O Runtime执行时可用的资源。

+ [配置manifest.yml](./develop/manifest.md)

### 开发员工

开发资产计算工作人员是扩展资产计算微服务的核心，因为该工作人员包含生成或编排生成资产演绎版的自定义代码。

+ [开发资产计算工作人员](./develop/worker.md)

### 使用Asset Compute Development Tool

资产计算开发工具提供本地Web工具，用于部署、执行和预览工作人员生成的演绎版，支持快速、迭代的资产计算工作人员开发。

+ [使用Asset Compute Development Tool](./develop/development-tool.md)

## 测试和调试

了解如何测试自定义资产计算工作线程对其操作的信心，以及如何调试资产计算工作线程来了解和排除如何执行自定义代码的问题。

### 测试工作人员

资产计算提供了一个测试框架，用于为工作者创建测试套件，从而定义测试，确保行为正确易行。

+ [测试工作人员](./test-debug/test.md)

### 调试工作者

资产计算工作线程提供从传统输出到与 `console.log(..)` VS代码和wskdebug集成的各 __种调试级别__ ，使开 ____&#x200B;发人员能够在工作线程代码的实时执行中逐步完成。

+ [调试工作者](./test-debug/debug.md)

## 部署

了解如何将自定义资产计算工作者与AEM作为Cloud Service集成，方法是首先将它们部署到Adobe I/O Runtime，然后通过AEM Assets处理用户档案从AEM作为Cloud Service作者进行调用。

### 部署到Adobe I/O Runtime

必须将资产计算工作者部署到Adobe I/O Runtime，以便与AEM一起作为Cloud Service。

+ [使用处理用户档案](./deploy/runtime.md)

### 通过AEM处理用户档案集成工作人员

部署到Adobe I/O Runtime后，资产计算工作者可以通过资产处理用户档案在AEM中 [注册为Cloud Service](../../assets/configuring/processing-profiles.md)。 处理用户档案反过来会应用于应用于其中资产的资产文件夹。

+ [与AEM处理用户档案集成](./deploy/processing-profiles.md)

## Github上的教程代码库

教程代码库可在Github上找到，网址为：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主控分支

源代码不包含所需的 `.env` 或文 `config.json` 件。 必须使用您的帐户和服务信息 [添加和配置这些](#accounts-and-services) 。

## 其他资源

以下是各种Adobe资源，它们提供用于开发资产计算工作器的更多信息和有用的API和SDK。

### 文档

+ [资产计算服务文档](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [资产计算开发工具自述文件](https://github.com/adobe/asset-compute-devtool)

### 其他代码示例

+ [资产计算示例工作程序](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [资产计算SDK](https://github.com/adobe/asset-compute-sdk)
   + [资产计算公域](https://github.com/adobe/asset-compute-commons)
+ [Adobe云Blobstore包装器库](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe节点提取重试库](https://github.com/adobe/node-fetch-retry)
