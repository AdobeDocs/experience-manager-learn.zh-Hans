---
title: 第3章——创作事件内容片段
seo-title: AEM内容服务入门——第3章——创作事件内容片段
description: AEM无头教程的第3章涵盖从第2章创建的内容片段模型中创建和创作事件内容片段。
seo-description: AEM无头教程的第3章涵盖从第2章创建的内容片段模型中创建和创作事件内容片段。
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---


# 第3章——创作事件内容片段

AEM无头教程的第3章涵盖从第2章创建的内容片段模型创建和创作事件内 [容片段](./chapter-2.md)。

## 创作事件内容片段

创建内 [!DNL Event] 容片段模型并将WKND的AEM配置应用到资产 `/content/dam/wknd-mobile` 文件夹(通过属 `cq:conf` 性)后，可以 [!DNL Event] 创建内容片段。

内容片段是一种资产类型，与其他资产一样，应在AEM Assets组织和管理。

* 如果需要转换（或可能需要转换），请在“资产”文件夹结构中使用区域设置文件夹
* 按逻辑组织内容片段，以便轻松进行定位和管理

在此步骤中，请为资 [!DNL Event] 产文 `Punkrock Fest` 件夹创 `/content/dam/wknd-mobile/en/events` 建新。

1. 导航到AEM **>资[!UICONTROL 产]>Files[!DNL WKND Mobile]>[!DNL English]****[!DNL Events]**&#x200B;创建资产文件夹。
1. 在“ **[!UICONTROL 资产]” >[!UICONTROL Files]>[!DNL WKND Mobile]>[!DNL English]>创建[!DNL Events]****[!DNL Event]****[!DNL Punkrock Fest]**&#x200B;标题为的新的内容片段。
1. 创作新创建的 [!DNL Event] 内容片段。

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;输入几行说明……>**
   * [!DNL Event Date] : **&lt;选择将来的日期>**
   * [!DNL Event Type] : **音乐**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **爬虫屋**
   * [!DNL Venue City] : **New York**

   点按 **[!UICONTROL 顶部]** 操作栏中的“保存”以保存更改。

1. 使 [用AEM Package](http://localhost:4502/crx/packmgr/index.jsp)Manager，在AEM Author上安装以下包。 此包包含许多事件内容片段。

   [获取文件：GitHub >资产> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## 查看内容片段的JCR结构

*此部分仅供参考，旨在使内容片段模型生成的内容片段的基础JCR结构社交化。*

1. 在AEM **[作者](http://localhost:4502/crx/de/index.jsp)** 上打开CRXDE Lite。
1. 在CRXDE Lite中，在左侧的层次结构菜单中，导 [航到/content/dam/wknd-mobile/en/事件/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) ，该节点表示JCR [!DNL Punkrock Fest] 中的内 [!DNL Event] 容片段。
1. 展开数 [据节](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 点。
在“属性 **”窗格中** ，查看它是否具有 `cq:model` 指向内容片段模 [!DNL Event] 型定义的属性。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. 在节点 `data` 下选择 [主控](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) 节点并查看属性。 此节点包含在创作内容片段模型时收 [!DNL Event] 集的内容。 JCR属性名称与内容片段模型属性名称相对应，且这些值与创作的“”内容片段[!DNL Punkrock Fest]的 [!DNL Event] 值相对应。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 下一步

建议您通过AEM [包管理器在AEM作者上安装com.adobe.aem.guides.wknd-mobile.content.chapter](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) -3.zip内 [容 [!UICONTROL 包]](http://localhost:4502/crx/packmgr/index.jsp)。 此包包含教程本章和前几章中概述的配置和内容。

* [第4章——定义AEM Content Services模板](./chapter-4.md)
