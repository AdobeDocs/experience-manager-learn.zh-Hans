---
title: 调试AEM as aCloud Service
description: 在自助式、可扩展的云基础架构上，这要求AEM开发人员了解如何了解和调试AEM as a Cloud Service的各个方面，从构建和部署到获取运行AEM应用程序的详细信息。
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---

# 调试AEM as aCloud Service

AEM as a Cloud Service是利用AEM应用程序的云原生方式。 AEM as aCloud Service在自助式、可扩展的云基础架构上运行，这要求AEM开发人员了解如何将AEM作为Cloud Service来了解和调试从构建和部署到获取运行AEM应用程序的详细信息等各个方面。

## 日志

日志提供了有关您的应用程序在AEM as a Cloud Service中运行情况的详细信息，以及有关部署问题的分析。

[使用日志调试AEM as aCloud Service](./logs.md)

## 构建和部署

AdobeCloud Manager管道通过一系列步骤来部署AEM应用程序，以确定将代码部署到AEM作为Cloud Service时的代码质量和可行性。 每个步骤都可能会导致失败，因此务必要了解如何调试内部版本以确定根本原因以及如何解决任何故障。

[调试AEM as a Cloud Service构建和部署](./build-and-deployment.md)

## 开发人员控制台

开发人员控制台在AEM作为Cloud Service环境中提供了各种信息和介绍，这些信息和介绍有助于了解AEM如何识别您的应用程序，以及如何在中作为Cloud Service使用。

[使用开发人员控制台调试AEM as aCloud Service](./developer-console.md)

## CRXDE Lite

CRXDE Lite是一款经典但功能强大的工具，可用于将AEM作为Cloud Service开发环境进行调试。 CRXDE Lite提供了一套功能，可帮助调试人员检查所有资源和属性、处理JCR的可变部分、调查权限和评估查询。

[将AEM作为Cloud Service进行CRXDE Lite](./crxde-lite.md)
