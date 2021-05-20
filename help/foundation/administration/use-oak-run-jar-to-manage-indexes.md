---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了许多功能，用于管理AEM中的Oak索引，包括收集索引统计信息、运行索引一致性检查以及重新索引索引本身。
version: 6.4, 6.5
feature: 搜索
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: 演出
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# 使用oak-run.jar管理索引

[!DNL oak-run.jar]&#39;s index命令整合了许多功能，用于管理AEM中 [!DNL Oak]的200个索引，包括收集索引统计信息、运行索引一致性检查以及重新索引索引索引本身。

>[!NOTE]
>
>在本文和视频中，术语索引和重新索引可互换使用，并被视为相同的操作。

## [!DNL oak-run.jar] 索引命令基础知识

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* 使用的[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)版本必须与AEM实例中使用的Oak版本匹配。
* 使用[!DNL oak-run.jar]管理索引时，会利用带有各种标记的&#x200B;**[!DNL index]**&#x200B;命令来支持不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引统计

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` 转储用于离线分析的所有索引定义、重要索引统计资料和索引内容。
* 在使用中的AEM实例上执行索引统计信息收集是安全的。

## 索引一致性检查

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` 快速确定lucene Oak索引是否已损坏。
* 在使用中的AEM实例上运行一致性检查级别1和2是安全的。

## 使用[!DNL oak-run.jar]的TarMK在线索引 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]联机索引[!DNL TarMK]比在`oak:queryIndexDefinition`节点上设置`reindex=true`的速度更快。 尽管性能提高了，但使用[!DNL oak-run.jar]的联机索引仍需要维护窗口才能执行索引。

* 使用[!DNL oak-run.jar]联机索引[!DNL TarMK]应对AEM实例维护窗口外的AEM实例执行&#x200B;**不**。

## 使用oak-run.jar建立TarMK脱机索引

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]离线索引[!DNL TarMK]是[!DNL TarMK]基于[!DNL oak-run.jar]的最简单索引方法，因为它需要一个[!DNL oak-run.jar]命令，但它需要关闭AEM实例。

## 使用oak-run.jar的TarMK带外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* 使用[!DNL oak-run.jar]在[!DNL TarMK]上建立带外索引可最大限度地减少索引对使用中AEM实例的影响。
* 带外索引是AEM安装的推荐索引方法，在这种情况下，重新编入/索引的时间超过了可用的维护时间范围。

## 使用oak-run.jar建立MongoMK在线索引

* 在[!DNL MongoMK]和[!DNL RDBMK]上具有[!DNL oak-run.jar]的联机索引是重新索引[!DNL MongoMK]（和[!DNL RDBMK]）AEM安装的推荐方法。 **或不应使用其他 [!DNL MongoMK] 方 [!DNL RDBMK]法。**
* 只需对群集中的单个AEM实例执行此索引。
* [!DNL MongoMK]的在线索引是安全的，可以针对正在运行的AEM群集执行，因为存储库遍历将仅发生在单个[!DNL MongoDB]节点上，从而允许其他节点继续提供请求，而不会对性能产生重大影响。

用于执行[!DNL MongoMK]联机索引的[!DNL oak-run.jar]索引命令与 [!DNL TarMK] 与 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)联机索引的[相同，其差异在于区段存储参数指向包含节点存储的[!DNL MongoDB]实例。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 辅助材料

* [下载 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *如上所述，确保下载的版本与AEM上安装的Oak版本匹配*
* [Apache Jackrabbit Oak-run.jar索引命令文档](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
