---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了许多功能来管理AEM中的Oak索引，这些功能包括收集索引统计数据、运行索引一致性检查以及重新索引索引本身。
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

[!DNL oak-run.jar]的index命令整合了许多要管理的功能 [!DNL Oak]AEM中的200个索引，包括收集索引统计数据、运行索引一致性检查以及重新索引索引本身。

>[!NOTE]
>
>在本文和视频中，术语索引和重新索引可互换使用，并被视为相同的操作。

## [!DNL oak-run.jar] index命令基础知识

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 的版本 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 使用的必须匹配AEM实例上使用的Oak版本。
* 使用管理索引 [!DNL oak-run.jar] 利用 **[!DNL index]** 命令中提供了多种标记以支持不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引统计信息

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` 转储所有索引定义、重要索引统计和索引内容以进行离线分析。
* 可以在正在使用的AEM实例上安全地执行索引统计信息收集。

## 索引一致性检查

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` 快速确定lucene Oak索引是否损坏。
* 对于一致性检查级别1和2，可以在正在使用的AEM实例上安全地运行一致性检查。

## TarMK联机索引 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 在线索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 比设置快 `reindex=true` 在 `oak:queryIndexDefinition` 节点。 尽管性能有所提高，但在线索引使用 [!DNL oak-run.jar] 仍需要维护窗口来执行索引。

* 在线索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 应该 **非** 在AEM实例维护窗口之外对AEM实例执行。

## 使用oak-run.jar建立TarMK离线索引

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 离线索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 是最简单的 [!DNL oak-run.jar] 基于索引的方法 [!DNL TarMK] 因为它需要一个 [!DNL oak-run.jar] 命令，但是它要求关闭AEM实例。

## 使用oak-run.jar进行TarMK带外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 上的带外索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 将索引对正在使用的AEM实例的影响降至最低。
* 带外索引是推荐用于AEM安装的索引方法，其中重新编入/索引的时间超过了可用的维护时段。

## 使用oak-run.jar的MongoMK在线索引

* 联机索引 [!DNL oak-run.jar] 日期 [!DNL MongoMK] 和 [!DNL RDBMK] 是重新/索引的推荐方法 [!DNL MongoMK] (和 [!DNL RDBMK]) AEM安装。 **不应将任何其他方法用于 [!DNL MongoMK] 或 [!DNL RDBMK].**
* 此索引只需要针对群集中的单个AEM实例执行。
* 在线索引 [!DNL MongoMK] 对正在运行的AEM群集执行是安全的，因为存储库遍历将仅在一个 [!DNL MongoDB] 节点，允许其他节点继续为请求提供服务而不会对性能产生重大影响。

此 [!DNL oak-run.jar] index命令执行联机索引 [!DNL MongoMK] 是 [与 [!DNL TarMK] 使用进行在线索引 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 与区段存储参数指向的 [!DNL MongoDB] 包含节点存储的实例。

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
