---
title: 将AEM作为Cloud Service进行调试
description: 在自助服务、可扩展的云基础架构上，这要求AEM开发人员了解如何理解和调试作为Cloud Service的AEM的各个方面，从构建和部署到获取运行的AEM应用程序的详细信息。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
translation-type: tm+mt
source-git-commit: debb65a218ae0b9e5105c3f63018358902330b34
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---


# 将AEM作为Cloud Service进行调试

AEM作为Cloud Service是利用AEM应用程序的云本机方式。 AEM作为Cloud Service运行于自助服务、可扩展的云基础架构上，这要求AEM开发人员了解如何理解和调试AEM作为Cloud Service的各个方面，从构建和部署到获取运行的AEM应用程序的详细信息。

## 日志

日志提供了有关您的应用程序在AEM中作为Cloud Service的运行方式的详细信息，以及对部署问题的洞察。

[使用日志将AEM作为Cloud Service进行调试](./logs.md)

## 构建和部署

Adobe云管理器管道通过一系列步骤部署AEM应用程序，以确定部署到AEM作为Cloud Service时的代码质量和可行性。 每个步骤都可能导致失败，因此了解如何调试构建以确定故障的根本原因以及如何解决任何故障非常重要。

[将AEM作为Cloud Service构建和部署进行调试](./build-and-deployment.md)

## 开发人员控制台

开发人员控制台将各种信息和介绍作为Cloud Service环境引入AEM，这些信息和介绍对于了解您的应用程序如何被AEM识别以及在Cloud Service中的功能是有用的。

[使用开发人员控制台将AEM作为Cloud Service进行调试](./developer-console.md)

## CRXDE Lite

CRXDE Lite是调试AEM作为Cloud Service开发环境的经典但功能强大的工具。 CRXDE Lite提供一套功能，有助于调试检查所有资源和属性、处理JCR的可变部分、调查权限和评估查询。

[将AEM作为Cloud Service进行CRXDE Lite](./crxde-lite.md)
