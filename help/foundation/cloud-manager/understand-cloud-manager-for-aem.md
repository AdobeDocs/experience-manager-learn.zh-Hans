---
title: 了解AdobeCloud Manager
description: AdobeCloud Manager提供了一个简单但强大的解决方案，允许对AEM环境进行轻松管理、内部检查和自助服务。
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1011
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 15%

---

# 了解AdobeCloud Manager

AdobeCloud Manager提供了一个简单但强大的解决方案，允许对AEM环境进行轻松管理、内部检查和自助服务。

## Cloud Manager 概述

本视频系列探讨Cloud Manager为AEM提供的主要功能，包括：

* [项目](#programs)
* [环境](#environments)
* [报告](#reports)
* [CI/CD 生产管道](#cicd-production-pipeline)
* [CI/CD非生产管道](#cicd-non-production-pipeline)
* [活动](#activity)

有关完整概述，请查阅[Cloud Manager用户指南](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html)。

## 项目 {#programs}

[Cloud Manager程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html)表示支持业务计划逻辑集的AEM环境集，通常与购买的服务水平协议(SLA)相对应。 例如，一个程序可能代表支持全球公共网站的AEM资源，而另一个程序代表内部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## 环境 {#environments}

[Cloud Manager环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html)由AEM Author、AEM Publish和Dispatcher实例组成。 不同的环境支持各种角色，并且可以使用不同的CI/CD管道参与环境（如下所述）。 Cloud Manager环境通常有一个生产环境和一个暂存环境。

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## 报告 {#reports}

[Cloud Manager报表](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html)通过一组图表提供项目群环境和AEM实例的视图，这些图表报告和跟踪每个AEM实例的各种指标。

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD 生产管道 {#cicd-production-pipeline}

*[在AdobeCloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)视频系列中使用CI/CD管道可深入了解生产管道的执行，包括探索失败和成功的部署。*

>[!NOTE]
>
> 通过这些视频，加快了构建、测试和部署时间，从而减少了视频时间。 根据项目大小、AEM实例数和UAT流程，完整的管道执行通常需要45分钟或更长时间（包括强制性的30分钟性能测试）。

### 配置

[CI/CD生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html)配置定义启动管道的触发器，以及控制生产部署和性能测试参数的参数。

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### 管道执行

[CI/CD生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html)用于通过暂存生成代码并将其部署到生产环境，从而缩短实现值的时间。

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD非生产管道 {#cicd-non-production-pipeline}

[CI/CD非生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html)分为两个类别：代码质量管道和部署管道。 代码质量管道从 Git 分支获取所有代码以生成并对照 Cloud Manager 的代码质量扫描接受评估。部署管道支持将代码从 Git 存储库自动部署到任意非生产环境，这意味着任何已配置的 AEM 环境不是暂存环境或生产环境。

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## 活动 {#activity}

Cloud Manager提供了项目活动的综合视图，其中列出了所有用于生产环境和非生产环境的CI/CD管道执行，并允许查看过去和现在的活动，并且可以查看任何活动的详细信息。

Cloud Manager还在每个用户级别与[Adobe Experience Cloud通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html)集成，从而提供感兴趣的事件和操作的全景视图。

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
