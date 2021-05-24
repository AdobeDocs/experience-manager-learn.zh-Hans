---
title: 为打印渠道创建您的第一个交互式通信
seo-title: 为打印渠道创建您的第一个交互式通信
description: 交互式通信是AEM Forms 6.4的新增功能。本文档将指导您完成为打印渠道创建交互式通信所需的步骤。
seo-description: 交互式通信是AEM Forms 6.4的新增功能。本文档将指导您完成为打印渠道创建交互式通信所需的步骤。
feature: 交互式通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 3%

---


# 为打印渠道创建您的第一个交互式通信

交互式通信是AEM Forms 6.4的新增功能。本文档将指导您完成为打印渠道创建交互式通信所需的步骤。

请访问[AEM Forms示例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)页面，获取此功能的实时演示链接。

## 前提条件 {#prerequistes}

[使用包管理器将与本教程相关的资产下载并导入AEM。](assets/gettingstartedassets.zip)此zip文件包含作为资产包一部分的图像、文档片段、监视文件夹配置和布局文件(xdp)

[下载并解压缩此文件。](assets/warfileandswaggerfile.zip) 此文件包含需要部署到Tomcat上的SampleRest.war文件以及需要用于配置数据源的swagger文件。

在完成本教程后，您将学习到以下内容：

* 创建数据源
* 创建表单数据模型
* 创建文档片段
* 配置表和图表
* 使用监视文件夹在批处理模式下生成文档

