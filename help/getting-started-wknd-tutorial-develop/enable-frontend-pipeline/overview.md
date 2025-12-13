---
title: 为标准 AEM 项目原型启用前端管道
description: 了解如何为标准 AEM 项目启用前端管道，以便更快地部署 CSS、JavaScript、字体、图标等静态资源。还有 AEM 上的前端开发与全栈后端开发的分离。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 206
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 100%

---

# 为标准 AEM 项目原型启用前端管道{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

了解如何启用通过 [AEM 项目原型](https://github.com/adobe/aem-project-archetype)创建的 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)（又称标准 AEM 项目），使用前端管道来部署 CSS、JavaScript、字体和图标等前端资源，从而加快从开发到部署的周期。AEM 上的前端开发与全栈后端开发的分离。您还将了解这些前端资源&#x200B;__并非__&#x200B;从 AEM 存储库提供，而是从内容传递网络提供，这是一种交付范式的改变。


在 Adobe Cloud Manager 中创建了一个新的前端管道，该管道仅构建 `ui.frontend` 工件并将其部署到内置内容传递网络，并将其位置告知 AEM。在 AEM 上生成网页 HTML 的过程中，`<link>` 和 `<script>` 标记指向位于 `href` 属性值中的这个工件的位置。

但是，在 WKND Sites AEM 项目转换之后，前端开发人员可以与 AEM 上的任何全栈后端开发工作分开同时进行，后者具有自己的部署管道。

>[!IMPORTANT]
>
>一般来说，前端管道通常与 [AEM 快速网站创建](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=zh-hans)结合使用，可以通过一个相关教程 [AEM Sites 快速入门 - 快速网站创建](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=zh-Hans)了解更多信息。在本教程和相关视频中您会看到对它的引用，这是为了确保指出细微的差别，并通过一些直接或间接的比较来解释关键概念。


相关的[多步骤教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=zh-Hans)详细说明了如何使用快速网站创建功能为虚构的生活方式品牌 WKND 实施一个 AEM 网站。查看[主题化工作流](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=zh-Hans)，了解前端管道工作也很有帮助。

## 前端管道的概述、优势和考虑事项

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>这仅适用于 AEM as a Cloud Service，不适用于基于 AMS 的 Adobe Cloud Manager 部署。

## 先决条件

本教程中的部署步骤在 Adobe Cloud Manager 中进行，请确保您有&#x200B;__部署管理者__&#x200B;角色，请参阅云管理[角色定义](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=zh-hans#role-definitions)。

请确保使用[沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=zh-Hans)和[开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=zh-Hans)完成本教程。

## 后续步骤 {#next-steps}

一个分步教程将引导您完成 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)转换，以使其适合前端管道。

您在等什么？现在开始本教程，先导航至[查看全栈项目](review-uifrontend-module.md)一章，在标准 AEM Sites 项目的上下文中回顾前端开发生命周期。
