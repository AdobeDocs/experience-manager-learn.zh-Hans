---
title: 开发注意事项
description: 启用前端管道后，请考虑对前端和后端开发流程的影响。
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
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# 开发注意事项

启用前端管道以仅在AEMas a Cloud Service环境中部署前端资源后，会对本地AEM开发产生一些影响，您必须调整git分支模型。

## 目标

* 如何实现无摩擦的前端和后端开发流程
* 查看全栈和前端管道之间的依赖关系


## 本地开发考虑事项

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## 调整后的发展方法

* 对于使用AEM SDK的本地开发，后端开发团队仍需要通过 `ui.frontend` 模块，但在将Cloud Manager部署到AEMas a Cloud Service环境期间，您必须跳过该步骤。 这在如何隔离 [更新项目](update-project.md) 章节。

A __解决方案__ 可以调整您的git分支模型，并确保AEM项目配置更改从不流回到 __地方发展__ 分支AEM后端开发人员使用的。


* 如果您引入了新组件或更新了现有组件（两者均有更改），则作为AEM项目持续增强功能的一部分 `ui.app` 和 `ui.frontend` 模块，则必须同时运行全栈和前端管道。



