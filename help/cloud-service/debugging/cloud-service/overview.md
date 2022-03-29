---
title: 调试AEMas a Cloud Service
description: 在自助式、可扩展的云基础架构上，这要求AEM开发人员了解如何了解和调试AEMas a Cloud Service的各个方面，从构建和部署到获取运行AEM应用程序的详细信息。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# 调试AEMas a Cloud Service

AEMas a Cloud Service是利用AEM应用程序的云原生方式。 AEM as a Cloud Service在自助式、可扩展的云基础架构上运行，这要求AEM开发人员了解如何了解和调试AEMas a Cloud Service的各个方面，从构建和部署到获取运行AEM应用程序的详细信息。

## 日志

日志提供了有关您的应用程序在AEMas a Cloud Service中如何运行的详细信息，以及有关部署问题的分析。

[使用日志调试AEMas a Cloud Service](./logs.md)

## 构建和部署

AdobeCloud Manager管道通过一系列步骤来部署AEM应用程序，以确定部署到AEM as a Cloud Service时的代码质量和可行性。 每个步骤都可能会导致失败，因此务必要了解如何调试内部版本以确定根本原因以及如何解决任何故障。

[调试AEMas a Cloud Service内部版本和部署](./build-and-deployment.md)

## 开发人员控制台

开发人员控制台在AEMas a Cloud Service环境中提供了各种信息和介绍，这些信息和介绍有助于了解应用程序如何被AEMas a Cloud Service识别以及在中的功能。

[使用开发人员控制台调试AEMas a Cloud Service](./developer-console.md)

## 存储库浏览器

存储库浏览器是一款功能强大的工具，可显示AEM基础数据存储，从而轻松调试AEMas a Cloud Service环境。 存储库浏览器支持在生产、暂存和开发以及创作、发布和预览服务中以只读方式查看AEM的资源和属性。

[使用存储库浏览器调试AEMas a Cloud Service](./repository-browser.md)
