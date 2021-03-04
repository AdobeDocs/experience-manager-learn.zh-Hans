---
title: 将AEM作为Cloud Service
description: 自助、可扩展的云基础架构，这要求AEM开发人员了解如何将AEM的各个方面理解和调试作为Cloud Service，从构建和部署到获取运行AEM应用程序的详细信息。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: 开发
role: 开发人员
level: 初学者，中级
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 2%

---


# 将AEM作为Cloud Service

AEM作为Cloud Service是利用AEM应用程序的云本机方式。 AEM作为一个Cloud Service运行在自助服务、可扩展的云基础架构上，这要求AEM开发人员了解如何将AEM作为一个Cloud Service的各个方面理解和调试，从构建和部署到获取运行AEM应用程序的详细信息。

## 日志

日志提供详细信息，说明您的应用程序在AEM中作为Cloud Service的运行情况，以及对部署问题的洞察。

[使用日志将AEM作为Cloud Service进行调试](./logs.md)

## 构建和部署

Adobe Cloud Manager管道通过一系列步骤部署AEM应用程序，以确定作为Cloud Service部署到AEM时的代码质量和可行性。 每个步骤都可能导致失败，因此了解如何调试构建以确定故障的根本原因以及如何解决任何故障非常重要。

[将AEM作为Cloud Service构建和部署进行调试](./build-and-deployment.md)

## 开发人员控制台

开发人员控制台将多种信息和内部介绍作为Cloud Service环境提供到AEM中，这些信息和内部介绍对于了解您的应用程序如何被AEM识别以及在中作为Cloud Service发挥作用非常有用。

[使用开发人员控制台将AEM作为Cloud Service进行调试](./developer-console.md)

## CRXDE Lite

CRXDE Lite是将AEM作为Cloud Service开发环境进行调试的经典而强大的工具。 CRXDE Lite提供一套功能，可帮助调试检查所有资源和属性、处理JCR的可变部分、调查权限和评估查询。

[将AEM作为Cloud Service与CRXDE Lite](./crxde-lite.md)
