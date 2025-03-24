---
title: AEM as a Cloud Service的Asset Compute微服务可扩展性
description: 本教程介绍如何创建简单的Asset Compute工作程序，该工作程序通过将原始资源裁切到圆圈来创建资源演绎版，并应用可配置的对比度和亮度。 虽然工作进程本身是基础性的，但本教程将使用该工作进程来探索创建、开发和部署自定义Asset Compute工作进程以用于AEM as a Cloud Service。
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
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Asset Compute微服务可扩展性

AEM as Cloud Service的Asset Compute微服务支持开发和部署自定义工作程序，用于读取和操作存储在AEM中的资产的二进制数据，最常用的操作是创建自定义资产演绎版。

在AEM 6.x中，自定义AEM工作流进程用于读取、转换和写回资产演绎版，而在AEM as a Cloud Service Asset Compute中，工作进程可满足此需求。

## 您将要做什么

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教程介绍如何创建简单的Asset Compute工作程序，该工作程序通过将原始资源裁切到圆圈来创建资源演绎版，并应用可配置的对比度和亮度。 虽然工作进程本身是基础性的，但本教程将使用该工作进程来探索创建、开发和部署自定义Asset Compute工作进程以用于AEM as a Cloud Service。

### 目标 {#objective}

1. 配置和设置必要的帐户和服务以构建和部署Asset Compute工作人员
1. 创建和配置Asset Compute项目
1. 开发可生成自定义演绎版的Asset Compute Worker
1. 编写测试并了解如何调试自定义Asset Compute Worker
1. 部署Asset Compute Worker，并通过处理配置文件集成其AEM as a Cloud Service Author服务

## 设置

了解如何正确准备扩展Asset Compute工作程序，并了解必须配置和配置哪些服务和帐户，以及在本地安装哪些软件以进行开发。

### 帐户和服务配置{#accounts-and-services}

要完成教程、 AEM as a Cloud Service开发环境或沙盒程序，以及对App Builder和Microsoft Azure Blob Storage的访问权限，以下帐户和服务需要配置和访问权限。

+ [提供帐户和服务](./set-up/accounts-and-services.md)

### 本地开发环境

Asset Compute项目的本地开发需要特定的开发人员工具集，该工具集不同于传统的AEM开发，包括：Microsoft Visual Studio Code、Docker Desktop、Node.js和支持npm的模块。

+ [设置本地开发环境](./set-up/development-environment.md)

### App Builder

Asset Compute项目是特别定义的App Builder项目，因此，需要访问Adobe Developer Console中的App Builder才能设置和部署这些项目。

+ [设置App Builder](./set-up/app-builder.md)

## 开发

了解如何创建和配置Asset Compute项目，然后开发可生成定制资源演绎版的自定义工作程序。

### 创建新的Asset Compute项目

Asset Compute项目包含一个或多个使用交互式Adobe I/O CLI生成的Asset Compute工作程序。 Asset Compute项目是专门的结构化App Builder项目，依次是Node.js项目。

+ [创建新的Asset Compute项目](./develop/project.md)

### 配置环境变量

环境变量在`.env`文件中进行维护以用于本地开发，并用于提供本地开发所需的Adobe I/O凭据和云存储凭据。

+ [配置环境变量](./develop/environment-variables.md)

### 配置manifest.yml

Asset Compute项目包含清单，这些清单定义了项目中包含的所有Asset Compute工作程序，以及这些工作程序在部署到Adobe I/O Runtime以执行时可用的资源。

+ [配置manifest.yml](./develop/manifest.md)

### 开发工作人员

开发Asset Compute工作程序是扩展Asset Compute微服务的核心，因为该工作程序包含生成或协调生成结果资源演绎版的自定义代码。

+ [开发Asset Compute员工](./develop/worker.md)

### 使用Asset Compute开发工具

Asset Compute Development Tool提供了一个本地Web工具，可用于部署、执行和预览工作人员生成的演绎版，并支持快速而迭代的Asset Compute工作人员开发。

+ [使用Asset Compute开发工具](./develop/development-tool.md)

## 测试和调试

了解如何测试自定义Asset Compute工作程序以确保其正常运行，以及调试Asset Compute工作程序以了解自定义代码的执行方式并对其进行故障排除。

### 测试工作人员

Asset Compute提供了一个测试框架，用于为员工创建测试套件，从而轻松定义确保正常行为的测试。

+ [测试工作人员](./test-debug/test.md)

### 调试工作程序

Asset Compute工作进程提供了从传统`console.log(..)`输出到与&#x200B;__VS代码__&#x200B;和&#x200B;__wskdebug__&#x200B;集成的各种级别的调试，从而允许开发人员在工作进程代码实时执行时逐步执行该代码。

+ [调试工作程序](./test-debug/debug.md)

## 部署

了解如何将自定义Asset Compute工作程序与AEM as a Cloud Service集成，方法是先将自定义工作程序部署到Adobe I/O Runtime，然后通过AEM as a Cloud Service的处理配置文件从AEM Assets Author进行调用。

### 部署到Adobe I/O Runtime

Asset Compute工作人员必须部署到Adobe I/O Runtime才能与AEM as a Cloud Service一起使用。

+ [使用处理配置文件](./deploy/runtime.md)

### 通过AEM处理用户档案集成工作人员

部署到Adobe I/O Runtime后，可以通过[Asset Compute处理配置文件](../../assets/configuring/processing-profiles.md)在AEM as a Cloud Service中注册Assets工作程序。 处理用户档案依次应用于应用于其中资产的资产文件夹。

+ [与AEM处理用户档案集成](./deploy/processing-profiles.md)

## 高级

这些简短的教程基于前几章中的基础学习来处理更高级的用例。

+ [开发一个可以将元数据写回的Asset Compute元数据工作程序](./advanced/metadata.md)

## 基于Github的代码

该教程的代码库可在Github上获取，网址为：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

源代码不包含所需的`.env`或`config.json`文件。 必须使用您的[帐户和服务](#accounts-and-services)信息添加和配置这些帐户。

## 其他资源

以下是各种Adobe资源，这些资源为开发Asset Compute工作程序提供了进一步的信息和有用的API和SDK。

### 文档

+ [Asset Compute服务文档](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [Asset Compute开发工具自述文件](https://github.com/adobe/asset-compute-devtool)
+ [Asset Compute示例工作程序](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute共享资源](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore包装器库](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe节点提取重试库](https://github.com/adobe/node-fetch-retry)
+ [Asset Compute示例工作程序](https://github.com/adobe/asset-compute-example-workers)
