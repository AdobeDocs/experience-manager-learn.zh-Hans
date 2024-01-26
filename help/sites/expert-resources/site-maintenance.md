---
title: 日常站点维护指南
description: 无论您是管理员、作者还是开发人员，站点维护都会涉及AEM Sites实例的每个方面。 使用本指南可确保设置策略以取得成功。
role: Admin
level: Beginner, Intermediate
topic: Administration
feature: Learn From Your Peers
jira: KT-14255
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
duration: 255
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---

# 站点维护提示和技巧

安装和维护AEM实例时，有三个选项

* AEMaaCS（云服务） — 系统始终运行、保持最新状态，并根据需要动态扩展
* AdobeManaged Services，Adobe客户服务工程师在这里进行所有每日/每周/每月的维护，并确保所有Service Pack均已安装，并且系统始终安全且运行顺畅
* 在本地运行，您需要负责整个系统，包括备份、升级和安全性。

如果您选择在内部实施自己的系统，请谨记以下一些事项，以确保您拥有一个安全、高性能系统。 除了“关怀和馈送”项目外，本文还指出了AEM开发人员应该注意的一些项目来帮助系统保持良好的运行。

## 管理员

备份 — 确保您按频繁的时间表执行完整和/或部分备份：

* 每日
* 每周
* 每月

许多客户执行快照备份，如果基础操作系统支持此类备份，则仅需几分钟的时间。 确保正确地存储了这些备份(在AEM系统外)。 确保备份正常运行，并可用于定期重新创建正常运行的系统 — 没有什么比发生系统崩溃和备份由于某种原因损坏更糟的了！

您需要监视几个项目以确保无故障运行：

### 日常维护

#### [索引维护](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=en)

索引允许查询尽可能快地运行，从而腾出资源用于其他操作。 确保索引处于顶部形状！ AEM会取消遍历的查询，而不是使用索引来防止一个错误的查询影响整体AEM性能。

#### [Tar压缩/修订版清理](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

存储库的每次更新都会创建一个新的内容修订版本。 因此，存储库的大小会随着每次更新而增长。 为避免存储库增长失控，需要清理旧修订以释放磁盘资源。

#### [Lucene二进制文件清理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

清除Lucene二进制文件并减少正在运行的数据存储大小要求。

#### [数据存储垃圾桶](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

删除AEM中的资产时，可能会从节点层次结构中删除对基础数据存储记录的引用，但数据存储记录本身会保留。 此未引用的数据存储记录会变为“垃圾”，无需保留。 如果存在大量未引用的资产，则将其删除非常有用，可以保留空间，优化备份和文件系统维护性能。

#### [工作流清除](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html)

最大限度地减少工作流实例的数量可以提高工作流引擎的性能，因此，您可以定期从存储库中清除已完成或正在运行的工作流实例。

#### [审核日志维护](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

符合审核日志记录条件的AEM事件会生成大量存档数据。 由于复制、资产上传和其他系统活动，这些数据会随着时间的推移而快速增长。

#### [安全性](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

确保严格遵循安全清单最佳实践，以确保AEM的最安全实例。

#### 磁盘空间

监控磁盘空间，确保您有足够的空间来使用JCR存储库，并再增加大约一半的空间 — tar压缩在运行时会占用额外的空间。 磁盘空间不足是JCR损坏的首要原因！

## 开发人员

尝试不使用自定义组件 — 使用 [核心组件](https://www.aemcomponents.dev/). 您的目标应该是80-90%的时间使用核心组件，并且只谨慎使用自定义组件。 这通常需要一种新的查看页面上组件的方式 — 您必须意识到前端开发人员可以使用CSS轻松重新设置组件的样式。 同时请记住，这些核心组件可以相互嵌入，以实现非常复杂的结果。 发挥创意！

### [样式系统](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

样式系统允许核心组件，甚至自定义组件，其外观和感觉会根据作者的判断而发生变化，以创建全新的组件。 这些文体变化通常只涉及前端设计人员和知识丰富的作者（通常称为“超级作者”）

### [启动项](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

启动项允许完成新的促销活动、销售或网站转出的工作，而不影响当前部署的页面。 此外，它们可以计划为自动上线，而无需出席或监督，这样作者就可以今天完成下周（或下一季度）的工作，而不用在上线前一天就匆忙进行页面开发 — 这真是“时间”的天赋！)

### [内容片段](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

内容片段是可自定义的信息的“块”，可以轻松地在网站上重复使用。 如果您需要更改，只需更改原始块，并且更新会出现在任何使用它的位置 — 立即！

### [体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

虽然体验片段的声音与内容片段几乎相同，但体验片段是细小的可见页面片段。 它们还可以在您的站点中广泛重复使用，并在AEM的中央位置进行维护，以简化在几秒钟内对站点进行潜在全局更改的任务，而无需几天或几周的时间。

提前思考，看看哪些内容可以重复使用。 页脚？ 免责声明？ 标题？ 特定类型的内容？ 所有这些内容可以在整个站点中共享，同时尽量避免维护。 需要更新免责声明中的日期，但它位于您网站上的1,000个页面上？ 如果您使用体验片段，它需要5秒的操作！

## 常规

通过持续学习及时了解更改AEM — 不要沉迷于过去。 使用 [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) 和 [Adobe数字学习服务(ADLS)](https://learning.adobe.com/) 磨练你的技能。

## 结论

AEM可以是一个很大的系统，它需要很多类型的人才能让它“唱起来”。 从管理员到开发人员（前端和核心Java开发人员）再到作者 — 每个人都有自己的东西！ 如果您不想处理日常管理，可以随时使用AMS和AEMas a Cloud Service。
