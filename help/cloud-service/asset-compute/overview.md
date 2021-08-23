---
title: asset computeAEM作为Cloud Service的微服务可扩展性
description: 本教程将指导您创建一个简单的Asset compute工作程序，该工作程序通过将原始资产裁剪到圆圈来创建资产演绎版，并应用可配置的对比度和亮度。 虽然工作人员本身是基本的，但本教程将利用它来探索如何创建、开发和部署自定义Asset compute工作人员，以便与AEM作为Cloud Service一起使用。
feature: asset compute微服务
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 集成、开发
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1031'
ht-degree: 0%

---


# asset compute微服务可扩展性

AEM作为Cloud Service的Asset compute微服务支持开发和部署自定义工作程序，这些工作程序用于读取和操作存储在AEM中的资产的二进制数据，最常用的操作是创建自定义资产演绎版。

而在AEM 6.x中，自定义AEM工作流流程用于读取、转换和回写资产演绎版，而在AEM中，作为Cloud ServiceAsset compute工作程序，则会满足这一需求。

## 您将执行的操作

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程将指导您创建一个简单的Asset compute工作程序，该工作程序通过将原始资产裁剪到圆圈来创建资产演绎版，并应用可配置的对比度和亮度。 虽然工作人员本身是基本的，但本教程将利用它来探索如何创建、开发和部署自定义Asset compute工作人员，以便与AEM作为Cloud Service一起使用。

### 目标 {#objective}

1. 配置和设置必要的帐户和服务以构建和部署Asset compute工作人员
1. 创建和配置Asset compute项目
1. 开发生成自定义呈现版本的Asset compute工作程序
1. 为编写测试，并了解如何调试自定义Asset compute工作程序
1. 部署Asset compute工作程序，并通过处理配置文件将其作为Cloud Service创作服务集成到AEM中

## 设置

了解如何为扩展Asset compute工作程序做好适当准备，并了解必须配置和配置哪些服务和帐户，以及在本地安装软件以进行开发。

### 帐户和服务配置{#accounts-and-services}

以下帐户和服务需要配置和访问，以完成教程(AEM as a An Dev环境或沙盒计划)、对Adobe项目Firefly和Microsoft Azure Blob Storage的访问。

+ [提供账户和服务](./set-up/accounts-and-services.md)

### 本地开发环境

asset compute项目的本地开发需要一个与传统AEM开发不同的特定开发人员工具集，包括：Microsoft Visual Studio代码、Docker Desktop、Node.js和支持npm模块。

+ [设置本地开发环境](./set-up/development-environment.md)

### Adobe项目Firefly

asset compute项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发人员控制台中的Adobe项目Firefly才能设置和部署它们。

+ [设置Adobe项目Firefly](./set-up/firefly.md)

## 开发

了解如何创建和配置Asset compute项目，然后开发生成定制资产演绎版的自定义工作程序。

### 创建新Asset compute项目

asset compute项目(包含一个或多个Asset compute工作程序)使用交互式Adobe I/OCLI生成。 asset compute项目是特别结构化的Adobe项目Firefly项目，而Node.js项目又是Node.js项目。

+ [创建新Asset compute项目](./develop/project.md)

### 配置环境变量

环境变量在`.env`文件中进行维护以用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。

+ [配置环境变量](./develop/environment-variables.md)

### 配置manifest.yml

asset compute项目包含清单，该清单定义了项目中包含的所有Asset compute工作程序，以及这些工作程序部署到Adobe I/O Runtime执行时可用的资源。

+ [配置manifest.yml](./develop/manifest.md)

### 开发工作人员

开发Asset compute工作程序是扩展Asset compute微服务的核心，因为该工作程序包含生成或协调结果资产呈现的自定义代码。

+ [开发Asset compute工作人员](./develop/worker.md)

### 使用Asset compute开发工具

asset compute开发工具提供了本地Web工具，用于部署、执行和预览工作程序生成的演绎版，支持快速、迭代的Asset compute工作程序开发。

+ [使用Asset compute开发工具](./develop/development-tool.md)

## 测试和调试

了解如何测试自定义Asset compute工作程序以确保其操作有信心，以及调试Asset compute工作程序以了解和排除自定义代码的执行方式。

### 测试工作人员

asset compute提供了一个测试框架，用于为工作人员创建测试包，从而定义测试以确保行为正确易行。

+ [测试工作人员](./test-debug/test.md)

### 调试工作程序

asset compute工作程序提供从传统`console.log(..)`输出到与&#x200B;__VS代码__&#x200B;和&#x200B;__wskdebug__&#x200B;集成的各种级别的调试，从而允许开发人员在工作程序代码实时执行时逐步完成该代码。

+ [调试工作程序](./test-debug/debug.md)

## 部署

了解如何将自定义Asset compute工作程序与AEM as a AsCloud Service集成，方法是先将它们部署到Adobe I/O Runtime，然后通过AEM Assets的处理配置文件从AEM as a Author调用。

### 部署到Adobe I/O Runtime

asset compute工作程序必须部署到Adobe I/O Runtime才能与AEM作为Cloud Service一起使用。

+ [使用处理配置文件](./deploy/runtime.md)

### 通过AEM处理用户档案集成工作程序

部署到Adobe I/O Runtime后，Asset compute工作程序即可在AEM中通过[资产处理配置文件](../../assets/configuring/processing-profiles.md)注册为Cloud Service。 处理配置文件反过来会应用于应用于其中资产的资产文件夹。

+ [与AEM处理配置文件集成](./deploy/processing-profiles.md)

## 高级

这些简略教程基于前几章中建立的基础学习，处理更高级的用例。

+ [开发可以将元数](./advanced/metadata.md) 据写回到的Asset compute元数据工作器

## Github上的代码库

在Github上提供了本教程的代码库：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主控分支

源代码不包含所需的`.env`或`config.json`文件。 必须使用[帐户和服务](#accounts-and-services)信息添加和配置这些值。

## 其他资源

以下是各种Adobe资源，它们为开发Asset compute工作程序提供了更多信息以及有用的API和SDK。

### 文档

+ [asset compute服务文档](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute开发工具自述文件](https://github.com/adobe/asset-compute-devtool)
+ [asset compute示例工作程序](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute共用](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe云Blobstore包装器库](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe节点获取重试库](https://github.com/adobe/node-fetch-retry)
+ [asset compute示例工作程序](https://github.com/adobe/asset-compute-example-workers)
