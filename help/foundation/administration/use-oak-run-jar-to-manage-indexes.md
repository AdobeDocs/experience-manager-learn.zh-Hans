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
source-wordcount: '449'
ht-degree: 0%

---


# 使用oak-run.jar管理索引

[!DNL oak-run.jar]&#39;s index命令整合了多个功能，用于管理AEM中 [!DNL Oak]的200个索引，包括收集索引统计、运行索引一致性检查以及重新编制索引本身。

>[!NOTE]
>
>在本文和视频中，术语索引和重新索引可互换使用，并视为同一操作。

## [!DNL oak-run.jar] 索引命令基础知识

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用的[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)版本必须与AEM实例上使用的Oak版本匹配。
* 使用[!DNL oak-run.jar]管理索引将&#x200B;**[!DNL index]**&#x200B;命令与各种标志结合，以支持不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引统计

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 转储所有索引定义、重要索引状态和索引内容，以便脱机分析。
* 索引统计信息收集在使用中的AEM实例上是安全的。

## 索引一致性检查

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` 快速确定lucene Oak索引是否损坏。
* 一致性检查在使用中的AEM实例上运行是安全的，用于一致性检查级别1和2。

## 具有[!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}的TarMK在线索引

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]的[!DNL TarMK]的联机索引比在`oak:queryIndexDefinition`节点上设置`reindex=true`的速度快。 尽管性能有所提高，但使用[!DNL oak-run.jar]的联机索引仍然需要一个维护窗口来执行索引。

* 使用[!DNL oak-run.jar]的[!DNL TarMK]的在线索引应&#x200B;**不**&#x200B;对AEM实例维护窗口外的AEM实例执行。

## 使用oak-run.jar建立TarMK脱机索引

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]的[!DNL TarMK]的脱机索引是[!DNL TarMK]最简单的基于[!DNL oak-run.jar]的索引方法，因为它需要单个[!DNL oak-run.jar]命令，但它需要关闭AEM实例。

## 使用oak-run.jar建立TarMK带外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]在[!DNL TarMK]上建立带外索引可最大限度地减少索引对使用中的AEM实例的影响。
* 带外索引是AEM安装的推荐索引方法，其中重新／索引时间超过可用维护窗口。

## MongoMK使用oak-run.jar在线索引

* 在[!DNL MongoMK]和[!DNL RDBMK]上具有[!DNL oak-run.jar]的联机索引是重新／索引[!DNL MongoMK]（和[!DNL RDBMK]）AEM安装的推荐方法。 **不应使用其他方 [!DNL MongoMK] 法 [!DNL RDBMK]。**
* 只需对群集中的单个AEM实例执行此索引。
* [!DNL MongoMK]的在线索引安全地针对正在运行的AEM群集执行，因为存储库遍历仅在单个[!DNL MongoDB]节点上进行，从而使其他节点能够继续提供请求，而不会对性能产生重大影响。

用于执行[!DNL MongoMK]在线索引的[!DNL oak-run.jar]索引命令与 [!DNL TarMK] 在线索引的[相同，其差别在于段存储参数指向包含节点存储的[!DNL MongoDB]实例。 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)

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
