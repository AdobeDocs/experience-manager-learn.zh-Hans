---
title: AEM Headless OpenAPI教程 | 内容片段投放
description: 一个端到端教程，其中演示了如何使用AEM的基于OpenAPI的内容片段投放API来构建和展示内容。
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 22%

---

# 开始使用OpenAPI进行AEM内容片段交付

浏览本教程，以说明如何在Headless AEM场景中使用AEM的OpenAPI内容片段投放并展示由外部应用程序使用的CMS内容。 本课程通过深入思考如何创建可显示WKND团队和相关成员详细信息的React应用程序来探索这些概念。 团队和成员使用AEM内容片段模型建模，由React应用程序使用带OpenAPI的AEM内容片段投放来使用。

![WKND Teams应用程序](./assets/overview/main.png)

本教程涵盖以下主题：

* 创建项目配置
* 创建内容片段模型以对数据进行建模
* 根据之前创建的模型创建内容片段
* 探索如何使用带OpenAPI文档的“Try It”功能的“AEM内容片段交付”来查询AEM中的内容片段
* 通过来自示例React应用程序的OpenAPI API调用AEM内容片段投放来使用内容片段数据
* 增强React应用程序以在通用编辑器中可编辑

## 先决条件 {#prerequisites}

学习本教程需具备以下条件：

* AEM Sites as a Cloud Service
* 基本的 HTML 和 JavaScript 技能
* 必须在本地安装以下工具：
   * [Node.js v22+](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE（例如，[Microsoft® Visual Studio Code](https://code.visualstudio.com/)）

### AEM as a Cloud Service环境

要完成本教程，建议您拥有AEM as a Cloud Service环境的&#x200B;**AEM管理员**&#x200B;访问权限。 也可以使用&#x200B;**开发**&#x200B;环境、**快速开发环境**&#x200B;或&#x200B;**沙盒程序**&#x200B;中的环境。

## 让我们开始吧！

从[定义内容片段模型](1-content-fragment-models.md)开始学习本教程。

## GitHub项目

源代码和内容包在[AEM Headless教程](https://github.com/adobe/aem-tutorials) GitHub存储库中可用。

[`main`分支包含本教程的最终源代码](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic)。
每个步骤末尾的代码快照均作为Git标记提供。

* 第4章开始 — React应用程序： [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* 第4章结束 — React应用程序： [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* 第5章结束 — 通用编辑器： [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

如果您在使用教程或代码时发现问题，欢迎[在 GitHub 提交问题](https://github.com/adobe/aem-tutorials/issues)。
