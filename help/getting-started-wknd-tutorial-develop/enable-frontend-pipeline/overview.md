---
title: 为标准AEM项目原型启用前端管道
description: 了解如何为标准AEM项目启用前端管道，以便更快地部署静态资源，如CSS、JavaScript、字体和图标。 此外，在AEM上还将前端开发与全栈后端开发分离。
version: Experience Manager as a Cloud Service
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
duration: 206
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 1%

---

# 为标准AEM项目原型启用前端管道{#enable-front-end-pipeline-standard-aem-project}

了解如何启用使用[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)创建的[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)&#x200B;(又称标准AEM项目)，以便使用前端管道部署CSS、JavaScript、字体和图标等前端资源，从而加快开发到部署周期。 在AEM上将前端开发与全栈后端开发分离。 您还将了解这些前端资源是如何&#x200B;__不从AEM存储库提供__，而是从CDN提供，这是投放模式的更改。


在Adobe Cloud Manager中创建一个新的前端管道，该管道仅将`ui.frontend`工件构建和部署到内置CDN并通知AEM其位置。 在网页生成HTML期间，在AEM上，`<link>`和`<script>`标记在`href`属性值中引用此工件位置。

但是，在WKND Sites AEM项目转换之后，前端开发人员可以与AEM上的任何全栈后端开发分开工作，并且平行于该开发，后者具有自己的部署管道。

>[!IMPORTANT]
>
>一般而言，前端管道通常与[AEM快速站点创建](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=zh-Hans)一起使用，请参阅相关教程[AEM Sites快速入门 — 快速站点创建](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=zh-Hans)以了解详细信息。 在本教程和相关视频中，您会看到一些参考内容，这是为了确保能够揭示细微的差异，并且可以通过一些直接或间接的比较来解释关键概念。


相关的[多步教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=zh-Hans)将介绍如何使用快速站点创建功能为虚拟生活方式品牌WKND实施AEM站点。 查看[主题工作流](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=zh-Hans)以了解前端管道的工作方式也很有帮助。

## 前端管道的概述、优点和注意事项

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>这仅适用于AEM as a Cloud Service，不适用于基于AMS的Adobe Cloud Manager部署。

## 先决条件

本教程中的部署步骤在Adobe Cloud Manager中进行，请确保您具有&#x200B;__部署管理员__&#x200B;角色，请参阅云管理[角色定义](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=zh-Hans#role-definitions)。

完成本教程时，请确保使用[沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=zh-Hans)和[开发环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=zh-Hans)。

## 后续步骤 {#next-steps}

分步教程介绍[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)转换以便为前端管道启用它。

你在等什么？ 导航到[查看全栈项目](review-uifrontend-module.md)一章，开始本教程，并在标准AEM Sites项目上下文中回顾前端开发生命周期。
