---
title: 开发注意事项
description: 考虑启用前端管道后对前端和后端开发过程的影响。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 开发注意事项

在启用前端管道以仅在AEMas a Cloud Service环境中部署前端资源后，对本地AEM开发有一些影响，您必须调整Git分支模型。

## 目标

* 如何实现流畅的前端和后端开发流程
* 查看全栈管道和前端管道之间的依赖关系


## 本地开发注意事项

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 调整后的发展方法

* 对于使用AEM SDK的本地开发，后端开发团队仍需要通过生成clientlib `ui.frontend` 模块，但在Cloud Manager部署到AEMas a Cloud Service环境期间，您必须跳过它。 这给如何隔离中概述的项目配置更改带来了挑战 [更新项目](update-project.md) 章节。

A __解决方案__ 可以是调整您的Git分支模型，并确保AEM项目配置更改绝不会流回 __本地开发__ AEM后端开发人员使用的分支。


* 作为对AEM项目持续增强的一部分，如果您引入新组件或更新了两个组件中都有更改的现有组件 `ui.app` 和 `ui.frontend` 模块，您必须同时运行全栈管道和前端管道。
