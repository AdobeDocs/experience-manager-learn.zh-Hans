---
title: 为标准AEM项目原型启用前端管道
description: 转换标准AEM项目，以便使用前端管道更快地部署静态资源，例如CSS、JavaScript、字体、图标。 在AEM上实现前端开发与全栈后端开发的分离。
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 2%

---


# 为标准AEM项目原型启用前端管道{#enable-front-end-pipeline-standard-aem-project}

了解如何启用使用创建的标准AEM项目 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 使用前端管道部署静态资源（如CSS、JavaScript、字体、图标），以加快开发到部署的周期。 在AEM上实现前端开发与全栈后端开发的分离。 您还可以了解这些静态资源是如何 __not__ 从AEM存储库提供，但从CDN提供，这改变了交付模式。

新的前端管道在Adobe云管理器中创建，该管道仅构建和部署 `ui.frontend` 工件会发送到CDN并通知AEM其位置。 在网页的HTML生成过程中， `<link>` 和 `<script>` 标记在 `href` 属性值。

在标准的AEM项目转换之后，前端开发人员可以与AEM上的任何全栈后端开发分开处理并与之并行处理，后者具有自己的部署管道。

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>这仅适用于AEMas a Cloud Service，而不适用于基于AMS的AdobeCloud Manager部署。

