---
title: 开发注意事项
description: 在启用前端管道后，请考虑对前端和后端开发过程的影响。
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 79
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 开发注意事项

在启用前端管道以仅在AEM as a Cloud Service环境中部署前端资源后，会对本地AEM开发产生一些影响，因此您必须调整Git分支模型。

## 目标

* 如何拥有流畅的前端和后端开发流程
* 查看全栈管道和前端管道之间的依赖关系


## 本地开发注意事项

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 经调整发展方法（续）

* 对于使用AEM SDK的本地开发，后端开发团队仍需要通过`ui.frontend`模块生成clientlib，但在将Cloud Manager部署到AEM as a Cloud Service环境时，您必须跳过它。 这给如何隔离[更新项目](update-project.md)章节中概述的项目配置更改带来了挑战。

__解决方案__&#x200B;可以调整您的Git分支模型，并确保AEM项目配置更改不会流回AEM后端开发人员使用的&#x200B;__本地开发__&#x200B;分支。


* 作为对您的AEM项目持续增强的一部分，如果您引入新组件或更新在`ui.app`和`ui.frontend`模块都有更改的现有组件，则必须运行全栈管道和前端管道。
