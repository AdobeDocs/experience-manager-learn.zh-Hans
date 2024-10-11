---
title: 在AEM Forms中使用样式系统
description: 为按钮组件创建样式变体
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# 简介

Adobe Experience Manager (AEM)中的样式系统允许用户创建组件的多个可视化变量，然后选择在创作表单时要使用的样式。 这使组件变得更加灵活和可重用，而无需为每个样式创建自定义组件。

本文可帮助您创建按钮组件的变体，并在使用Cloud Manager将更改推送到云实例之前在本地云就绪环境中测试变体。

此屏幕抓图显示了表单作者可用的按钮组件的2种样式变化。


![按钮变体](assets/button-variations.png)

## 先决条件

* 包含核心组件的AEM Forms cloud ready实例。
* 克隆主题：在克隆主题时需要熟悉。 在本教程中，我们已克隆[画板主题](https://github.com/adobe/aem-forms-theme-easel)。 您可以克隆任何可用主题以满足您的需求。

* 安装Apache Maven的最新版本。 Apache Maven是一种常用于Java™项目的构建自动化工具。 安装最新版本可确保您具有主题自定义所需的依赖项。
* 安装纯文本编辑器。 例如，Microsoft® Visual Studio Code。 使用Microsoft等纯文本编辑器®Visual Studio Code为编辑和修改主题文件提供了用户友好的环境。



## 后续步骤

[创建样式策略](./style-policy.md)
