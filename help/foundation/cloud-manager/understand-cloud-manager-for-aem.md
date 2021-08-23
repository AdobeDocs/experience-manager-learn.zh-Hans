---
title: 了解AdobeCloud Manager
description: AdobeCloud Manager提供了简单而强大的解决方案，允许轻松管理、反省和自助服务AEM环境。
sub-product: cloud manager， foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: 架构
role: Architect
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# 了解AdobeCloud Manager

AdobeCloud Manager提供了简单而强大的解决方案，允许轻松管理、反省和自助服务AEM环境。

## Cloud Manager概述

本视频系列探讨了Cloud Manager for AEM的主要功能，包括：

* [程序](#programs)
* [环境](#environments)
* [报告](#reports)
* [CI/CD生产管道](#cicd-production-pipeline)
* [CI/CD非生产管道](#cicd-non-production-pipeline)
* [活动](#activity)

有关完整的概述，请参阅[Cloud Manager用户指南](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=zh-Hans)。

## 程序 {#programs}

[Cloud Manager程](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) 序表示一组支持业务计划逻辑集的AEM环境，通常对应于购买的服务级别协议(SLA)。例如，一个项目可以表示支持全球公共网站的AEM资源，而另一个项目则表示内部中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 环境 {#environments}

[Cloud Manager环](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) 境由AEM创作、AEM发布和调度程序实例组成。不同的环境支持角色，并且可以使用不同的CI/CD管线参与（如下所述）。 Cloud Manager环境通常具有一个生产环境和一个阶段环境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 报告 {#reports}

[Cloud Manager报](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) 表通过一组图表提供对项目环境和AEM实例的视图，这些图表报告并跟踪每个AEM实例的各种量度。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD生产管道 {#cicd-production-pipeline}

*[在Cloud Manager Video系列中使用CI/CD管](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 道可深入了解生产管道执行，包括探索失败和成功的部署。*

>[!NOTE]
>
> 在这些视频中，构建、测试和部署时间已加快，可缩短视频的时间。 根据项目大小、AEM实例数和UAT进程，完成管道执行通常需要45分钟或更长时间（包括强制性的30分钟性能测试）。

### 配置

[CI/CD生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html)配置定义将启动管道的触发器、控制生产部署和性能测试参数的参数。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管道执行

[CI/CD生产管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/deploying-code.html)用于通过暂存构建代码并将其部署到生产环境，从而缩短实现价值的时间。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生产管道 {#cicd-non-production-pipeline}

[CI/CD非生产管](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) 道分为两类：代码质量管道和部署管道。代码质量会从Git分支中管道所有代码，以根据Cloud Manager的代码质量扫描构建和评估这些代码。 部署管道支持将代码从Git存储库自动部署到任何非生产环境，这意味着任何非暂存或生产的已配置AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活动 {#activity}

Cloud Manager提供了对项目活动的整合视图，其中列出了所有CI/CD管道执行（包括生产和非生产），从而可以查看过去和当前活动的情况，并且可以查看任何活动的详细信息。

Cloud Manager还在每用户级别与[Adobe Experience Cloud通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/notifications.html)集成，提供了对所关注事件和操作的全方位视图。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
