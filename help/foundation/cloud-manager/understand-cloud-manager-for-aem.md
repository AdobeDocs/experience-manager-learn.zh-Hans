---
title: 了解Adobe Cloud Manager
description: Adobe Cloud Manager提供简单而可靠的解决方案，可轻松管理、检查和自助服务AEM环境。
sub-product: 云管理器，基础
feature: 管道、项目、项目、质量门、报告
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 3%

---


# 了解Adobe Cloud Manager

Adobe Cloud Manager提供简单而可靠的解决方案，可轻松管理、检查和自助服务AEM环境。

## Cloud Manager概述

此视频系列探讨了Cloud Manager的AEM版主要功能，包括：

* [程序](#programs)
* [环境](#environments)
* [报告](#reports)
* [CI/CD生产管道](#cicd-production-pipeline)
* [CI/CD非生产管道](#cicd-non-production-pipeline)
* [活动](#activity)

有关完整概述，请查阅[Cloud Manager用户指南](https://docs.adobe.com/content/help/zh-Hans/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)。

## 程序 {#programs}

[Cloud Manager计](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) 划是支持业务计划逻辑集的AEM环境集，通常对应于购买的服务级别协议(SLA)。例如，一个项目可以表示支持全球公共网站的AEM资源，而另一个项目则表示内部的中央DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 环境 {#environments}

[Cloud Manager环境](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) 由AEM作者、AEM发布和调度程序实例组成。不同环境支持角色，可使用不同的CI/CD管道（如下所述）参与。 Cloud Manager环境通常具有一个生产环境和一个阶段环境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 报告 {#reports}

[Cloud Manager报](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) 表通过一组图表向项目的环境和AEM实例提供视图，这些图表报告并跟踪每个AEM实例的各种量度。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD生产管道{#cicd-production-pipeline}

*[使用Adobe Cloud Manager Video系列中的CI/CD](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 管道可深入探索生产管道执行，包括探索失败和成功的部署。*

>[!NOTE]
>
> 在这些视频中，构建、测试和部署时间都得到了加快，缩短了视频播放时间。 根据项目大小、AEM实例数和UAT进程，完成管道执行通常需要45分钟或更长时间（包括强制的30分钟性能测试）。

### 配置

[CI/CD生产管线](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html)配置定义将启动管线的触发器、控制生产部署和性能测试参数的参数。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管道执行

[CI/CD生产管道](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html)用于通过Stage构建代码并将代码部署到生产环境，从而缩短了实现价值的时间。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生产管道{#cicd-non-production-pipeline}

[CI/CD非生产管道](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) 分为两个类别，代码质量管道和部署管道。“代码质量”可以管道Git分支中的所有代码，以根据Cloud Manager的代码质量扫描构建和评估。 部署渠道支持将代码从Git存储库自动部署到任何非生产环境，即任何非舞台或生产的预配AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活动 {#activity}

Cloud Manager为项目的活动提供整合视图，列出所有CI/CD管道执行（生产和非生产），让您能够了解过去和现在的活动，并且可以查看任何活动的详细信息。

Cloud Manager还以每用户级别与[Adobe Experience Cloud Notifications](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)集成，为事件和相关操作提供无所不在的视图。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
