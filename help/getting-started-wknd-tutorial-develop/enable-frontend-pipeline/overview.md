---
title: 为标准AEM项目原型启用前端管道
description: 了解如何为标准AEM项目启用前端管道，以便更快地部署静态资源，如CSS、JavaScript、字体、图标。 此外，还在AEM上将前端开发与全栈后端开发分离。
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 4%

---

# 为标准AEM项目原型启用前端管道{#enable-front-end-pipeline-standard-aem-project}

了解如何启用 [AEM WKND站点项目](https://github.com/adobe/aem-guides-wknd) (又称标准AEM项目)创建于 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 使用前端管道部署CSS、JavaScript、字体和图标等前端资源，以加快开发到部署周期。 在AEM上将前端开发与全栈后端开发分离。 您还将了解这些前端资源是如何运作的 __非__ 从AEM存储库提供，但从CDN提供，改变了交付模式。


在Adobe Cloud Manager中创建了一个新的前端管道，该管道仅构建和部署 `ui.frontend` 内置CDN的工件并通知AEM其位置。 在网页HTML生成期间的AEM上， `<link>` 和 `<script>` 标记中，请参阅此工件在 `href` 属性值。

但是，在WKND Sites AEM项目转换后，前端开发人员可以与AEM上的任何全栈后端开发分开和并行工作，后者有自己的部署管道。

>[!IMPORTANT]
>
>一般而言，前端管道通常与 [AEM快速站点创建](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)，提供了相关教程 [AEM Sites快速入门 — 快速网站创建](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 以进一步了解它。 在本教程和相关视频中，您会看到一些参考内容，这是为了确保能够揭示细微的差异，并且可以通过一些直接或间接的比较来解释关键概念。


相关 [多步教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 介绍如何使用快速站点创建功能为虚构的生活方式品牌WKND实施AEM站点。 查看 [主题化工作流](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 了解前端管道的工作方式也很有帮助。

## 前端管道的概述、优点和注意事项

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>这仅适用于AEMas a Cloud Service，不适用于基于AMS的AdobeCloud Manager部署。

## 前提条件

本教程中的部署步骤将在AdobeCloud Manager中进行，请确保您拥有 __部署管理员__ 角色，请参阅云管理 [角色定义](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

确保使用 [沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) 和 [开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) 完成本教程时。

## 后续步骤 {#next-steps}

分步教程将介绍 [AEM WKND站点项目](https://github.com/adobe/aem-guides-wknd) 转化，为前端管道启用它。

你在等什么？ 导航到 [查看全栈项目](review-uifrontend-module.md) 本章回顾了标准AEM Sites项目背景下前端开发的生命周期。
