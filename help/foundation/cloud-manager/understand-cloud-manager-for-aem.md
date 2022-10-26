---
title: 了解AdobeCloud Manager
description: AdobeCloud Manager提供了简单而强大的解决方案，允许轻松管理、引入和自助服务AEM环境。
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 2%

---

# 了解AdobeCloud Manager

AdobeCloud Manager提供了简单而强大的解决方案，允许轻松管理、引入和自助服务AEM环境。

## Cloud Manager概述

本视频系列探讨了Cloud Manager for AEM的主要功能，包括：

* [项目](#programs)
* [环境](#environments)
* [报告](#reports)
* [CI/CD生产管道](#cicd-production-pipeline)
* [CI/CD非生产管道](#cicd-non-production-pipeline)
* [活动](#activity)

有关完整的概述，请查看 [Cloud Manager用户指南](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## 项目 {#programs}

[Cloud Manager程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) 表示支持逻辑业务计划集的AEM环境集，通常对应于购买的服务级别协议(SLA)。 例如，一个项目可以表示支持全球公共网站的AEM资源，而另一个项目则表示内部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 环境 {#environments}

[Cloud Manager环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) 由AEM创作、AEM发布和调度程序实例组成。 不同的环境支持角色，并且可以使用不同的CI/CD管线参与（如下所述）。 Cloud Manager环境通常具有一个生产环境和一个阶段环境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 报告 {#reports}

[Cloud Manager报表](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) 通过一组图表来提供对程序环境和AEM实例的查看，这些图表报告并跟踪每个AEM实例的各种量度。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD生产管道 {#cicd-production-pipeline}

*[在Adobe Cloud Manager中使用CI/CD管道Adobe](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 视频系列深入介绍了生产管道执行，包括对失败和成功部署的探索。*

>[!NOTE]
>
> 在这些视频中，构建、测试和部署时间都得到了加快，缩短了视频播放的时间。 根据项目大小、AEM实例数和UAT进程，完成管道执行通常需要45分钟或更长时间（包括强制性的30分钟性能测试）。

### 配置

的 [CI/CD生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 配置定义启动管道的触发器以及控制生产部署和性能测试参数的参数。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管道执行

的 [CI/CD生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) 用于通过Stage构建代码并将其部署到生产环境，从而缩短实现价值的时间。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生产管道 {#cicd-non-production-pipeline}

[CI/CD非生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 分为两类：代码质量管道和部署管道。 代码质量会从Git分支中管道所有代码，以根据Cloud Manager的代码质量扫描构建和评估这些代码。 部署管道支持将代码从Git存储库自动部署到任何非生产环境，这意味着任何非暂存或生产环境的已配置AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活动 {#activity}

Cloud Manager提供了对项目活动的整合视图，其中列出了所有CI/CD管道执行（包括生产和非生产），从而可以查看过去和当前活动的情况，并且可以查看任何活动的详细信息。

Cloud Manager还在每用户级别与 [Adobe Experience Cloud通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html)，提供对所关注事件和行动的全方位视图。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
