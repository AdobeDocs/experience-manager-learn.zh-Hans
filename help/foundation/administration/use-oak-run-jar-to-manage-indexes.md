---
title: 使用oak-run.jar管理索引
description: oak-run.jar的index命令整合了許多在AEM中管理Oak索引的功能，包括收集索引統計資料、執行索引一致性檢查以及重新索引索引本身。
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

[!DNL oak-run.jar]的index指令整合了許多要管理的功能 [!DNL Oak]AEM中的200個索引，包括收集索引統計資料、執行索引一致性檢查以及重新索引索引本身。

>[!NOTE]
>
>在本文與影片中，辭彙索引與重新索引可互換使用，且被視為相同的操作。

## [!DNL oak-run.jar] index命令基本知識

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* 的版本 [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) 使用的必須符合AEM執行個體上使用的Oak版本。
* 管理索引，使用 [!DNL oak-run.jar] 可運用 **[!DNL index]** 具有各種旗標以支援不同作業的命令。

   * `java -jar oak-run*.jar index ...`

## 索引統計資料

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` 會傾印所有索引定義、重要索引統計資料和索引內容，以供離線分析。
* 可以在使用中的AEM執行個體上安全地執行索引統計資料收集。

## 索引一致性檢查

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` 快速判斷lucene Oak索引是否損毀。
* 針對一致性檢查層級1和2，可在使用中的AEM執行個體上安全地執行一致性檢查。

## TarMK線上索引，使用 [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* 的線上索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 比設定快 `reindex=true` 於 `oak:queryIndexDefinition` 節點。 儘管效能提升，線上索引仍使用 [!DNL oak-run.jar] 仍需要維護時段來執行索引。

* 的線上索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 應該 **not** 在AEM執行個體維護期間之外，對AEM執行個體執行。

## 使用oak-run.jar建立TarMK離線索引

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* 的離線索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 是最簡單的 [!DNL oak-run.jar] 的基礎索引方法 [!DNL TarMK] 因為它需要單一 [!DNL oak-run.jar] 命令，但是它需要關閉AEM執行個體。

## 使用oak-run.jar編制的TarMK頻外索引

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* 上的頻外索引 [!DNL TarMK] 使用 [!DNL oak-run.jar] 將索引對正在使用的AEM執行個體產生的影響減到最少。
* 頻外索引是針對AEM安裝推薦的索引方法，當重新編列/索引的時間超過可用的維護時段時。

## 使用oak-run.jar建立MongoMK線上索引

* 線上索引，使用 [!DNL oak-run.jar] 於 [!DNL MongoMK] 和 [!DNL RDBMK] 是重新/索引的建議方法 [!DNL MongoMK] (和 [!DNL RDBMK]) AEM安裝。 **不應將任何其他方法用於 [!DNL MongoMK] 或 [!DNL RDBMK].**
* 此索引只需要針對叢集中的單一AEM執行個體執行。
* 的線上索引 [!DNL MongoMK] 可針對執行中的AEM叢集安全執行，因為存放庫周遊只會發生在單一叢集上 [!DNL MongoDB] 節點，讓其他節點能夠繼續處理請求，而不會對效能造成重大影響。

此 [!DNL oak-run.jar] index指令執行線上索引 [!DNL MongoMK] 是 [與 [!DNL TarMK] 使用建立線上索引 [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) 區段存放區引數指向的差異為 [!DNL MongoDB] 包含節點存放區的執行個體。

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## 支援材料

* [下载 [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *確保下載的版本符合上述在AEM上安裝的Oak版本*
* [Apache Jackrabbit Oak-run.jar索引命令檔案](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
