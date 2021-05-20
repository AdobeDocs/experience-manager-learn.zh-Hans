---
title: 将源文件翻译与AEM Assets结合使用
description: Adobe Experience Manager(AEM)Assets允许您使用新的“相关资产”功能识别共享常用属性的资产，并将其标记为相关资产。 它还允许用户定义资产之间的源/派生关系，以便用户轻松识别资产的来源。 对派生资产运行翻译工作流会获取源文件引用的任何资产，并将其包含在内以进行翻译，从而减少维护多站点的工作。
version: 6.3, 6.4, 6.5
topic: 内容管理
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---


# 将源文件翻译与AEM Assets {#using-source-file-translation-with-aem-assets}结合使用

Adobe Experience Manager(AEM)Assets允许您使用新的“相关资产”功能识别共享常用属性的资产，并将其标记为相关资产。 它还允许用户定义资产之间的源/派生关系，以便用户轻松识别资产的来源。 对派生资产运行翻译工作流会获取源文件引用的任何资产，并将其包含在内以进行翻译，从而减少维护多站点的工作。

## 多站点资产源文件管理{#multisite-asset-source-file-management}

>[!VIDEO](https://video.tv.adobe.com/v/18331/?quality=9&learn=on)

相关资产可帮助用户通过共享特征、属性来更好地管理跨链接资产，并简化工作流程：

* 新的“相关资产”功能，可手动关联具有相似特征或属于同一营销活动或项目的资产
* 用户可以在查看属性下查看资产的相关文件。 用户可以从“查看属性”窗口导航到相关文件。
* 如果两个相关资产的属性发生更改，用户可以使用“取消关联”选项取消这些资产的关联。
* 当您尝试删除相关资产时，如果该资产具有其他相关资产，则会收到一条警告消息。
* 在派生的相关资产上运行翻译工作流，将相关源文件添加到翻译工作流，从而轻松进行多站点管理。