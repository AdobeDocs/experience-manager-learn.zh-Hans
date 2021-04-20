---
title: 第3章 — 创作事件内容片段 — 内容服务
seo-title: AEM内容服务入门 — 第3章 — 创作事件内容片段
description: AEM无头教程的第3章涵盖从在第2章中创建的内容片段模型中创建和创作事件内容片段。
seo-description: AEM无头教程的第3章涵盖从在第2章中创建的内容片段模型中创建和创作事件内容片段。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 2%

---


# 第3章 — 创作事件内容片段

AEM无标题教程的第3章涵盖从在[第2章](./chapter-2.md)中创建的内容片段模型创建和创作事件内容片段。

## 创作事件内容片段

创建[!DNL Event]内容片段模型并将WKND的AEM配置应用到`/content/dam/wknd-mobile`资产文件夹（通过`cq:conf`属性）后，可以创建[!DNL Event]内容片段。

与其他资产一样，内容片段是一种资产，应在AEM Assets中进行组织和管理。

* 如果需要翻译（或可能需要翻译），请在“资产”文件夹结构中使用区域设置文件夹
* 以逻辑方式组织内容片段，以便于查找和管理

在此步骤中，请为`/content/dam/wknd-mobile/en/events` assets文件夹中的`Punkrock Fest`新建一个[!DNL Event]。

1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] >[!DNL English]**&#x200B;并创建资产文件夹&#x200B;**[!DNL Events]**。
1. 在&#x200B;**[!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**&#x200B;中，创建标题为&#x200B;**[!DNL Punkrock Fest]**&#x200B;的类型为&#x200B;**[!DNL Event]**&#x200B;的新内容片段。
1. 创作新创建的[!DNL Event]内容片段。

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **音乐**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **爬虫屋**
   * [!DNL Venue City] : **New York**

   点按顶部操作栏中的&#x200B;**[!UICONTROL 保存]**&#x200B;以保存更改。

1. 使用[AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安装以下包。 此包包含许多事件内容片段。

   [获取文件：GitHub >资产> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 查看内容片段的JCR结构

*此部分仅提供信息，旨在将从内容片段模型生成的内容片段的基础JCR结构社交化。*

1. 打开AEM作者上的&#x200B;**[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**。
1. 在CRXDE Lite中，在左侧的层次结构菜单中，导航到[/content/dam/wknd-mobile/cn/事件/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)，该节点表示JCR中的[!DNL Punkrock Fest] [!DNL Event]内容片段。
1. 展开[data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)节点。
在**“属性”窗格**&#x200B;中查看，该窗格具有指向[!DNL Event]内容片段模型定义的属性`cq:model`。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在`data`节点下方，选择[主控](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)节点并查看属性。 此节点包含在创作[!DNL Event]内容片段模型期间收集的内容。 JCR属性名称与内容片段模型属性名称相对应，这些值与“[!DNL Punkrock Fest]”[!DNL Event]内容片段的创作值相对应。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 下一步

建议通过[AEM [!UICONTROL 包管理器]](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安装[com.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此包包含教程本章和前几章中概述的配置和内容。

* [第4章 — 定义AEM内容服务模板](./chapter-4.md)
