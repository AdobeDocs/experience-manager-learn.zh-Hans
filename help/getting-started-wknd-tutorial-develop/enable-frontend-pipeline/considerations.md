---
title: 开发考虑事项
description: 启用前端管道后，您应考虑对前端和后端开发过程的影响。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 79
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 100%

---

# 开发考虑事项

启用前端管道后，只有前端资源会部署到 AEM as a Cloud Service 环境中，这会对本地 AEM 开发产生一些影响，您必须调整 git 分支模型。

## 目标

* 如何实现顺畅的前端和后端开发流
* 查看全栈与前端管道之间的依赖项


## 本地开发考虑事项

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 调整开发方式

* 在使用 AEM SDK 进行的本地开发中，后端开发团队仍然需要通过 `ui.frontend` 模块生成客户端库，但在将 Cloud Manager 部署到 AEM as a Cloud Service 环境的过程中，您必须跳过它。这带来了一个挑战，即如何隔离[更新项目](update-project.md)一章中概述的项目配置更改。

可能的一个&#x200B;__解决方案__&#x200B;是调整您的 git 分支模型，确保 AEM 项目配置更改绝不会流回到 AEM 后端开发人员使用的&#x200B;__本地开发__&#x200B;分支。


* 作为 AEM 项目持续增强的一部分，如果您引入新组件或者更新一个在 `ui.app` 和 `ui.frontend` 模块中均发生更改的现有组件，就必须同时运行全栈和前端管道。
