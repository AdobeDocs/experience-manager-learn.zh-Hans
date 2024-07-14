---
title: AEM as a Cloud Service的Asset compute微服务可扩展性
description: 本教程介绍如何创建简单的Asset compute工作程序，该工作程序通过将原始资源裁切到圆圈来创建资源演绎版，并应用可配置的对比度和亮度。 虽然工作进程本身是基础性的，但本教程将使用该工作进程来探索创建、开发和部署自定义Asset compute工作进程以用于AEM as a Cloud Service。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# asset compute微服务可扩展性

AEM as a Cloud Service的Asset compute微服务支持开发和部署自定义工作程序，用于读取和操作存储在AEM中的资产的二进制数据，最常用的操作是创建自定义资产演绎版。

在AEM 6.x中，自定义AEM Workflow进程用于读取、转换和写回资源演绎版，而在AEM as a Cloud Service中，Asset compute工作进程可满足此需求。

## 您将要做什么

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程介绍如何创建简单的Asset compute工作程序，该工作程序通过将原始资源裁切到圆圈来创建资源演绎版，并应用可配置的对比度和亮度。 虽然工作进程本身是基础性的，但本教程将使用该工作进程来探索创建、开发和部署自定义Asset compute工作进程以用于AEM as a Cloud Service。

### 目标 {#objective}

1. 提供并设置必要的帐户和服务以构建和部署Asset compute工作人员
1. 创建和配置Asset compute项目
1. 开发可生成自定义呈现版本的Asset compute工作程序
1. 编写测试并了解如何调试自定义Asset compute工作程序
1. 部署Asset compute工作程序，并通过处理配置文件将其与AEM as a Cloud Service创作服务相集成

## 设置

了解如何正确准备扩展Asset compute工作人员，并了解必须配置和配置哪些服务和帐户，以及本地安装用于开发的软件。

### 帐户和服务配置{#accounts-and-services}

要完成教程、 AEM as a Cloud Service开发环境或沙盒程序，以及对App Builder和Microsoft Azure Blob Storage的访问权限，以下帐户和服务需要配置和访问权限。

+ [提供帐户和服务](./set-up/accounts-and-services.md)

### 本地开发环境

asset compute项目的本地开发需要特定的开发人员工具集，该工具集不同于传统的AEM开发，包括：Microsoft Visual Studio Code、Docker Desktop、Node.js和支持npm的模块。

+ [设置本地开发环境](./set-up/development-environment.md)

### App Builder

asset compute项目是特别定义的App Builder项目，因此，需要访问Adobe Developer Console中的App Builder才能设置和部署这些项目。

+ [设置App Builder](./set-up/app-builder.md)

## 开发

了解如何创建和配置Asset compute项目，然后开发可生成定制资源演绎版的自定义工作程序。

### 创建新的Asset compute项目

包含一个或多个Asset compute工作进程的Asset compute项目是使用交互式Adobe I/OCLI生成的。 asset compute项目是专门的结构化App Builder项目，依次是Node.js项目。

+ [创建新的Asset compute项目](./develop/project.md)

### 配置环境变量

环境变量在`.env`文件中进行维护以用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。

+ [配置环境变量](./develop/environment-variables.md)

### 配置manifest.yml

asset compute项目包含清单，这些清单定义了项目中包含的所有Asset compute工作程序，以及这些工作程序在部署到Adobe I/O Runtime以供执行时可用的资源。

+ [配置manifest.yml](./develop/manifest.md)

### 开发工作人员

开发Asset compute工作程序是扩展Asset compute微服务的核心，因为该工作程序包含用于生成或协调生成的资源演绎版的自定义代码。

+ [开发Asset compute工作人员](./develop/worker.md)

### 使用Asset compute开发工具

“Asset compute开发工具”提供了一个本地Web工具，可用于部署、执行和预览工作程序生成的呈现形式，并支持快速的反复Asset compute工作程序开发。

+ [使用Asset compute开发工具](./develop/development-tool.md)

## 测试和调试

了解如何测试自定义Asset compute工作程序以确保其正常运行，以及调试Asset compute工作程序以了解自定义代码的执行方式并对其进行故障排除。

### 测试工作人员

asset compute提供了一个测试框架，用于为工作人员创建测试套件，使得定义测试来确保正常行为变得容易。

+ [测试工作人员](./test-debug/test.md)

### 调试工作程序

asset compute工作进程提供从传统`console.log(..)`输出到与&#x200B;__VS代码__&#x200B;和&#x200B;__wskdebug__&#x200B;集成的各种级别的调试，允许开发人员在工作进程代码实时执行时逐步执行该代码。

+ [调试工作程序](./test-debug/debug.md)

## 部署

了解如何将自定义Asset compute工作程序与AEM as a Cloud Service集成，方法是首先将其部署到Adobe I/O Runtime，然后通过AEM as a Cloud Service的处理配置文件从AEM Assets Author调用。

### 部署到Adobe I/O Runtime

asset compute工作人员必须部署到Adobe I/O Runtime才能与AEM as a Cloud Service一起使用。

+ [使用处理配置文件](./deploy/runtime.md)

### 通过AEM处理用户档案集成工作人员

部署到Adobe I/O Runtime后，可以通过[Assets处理配置文件](../../assets/configuring/processing-profiles.md)在AEM as a Cloud Service中注册Asset compute工作程序。 处理用户档案依次应用于应用于其中资产的资产文件夹。

+ [与AEM处理用户档案集成](./deploy/processing-profiles.md)

## 高级

这些简短的教程基于前几章中的基础学习来处理更高级的用例。

+ [开发一个Asset compute元数据工作程序](./advanced/metadata.md)，该工作程序可以将元数据写回

## 基于Github的代码

该教程的代码库可在Github上获取，网址为：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

源代码不包含所需的`.env`或`config.json`文件。 必须使用您的[帐户和服务](#accounts-and-services)信息添加和配置这些帐户。

## 其他资源

以下是各种Adobe资源，它们为开发Asset compute工作程序提供了进一步的信息和有用的API和SDK。

### 文档

+ [Asset compute服务文档](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [Asset compute开发工具自述文件](https://github.com/adobe/asset-compute-devtool)
+ [Asset compute示例辅助进程](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [Asset computeSDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset compute共享资源](https://github.com/adobe/asset-compute-commons)
   + [Asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe云Blobstore包装库](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe节点提取重试库](https://github.com/adobe/node-fetch-retry)
+ [Asset compute示例辅助进程](https://github.com/adobe/asset-compute-example-workers)
