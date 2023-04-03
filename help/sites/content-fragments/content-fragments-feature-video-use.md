---
title: 在AEM中创作内容片段
description: 内容片段是AEM中的内容抽象，它允许独立于它支持的渠道来创作和管理基于文本的内容。
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: Cloud Service
topic: Content Management
role: User
level: Beginner
exl-id: d33c033a-9577-4d4e-99be-f3c7e2a4ce73
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 17%

---

# 创作内容片段 {#authoring-content-fragments}

内容片段是AEM中的内容抽象，它允许独立于它支持的渠道来创作和管理基于文本的内容。

AEM内容片段是基于文本的编辑内容，可能包含一些与之关联但被视为纯内容的结构化数据元素，而不包含设计或布局信息。 内容片段通常创建为与渠道无关的内容，旨在跨渠道使用和重复使用，进而将内容包装在特定于上下文的体验中。

此视频系列涵盖AEM中内容片段的创作生命周期。 有关 [可在此处找到交付内容片段](content-fragments-delivery-feature-video-use.md).

1. 启用和定义内容片段模型
2. 创作内容片段
3. 下载内容片段
4. 编辑能力

>[!CONTEXTUALHELP]
>id="aemcloud_sites_admin_content_fragments"
>title="管理片段"
>abstract="了解如何使用内容片段来设计、创建、管理和使用独立于页面的内容。"

## 定义内容片段模型 {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452?quality=12&learn=on)

AEM内容片段模型（内容片段的数据架构）必须通过AEM启用 [[!UICONTROL 配置浏览器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)，允许根据每个配置定义内容片段模型。

## 创建内容片段 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451?quality=12&learn=on)

AEM配置会应用于AEM Assets文件夹层次结构，以允许将其内容片段模型创建为内容片段。 内容片段支持基于表单的丰富创作体验，允许将内容建模为元素集合。

内容片段可以有多个变体，每个变体都针对内容的不同用例（思路不一定是渠道）。

*导入的运动员传记示例：*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## 下载内容片段 {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450?quality=12&learn=on)

AEM内容片段可以从AEM作者中下载为包含变体、元素和元数据的Zip文件。

*内容片段下载Zip文件示例：*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## 内容片段编辑功能 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891?quality=12&learn=on)

>[!NOTE]
>
> 在 [AEM 6.4 Service Pack 2](https://helpx.adobe.com/cn/experience-manager/aem-releases-updates.html) 和 [AEM 6.3 Service Pack 3](https://helpx.adobe.com/cn/experience-manager/6-3/release-notes/sp3-release-notes.html).

## 后续步骤

了解 [交付内容片段](content-fragments-delivery-feature-video-use.md).

## 其他资源 {#additional-resources}

* [交付内容片段](content-fragments-delivery-feature-video-use.md)
* [AEM WCM 核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心内容片段组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans)

要在AEM 6.4+实例上下载并安装以下包，以获取视频系列的最终状态，请执行以下操作：

**[aem_demo_fluid-experiencecontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
