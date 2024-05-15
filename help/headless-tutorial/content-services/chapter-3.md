---
title: 第3章 — 创作事件内容片段 — 内容服务
description: AEM Headless教程的第3章涵盖了根据在第2章中创建的内容片段模型创建和创作事件内容片段。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# 第3章 — 创作事件内容片段

AEM Headless教程的第3章介绍了如何从在中创建的内容片段模型创建和创作事件内容片段 [第2章](./chapter-2.md).

## 创作事件内容片段

带有 [!DNL Event] AEM已创建的内容片段模型以及应用于 `/content/dam/wknd-mobile` 资产文件夹(通过 `cq:conf` 属性)， a [!DNL Event] 可创建内容片段。

内容片段是一种资源，应像其他资源一样在AEM Assets中组织和管理。

* 如果需要（或可能）翻译，请在Assets文件夹结构中使用区域设置文件夹
* 以逻辑方式组织内容片段，使其易于查找和管理

在此步骤中，创建一个新的 [!DNL Event] 对象 `Punkrock Fest` 在 `/content/dam/wknd-mobile/en/events` 资源文件夹。

1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] >[!DNL English]** 和创建资产文件夹 **[!DNL Events]**.
1. 范围 **[!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** 创建类型为的新内容片段 **[!DNL Event]** 标题为 **[!DNL Punkrock Fest]**.
1. 创作新创建的 [!DNL Event] 内容片段。

   * [!DNL Event Title] ： **[!DNL Punkrock Fest]**
   * [!DNL Event Description] ： **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] ： **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] ： **音乐**
   * [!DNL Ticket Price] ： **10**
   * [!DNL Event Image] ： **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] ： **爬行动物之家**
   * [!DNL Venue City] ： **纽约**

   点按 **[!UICONTROL 保存]** 以保存更改。

1. 使用 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安装以下包。 此包中包含多个事件内容片段。

   [获取文件： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 查看内容片段的JCR结构

*本节仅供参考，其目的是将来自内容片段模型的内容片段的基础JCR结构社会化。*

1. 打开 **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** 在AEM Author上。
1. 在CRXDE Lite的左侧层次结构菜单中，导航到 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr：content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) ，即表示 [!DNL Punkrock Fest] [!DNL Event] JCR中的内容片段。
1. 展开 [数据](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 节点。
在中查看 **“属性”窗格** 它有一个属性 `cq:model` 这表明 [!DNL Event] 内容片段模型定义。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在 `data` 节点选择 [母版](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 节点并查看属性。 此节点包含创作期间收集的内容 [!DNL Event] 内容片段模型。 JCR属性名称对应于内容片段模型属性名称的，而值对应于&#39;&#39;的创作值[!DNL Punkrock Fest]&quot; [!DNL Event] 内容片段。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 下一步

建议您安装 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM创作中的内容包，通过 [AEM [!UICONTROL 包管理器]](http://localhost:4502/crx/packmgr/index.jsp). 此资源包包含本教程及前面章节中概述的配置及内容。

* [第4章 — 定义AEM内容服务模板](./chapter-4.md)
