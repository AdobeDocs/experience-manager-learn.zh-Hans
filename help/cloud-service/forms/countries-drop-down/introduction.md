---
title: 创建国家/地区下拉列表组件
description: 基于aem forms核心下拉组件创建国家/地区下拉列表组件。
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# 根据下拉组件创建国家/地区下拉组件

在Adobe Experience Manager (AEM)中创建新核心组件是一个令人兴奋的过程，它涉及多个步骤，包括定义组件结构、自定义对话框以及实施用于动态功能的Sling模型。

在本教程结束时，您将掌握如何：

* 创建并使用Sling模型动态获取数据。
* 通过添加新字段和隐藏其他字段来自定义cq-dialog。
* 定义专为重用定制的强大组件结构。

名为“国家/地区”的组件将允许用户选择一个大陆，并在下拉列表中动态填充与所选大陆对应的国家/地区。 此操作将基于现成下拉列表组件构建，针对此特定用例进行了增强。

让我们深入了解并创建此动态且功能强大的组件！

## 先决条件

在Adobe Experience Manager (AEM)中构建新的核心组件需要满足多个先决条件才能确保顺利的开发过程。 以下是开始使用前您需要具备的条件：

* AEM开发环境：在本地运行的功能云就绪安装
* 访问AEM开发工具，如Visual Studio Code或IntelliJ
* 使用最新原型的MAven设置和AEM项目
* 了解AEM概念的基础知识
* 基本工具和设置，如Git存储库、JDK的正确版本


## 后续步骤

[创建组件结构](./component.md)
