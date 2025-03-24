---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了大量功能来管理AEM中的Oak索引，这些功能包括收集索引统计数据、运行索引一致性检查以及重新索引索引本身。
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# 使用oak-run.jar管理索引

[!DNL oak-run.jar]的index命令整合了一些功能以在AEM中管理[!DNL Oak]200个索引，这些功能包括收集索引统计数据、运行索引一致性检查以及重新索引本身。

>[!NOTE]
>
>在本文和视频中，术语索引和重新索引被互换使用，并被视为相同的操作。

## [!DNL oak-run.jar] index命令基本信息

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 使用的[[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0)版本必须与AEM实例上使用的Oak版本匹配。
* 使用[!DNL oak-run.jar]管理索引时，会利用带有各种标记的&#x200B;**[!DNL index]**&#x200B;命令来支持不同的操作。

   * `java -jar oak-run*.jar index ...`

## 索引统计信息

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar`转储所有索引定义、重要索引统计和索引内容以进行离线分析。
* 可以在正在使用的AEM实例上安全地执行索引统计信息收集。

## 索引一致性检查

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar`快速确定lucene Oak索引是否损坏。
* 对于一致性检查级别1和2，可以在正在使用的AEM实例上安全运行一致性检查。

## 使用[!DNL oak-run.jar]的TarMK联机索引 {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 使用[!DNL oak-run.jar]对[!DNL TarMK]进行联机索引比在`oak:queryIndexDefinition`节点上设置`reindex=true`更快。 尽管此性能提高，但使用[!DNL oak-run.jar]的联机索引仍需要维护窗口来执行索引。

* 使用[!DNL oak-run.jar]对[!DNL TarMK]进行联机索引应&#x200B;**不应**&#x200B;对AEM实例维护时段以外的AEM实例执行。

## 使用oak-run.jar建立TarMK离线索引

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 使用[!DNL oak-run.jar]对[!DNL TarMK]进行脱机索引是[!DNL TarMK]的最简单基于[!DNL oak-run.jar]的索引方法，因为它需要单个[!DNL oak-run.jar]命令，但需要关闭AEM实例。

## 使用oak-run.jar进行TarMK带外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 使用[!DNL oak-run.jar]对[!DNL TarMK]进行带外索引可将索引对正在使用的AEM实例的影响降至最低。
* 带外索引是推荐用于AEM安装的索引方法，当重新/索引时间超过可用的维护窗口时。

## 使用oak-run.jar的MongoMK在线索引

* 建议在[!DNL MongoMK]和[!DNL RDBMK]上使用[!DNL oak-run.jar]对联机索引重新编制索引[!DNL MongoMK] （和[!DNL RDBMK]） AEM安装。 **不应对[!DNL MongoMK]或[!DNL RDBMK]使用任何其他方法。**
* 此索引只需要针对群集中的单个AEM实例执行。
* 对正在运行的AEM群集安全地执行[!DNL MongoMK]的联机索引，因为存储库遍历将只发生在单个[!DNL MongoDB]节点上，这将允许其他节点继续处理请求而不会对性能产生重大影响。

用于执行[!DNL MongoMK]的联机索引的[!DNL oak-run.jar]索引命令与 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar)的 [!DNL TarMK] 联机索引的[相同，不同之处在于区段存储参数指向包含节点存储的[!DNL MongoDB]实例。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 支持材料

* [下载 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *确保下载的版本与如上所述在AEM上安装的Oak版本匹配*
* [Apache Jackrabbit Oak oak-run.jar索引命令文档](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
