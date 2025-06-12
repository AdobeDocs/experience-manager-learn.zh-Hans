---
title: 调试 AEM as a Cloud Service
description: 在自助服务、可扩展的云基础架构上运行，这要求 AEM 开发人员了解如何理解和调试 AEM as a Cloud Service 的各个方面——从构建和部署到获取运行中的 AEM 应用程序的详细信息。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '314'
ht-degree: 100%

---

# 调试 AEM as a Cloud Service

AEM as a Cloud Service 是利用 AEM 应用程序的云原生方式。AEM as a Cloud Service 在自助服务、可扩展的云基础架构上运行，这要求 AEM 开发人员了解如何理解和调试 AEM as a Cloud Service 的各个方面——从构建和部署到获取运行中的 AEM 应用程序的详细信息。

## 日志

日志提供了有关您的应用程序在 AEM as a Cloud Service 中运行情况的详细信息，以及对部署问题的洞察。

[使用日志调试 AEM as a Cloud Service](./logs.md)

## 生成和部署

Adobe Cloud Manager 管道通过一系列步骤部署 AEM应用程序，以确定将 AEM as a Cloud Service 部署到 AEM 时的代码质量和可行性。每个步骤都有可能出现失败，因此了解如何调试版本以确定故障根本原因并采取相应解决措施就显得尤为重要。

[调试 AEM as a Cloud Service 版本和部署](./build-and-deployment.md)

## Developer Console

Developer Console 提供了多种信息和对 AEM as a Cloud Service 环境的深入洞察，有助于开发人员了解其应用程序如何被 AEM as a Cloud Service 识别并在其中运行。

[使用 Developer Console 调试 AEM as a Cloud Service ](./developer-console.md)

## 存储库浏览器

存储库浏览器是一款功能强大的工具，可显示 AEM 基础数据存储，从而轻松调试 AEM as a Cloud Service 环境。 存储库浏览器支持以只读方式查看 AEM 在生产、预发布和开发环境中，以及在 Author、Publish 和 Preview 服务中的资源与属性。

[使用存储库浏览器调试 AEM as a Cloud Service](./repository-browser.md)
