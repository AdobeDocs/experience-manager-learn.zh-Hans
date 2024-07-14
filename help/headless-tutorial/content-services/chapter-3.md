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

AEM Headless教程的第3章介绍了如何根据在[第2](./chapter-2.md)章中创建的内容片段模型创建和创作事件内容片段。

## 创作事件内容片段

通过创建[!DNL Event]内容片段模型并将WKND的AEM配置应用于`/content/dam/wknd-mobile` Asset文件夹（通过`cq:conf`属性），可以创建[!DNL Event]内容片段。

内容片段是一种资源，应像其他资源一样在AEM Assets中组织和管理。

* 如果需要翻译，请在Assets文件夹结构中使用区域设置文件夹
* 以逻辑方式组织内容片段，使其易于查找和管理

在此步骤中，请在`/content/dam/wknd-mobile/en/events`资源文件夹中为`Punkrock Fest`创建一个新的[!DNL Event]。

1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL 文件] > [!DNL WKND Mobile] >[!DNL English]**，然后创建资产文件夹&#x200B;**[!DNL Events]**。
1. 在&#x200B;**[!UICONTROL Assets] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**&#x200B;内，创建标题为&#x200B;**[!DNL Punkrock Fest]**&#x200B;的类型为&#x200B;**[!DNL Event]**&#x200B;的新内容片段。
1. 创作新创建的[!DNL Event]内容片段。

   * [!DNL Event Title] ： **[!DNL Punkrock Fest]**
   * [!DNL Event Description] ： **&lt;输入几行说明……>**
   * [!DNL Event Date] ： **&lt;选择未来的日期>**
   * [!DNL Event Type] ： **音乐**
   * [!DNL Ticket Price] ： **10**
   * [!DNL Event Image] ： **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] ：**爬虫屋**
   * [!DNL Venue City] ： **纽约**

   点按顶部操作栏中的&#x200B;**[!UICONTROL 保存]**&#x200B;以保存更改。

1. 使用[AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安装以下包。 此包中包含多个事件内容片段。

   [获取文件： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## 查看内容片段的JCR结构

*此部分仅供参考，其目的是使从内容片段模型生成的内容片段的基础JCR结构社会化。*

1. 在AEM创作中打开&#x200B;**[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**。
1. 在CRXDE Lite的左侧层次结构菜单中，导航到[/content/dam/wknd-mobile/en/events/punkrock-fest/jcr：content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)，该节点表示JCR中的[!DNL Punkrock Fest] [!DNL Event]内容片段。
1. 展开[数据](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)节点。
在**属性窗格**&#x200B;中查看它是否具有指向[!DNL Event]内容片段模型定义的属性`cq:model`。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在`data`节点下，选择[主节点](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)节点并查看属性。 此节点包含在创作[!DNL Event]内容片段模型期间收集的内容。 JCR属性名称对应于内容片段模型属性名称，而值对应于“[!DNL Punkrock Fest]”[!DNL Event]内容片段的创作值。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 下一步

建议您通过[AEM的[!UICONTROL 包管理器]](http://localhost:4502/crx/packmgr/index.jsp)在AEM创作实例上安装[com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此资源包包含本教程及前面章节中概述的配置及内容。

* [第4章 — 定义AEM内容服务模板](./chapter-4.md)
