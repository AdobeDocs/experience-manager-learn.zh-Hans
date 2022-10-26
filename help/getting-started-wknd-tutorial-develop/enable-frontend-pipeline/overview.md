---
title: 为标准AEM项目原型启用前端管道
description: 了解如何为标准AEM项目启用前端管道，以便更快地部署静态资源，如CSS、JavaScript、字体、图标。 还将前端开发与AEM上的全栈后端开发分离。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# 为标准AEM项目原型启用前端管道{#enable-front-end-pipeline-standard-aem-project}

了解如何启用 [AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd) (也称为标准AEM项目) [AEM项目原型](https://github.com/adobe/aem-project-archetype) 使用前端管道部署前端资源（如CSS、JavaScript、字体和图标），以缩短开发到部署的周期。 在AEM上实现前端开发与全栈后端开发的分离。 您还将了解这些前端资源是如何 __not__ 从AEM存储库提供，但从CDN提供，这是交付范式的变化。


在AdobeCloud Manager中创建了新的前端管道，该管道仅构建和部署 `ui.frontend` 对象会发送到内置的CDN，并通知AEM其位置。 在网页HTML生成期间的AEM上， `<link>` 和 `<script>` 标记中，请在 `href` 属性值。

但是，在WKND Sites AEM项目转换后，前端开发人员可以与AEM上的任何全栈后端开发分开处理，并与之并行处理，后者拥有自己的部署管道。

>[!IMPORTANT]
>
>通常，前端管道与 [AEM快速网站创建](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)，则将提供相关教程 [AEM Sites快速入门 — 快速创建网站](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 以了解更多相关信息。 因此，在本教程和相关视频中，您会遇到对其的引用，这是为了确保能够找出细微差异，并且有一些直接或间接的比较来解释关键概念。


相关 [多步教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 逐步介绍如何使用“快速创建网站”功能，在WKND中实施AEM网站，以打造虚构的生活方式品牌。 审核 [主题工作流](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 了解前端管道工作原理也很有帮助。

## 前端管道的概述、优势和注意事项

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>这仅适用于AEMas a Cloud Service，而不适用于基于AMS的AdobeCloud Manager部署。

## 前提条件

本教程中的部署步骤在Analytics Cloud Manager中进行，请确保您具有 __部署管理器__ 角色，请参阅云管理 [角色定义](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

确保使用 [沙盒项目](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) 和 [开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) 完成本教程时。

## 下面的步骤 {#next-steps}

分步教程将演示 [AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd) 转换，以便为前端管道启用它。

你在等什么？ 通过导航到 [查看全栈项目](review-uifrontend-module.md) 章节和回顾在标准AEM Sites项目背景下的前端开发生命周期。

