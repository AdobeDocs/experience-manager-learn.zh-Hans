---
title: 了解Adobe云管理器
description: Adobe云管理器提供简单而可靠的解决方案，可轻松管理、检查和自助服务AEM环境。
sub-product: 云管理器，基础
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 3%

---


# 了解Adobe云管理器

Adobe云管理器提供简单而可靠的解决方案，可轻松管理、检查和自助服务AEM环境。

## 云管理器概述

此视频系列探索Cloud Manager for AEM的主要功能，包括：

* [程序](#programs)
* [环境](#environments)
* [报告](#reports)
* [CI/CD生产管道](#cicd-production-pipeline)
* [CI/CD非生产管道](#cicd-non-production-pipeline)
* [活动](#activity)

有关完整概述，请查看《Cloud Manager [用户指南》](https://docs.adobe.com/content/help/zh-Hans/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)。

## 程序 {#programs}

[Cloud Manager项目代表](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) AEM环境集，这些支持业务计划的逻辑集，通常对应于已购买的服务级别协议(SLA)。 例如，一个项目可以代表AEM资源以支持全球公共网站，而另一个项目则代表内部的Central DAM。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 环境 {#environments}

[Cloud Manager环境由](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) AEM作者、AEM发布和调度程序实例组成。 不同的环境支持角色，可使用不同的CI/CD管道（如下所述）参与。 Cloud Manager环境通常具有一个生产环境和一个阶段环境。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 报告 {#reports}

[Cloud Manager Reports](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) 通过一组图表为项目的视图和AEM实例提供环境，这些图表报告并跟踪每个AEM实例的各种指标。

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD Production Pipeline {#cicd-production-pipeline}

*[在AdobeCloud Manager视频系列中使用CI](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)/CD管道可深入探索生产管道执行，包括探索失败的部署和成功的部署。*

>[!NOTE]
>
> 在这些视频中，构建、测试和部署时间都得到了加快，缩短了视频的时间。 完整的管道执行通常需要45分钟或更长时间（包括强制性的30分钟性能测试），具体取决于项目大小、AEM实例数和UAT进程数。

### 配置

CI/ [CD生产管道配置](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) ，定义将启动管道的触发器、控制生产部署和性能测试参数的参数。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 管道执行

CI/ [CD生产管道](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) ，用于通过Stage构建代码并将其部署到生产环境，从而缩短了实现价值的时间。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非生产管道 {#cicd-non-production-pipeline}

[CI/CD非生产管道被分为](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) “代码质量”管道和“部署”管道两个类别。 代码质量将从Git分支中输入所有代码，以根据Cloud Manager的代码质量扫描进行构建和评估。 部署渠道支持将代码从Git存储库自动部署到任何非生产环境，这意味着任何非舞台或生产的经过调配的AEM环境。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 活动 {#activity}

Cloud Manager为项目的活动提供整合视图，列出所有CI/CD管道执行（生产和非生产），让您能够了解过去和现在的活动，并可以查看任何活动的详细信息。

Cloud Manager还在每用户级别与Adobe Experience Cloud通 [知集成](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)，为事件和相关操作提供全方位视图。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
