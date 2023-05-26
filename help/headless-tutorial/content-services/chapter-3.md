---
title: 第3章 — 创作事件内容片段 — 内容服务
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: AEM Headless教程的第3章介绍了如何根据在第2章中创建的内容片段模型创建和创作事件内容片段。
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 2%

---

# 第3章 — 创作事件内容片段

AEM Headless教程的第3章介绍了如何从在中创建的内容片段模型创建和创作事件内容片段 [第2章](./chapter-2.md).

## 创作事件内容片段

带有 [!DNL Event] 已创建内容片段模型并将WKND的AEM配置应用于 `/content/dam/wknd-mobile` 资产文件夹(通过 `cq:conf` property)， a [!DNL Event] 可以创建内容片段。

内容片段是一种资源，应当与其他资源一样，在AEM Assets中对其进行组织和管理。

* 如果需要翻译，请在Assets文件夹结构中使用区域设置文件夹
* 以逻辑方式组织内容片段，使其易于查找和管理

在此步骤中，创建一个新的 [!DNL Event] 对象 `Punkrock Fest` 在 `/content/dam/wknd-mobile/en/events` 资源文件夹。

1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] >[!DNL English]** 和创建资产文件夹 **[!DNL Events]**.
1. 范围 **[!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 创建类型为的新内容片段 **[!DNL Event]** 标题为 **[!DNL Punkrock Fest]**.
1. 创作新创建的 [!DNL Event] 内容片段。

   * [!DNL Event Title] ： **[!DNL Punkrock Fest]**
   * [!DNL Event Description] ： **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] ： **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] ： **音乐**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] ： **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] ： **爬行动物之家**
   * [!DNL Venue City] : **New York**

   点按 **[!UICONTROL 保存]** 以保存更改。

1. 使用 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)，在AEM作者上安装以下包。 此包包含许多事件内容片段。

   [获取文件： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 查看内容片段的JCR结构

*本节仅供参考，其目的是使从内容片段模型生成的内容片段的基础JCR结构社会化。*

1. 打开 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** 在AEM创作中。
1. 在CRXDE Lite的左侧层次结构菜单中，导航到 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr：content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) ，即表示以下内容的节点： [!DNL Punkrock Fest] [!DNL Event] JCR中的内容片段。
1. 展开 [数据](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 节点。
在中查看 **“属性”窗格** 它有一个属性 `cq:model` 这表明 [!DNL Event] 内容片段模型定义。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在 `data` 节点选择 [主控](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 节点并查看属性。 此节点包含创作期间收集的内容 [!DNL Event] 内容片段模型。 JCR属性名称对应于内容片段模型属性名称的值，而值对应于&quot;[!DNL Punkrock Fest]” [!DNL Event] 内容片段。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 下一步

建议您安装 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的内容包，通过 [AEM [!UICONTROL 包管理器]](http://localhost:4502/crx/packmgr/index.jsp). 此资源包包含本教程及前面章节中概述的配置和内容。

* [第4章 — 定义AEM Content Services模板](./chapter-4.md)
