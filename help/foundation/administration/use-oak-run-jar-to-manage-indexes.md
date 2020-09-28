---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了用于管理AEM中Oak索引的多项功能，包括收集索引统计信息、运行索引一致性检查以及自行重新编制索引。
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# 使用oak-run.jar管理索引

[!DNL oak-run.jar]&#39;s index命令整合了多个功能，用于管理AEM中 [!DNL Oak]的200个索引，包括收集索引统计、运行索引一致性检查以及重新编制索引本身。

>[!NOTE]
>
>在本文和视频中，术语索引和重新索引可互换使用，并视为同一操作。

## [!DNL oak-run.jar] 索引命令基础知识

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用的 [[!DNL oak-run.jar]版本必须与AEM实例上使用的Oak版本](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 。
* 使用带有各 [!DNL oak-run.jar] 种标志的命 **[!DNL index]** 令来管理索引，以支持不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引统计

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 转储所有索引定义、重要索引状态和索引内容，以便脱机分析。
* 索引统计信息收集在使用中的AEM实例上是安全的。

## 索引一致性检查

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` 快速确定lucene Oak索引是否损坏。
* 一致性检查在使用中的AEM实例上运行是安全的，用于一致性检查级别1和2。

## TarMK Online索引 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 使用的在 [!DNL TarMK] 线索 [!DNL oak-run.jar] 引比在节点上 `reindex=true` 设置的速 `oak:queryIndexDefinition` 度快。 尽管性能有所提高，但使用联机索引 [!DNL oak-run.jar] 仍需要一个维护窗口才能执行索引。

* 不应对AEM实 [!DNL TarMK] 例 [!DNL oak-run.jar] 维护 **窗口** 外的AEM实例执行使用的联机索引。

## 使用oak-run.jar建立TarMK脱机索引

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 脱机索引使 [!DNL TarMK] 用是最 [!DNL oak-run.jar] 简单的基于索引的方 [!DNL oak-run.jar] 法，因为它需要一个命令， [!DNL TarMK][!DNL oak-run.jar] 但它需要关闭AEM实例。

## 使用oak-run.jar建立TarMK带外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用带外索引可以 [!DNL TarMK] 最 [!DNL oak-run.jar] 小化索引对使用中的AEM实例的影响。
* 带外索引是AEM安装的推荐索引方法，其中重新／索引时间超过可用维护窗口。

## MongoMK使用oak-run.jar在线索引

* 在线索引 [!DNL oak-run.jar] ( [!DNL MongoMK] 带开) [!DNL RDBMK] 和是重新／索引（和）AEM [!DNL MongoMK] 安装的 [!DNL RDBMK]推荐方法。 **不应使用其他方[!DNL MongoMK]法或[!DNL RDBMK]。**
* 只需对群集中的单个AEM实例执行此索引。
* 联机索引 [!DNL MongoMK] 安全地针对正在运行的AEM群集执行，因为存储库遍历将仅发生在单个节点上， [!DNL MongoDB] 从而使其他节点能够继续提供请求而不会影响性能。

用于 [!DNL oak-run.jar] 执行联机索引的索引命 [!DNL MongoMK] 令与Online [索引 [!DNL TarMK] 命令相同 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) ，其差异在于段存储参数指向包含Node存储的 [!DNL MongoDB] 实例。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 支持材料

* [下载 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *确保下载的版本与AEM上安装的Oak版本匹配，如上所述*
* [Apache Jackrabbit Oak-run.jar索引命令文档](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
