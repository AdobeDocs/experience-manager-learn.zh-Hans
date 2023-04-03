---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了许多功能，用于管理AEM中的Oak索引，包括收集索引统计信息、运行索引一致性检查以及重新索引索引本身。
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# 使用oak-run.jar管理索引

[!DNL oak-run.jar]&#39;s index命令整合了许多功能以管理 [!DNL Oak]在AEM中收集200个索引，包括收集索引统计信息、运行索引一致性检查，以及重新索引索引本身。

>[!NOTE]
>
>在本文和视频中，术语索引和重新索引可互换使用，并被视为相同的操作。

## [!DNL oak-run.jar] 索引命令基础知识

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 的版本 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 使用的版本必须匹配AEM实例中使用的Oak版本。
* 使用管理索引 [!DNL oak-run.jar] 利用 **[!DNL index]** 命令来支持不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引统计

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` 转储用于离线分析的所有索引定义、重要索引统计资料和索引内容。
* 在使用中的AEM实例上执行索引统计信息收集是安全的。

## 索引一致性检查

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` 快速确定lucene Oak索引是否已损坏。
* 在使用中的AEM实例上运行一致性检查级别1和2是安全的。

## TarMK在线索引(使用 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 在线索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 快于设置 `reindex=true` 在 `oak:queryIndexDefinition` 节点。 尽管性能提高了，但使用 [!DNL oak-run.jar] 仍需要维护窗口才能执行索引。

* 在线索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] show **not** 在AEM实例维护窗口之外对AEM实例执行。

## 使用oak-run.jar建立TarMK脱机索引

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 脱机索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 最简单 [!DNL oak-run.jar] 基于索引的方法 [!DNL TarMK] 因为它需要一个 [!DNL oak-run.jar] 命令，但是它要求关闭AEM实例。

## 使用oak-run.jar的TarMK带外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 带外索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 最大限度地减少索引对正在使用的AEM实例的影响。
* 带外索引是AEM安装的推荐索引方法，在这种情况下，重新编入/索引的时间超过了可用的维护时间范围。

## 使用oak-run.jar建立MongoMK在线索引

* 在线索引 [!DNL oak-run.jar] on [!DNL MongoMK] 和 [!DNL RDBMK] 是推荐的重新索引方法 [!DNL MongoMK] (和 [!DNL RDBMK])AEM安装。 **不应使用其他方法 [!DNL MongoMK] 或 [!DNL RDBMK].**
* 只需对群集中的单个AEM实例执行此索引。
* 在线索引 [!DNL MongoMK] 对运行的AEM群集执行是安全的，因为存储库遍历将仅发生在一个群集上 [!DNL MongoDB] 节点，允许其他节点继续提供请求，而不会对性能产生重大影响。

的 [!DNL oak-run.jar] 索引命令，用于执行 [!DNL MongoMK] 是 [与 [!DNL TarMK] 在线索引 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 区段存储参数指向 [!DNL MongoDB] 包含节点存储的实例。

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
