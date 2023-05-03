---
title: 日常站点维护指南
seo-title: Your Routine Site Maintenance Guide
description: 无论您是管理员、作者还是开发人员，站点维护都会处理您的AEM Sites实例的各个方面。 使用本指南可确保您的策略已设置为成功。
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 5%

---

# 网站维护提示和技巧

安装和维护AEM实例时有三个选项

* AEMaCS（云服务） — 系统始终处于开启状态，处于最新状态，并可根据需要动态缩放
* Adobe Managed Services，Adobe客户服务工程师可以执行所有每日/每周/每月维护，并确保安装了所有Service Pack，并且系统始终安全且运行顺畅
* 在本地运行它，您必须负责整个系统，包括备份、升级和安全。

如果您选择在本地实施您自己的系统，请牢记以下几点，以确保您拥有安全、高性能的系统。 除了“关怀和馈送”项目外，本文还将指出AEM开发人员应牢记的几个项目，以帮助系统保持良好运行。

## 管理员

备份 — 确保您有频繁的完整和/或部分备份：

* 每日
* 每周
* 每月

许多客户执行快照备份，假定底层操作系统支持此类备份，则只需几分钟即可完成。 确保正确存储这些备份(在AEM系统之外)。 确保备份正常工作，并且可用于定期重新创建工作系统 — 没有什么比系统崩溃更糟的了，而且您的备份因某种原因而损坏了！

您需要监视以下几项以确保无故障运行：

### 日常维护

#### [索引维护](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=zh-Hans)

索引允许查询尽快运行，从而腾出资源用于其他操作。 确保索引处于顶端形状！ AEM会取消travere的查询，而不是使用索引来保留一个错误查询，以免影响AEM的整体性能。

#### [焦油压缩/修订清理](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

对存储库的每次更新都会创建新的内容修订版本。 因此，每次更新时，存储库的大小都会增大。 为避免存储库增长失控，需要清理旧的修订版本以释放磁盘资源。

#### [Lucene二进制文件清理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

清除Lucene二进制文件并减少正在运行的数据存储大小要求。

#### [数据存储垃圾](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

删除AEM中的资产后，可能会从节点层次结构中删除对基础数据存储记录的引用，但数据存储记录本身会保留。 此未引用的数据存储记录将变为“垃圾”，无需保留。 如果存在大量未引用的资产，则最好删除这些资产，保留空间，优化备份和文件系统维护性能。

#### [工作流清除](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html)

最大限度地减少工作流实例的数量可以提高工作流引擎的性能，因此，您可以定期从存储库中清除已完成或正在运行的工作流实例。

#### [审核日志维护](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html)

符合审核日志记录条件的AEM事件会生成大量存档数据。 由于复制、资产上传和其他系统活动，此数据会随着时间而快速增长。

#### [安全性](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=zh-Hans)

确保密切遵循安全检查列表最佳实践，以确保最安全的AEM实例。

#### 磁盘空间

监控磁盘空间，以确保您有足够的空间用于JCR存储库，另外，还需要大约一半的空间 — 焦油压缩在运行时会占用额外的空间。 磁盘空间不足是JCR损坏的首要原因！

## 开发人员

尝试不使用自定义组件 — 使用 [核心组件](https://www.aemcomponents.dev/). 您的目标应是仅谨慎使用80-90%的核心组件和自定义组件。 这通常需要一种查看页面上组件的新方法 — 您必须意识到前端开发人员可以使用CSS轻松地对组件进行重新设置样式。 另外，还要记住，这些核心组件可以相互嵌入，以实现相当复杂的结果。 有创意！

### [样式系统](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

样式系统允许核心组件（甚至自定义组件）根据作者的决定对其外观进行更改，以创建全新的外观组件。 这些风格变化通常只涉及前端设计师和知识渊博的作者（通常称为“超级作者”）

### [启动项](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

启动项允许完成新的促销、销售或网站推广工作，而不会影响当前部署的页面。 此外，它们可以被安排自动上线，而无需出席或监督，从而允许作者今天完成下周（或下季度）的工作，并且不会在页面开发开始前一天匆忙进行页面开发 — 这真的是TIME的礼物！)

### [内容片段](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

内容片段是可自定义的“信息块”，可在整个站点上轻松重复使用。 如果需要更改，只需更改原始块，更新即可在所有使用的地方看到 — 立即！

### [体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

体验片段听起来与内容片段几乎相同，但却只是一个页面的小块、可见的片段。 这些功能还可以在您的网站中广泛重复使用，并在AEM的中心位置进行维护，以简化在几秒（而非几天或几周）内对网站进行潜在全局更改的任务。

请先想一想，看看哪些内容可以重复利用。 页脚？ 免责声明？ 标题？ 某些类型的内容？ 所有这些内容都可以在整个站点之间共享，同时至少保持维护。 需要在免责声明中更新日期，但该日期位于您网站的1,000个页面上？ 如果您使用体验片段，则需要5秒钟的操作！

## 常规

通过不断学习掌握AEM的变化 — 不要被困在过去。 使用 [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) 和 [Adobe数字学习服务(ADLS)](https://learning.adobe.com/) 磨练你的技能。

## 结论

AEM可以是一个大型系统，需要多种类型的人员才能使其“歌唱”。 从管理员到开发人员（前端和硬核Java开发人员），再到作者 — 每个人都能从中受益！ 如果您不想处理日常管理工作，则始终会有AMS和AEMas a Cloud Service。
