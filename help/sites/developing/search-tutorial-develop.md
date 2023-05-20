---
title: 簡易搜尋實作指南
description: 簡單搜尋實作是來自2017 Summit lab AEM Search Demystified的資料。 本頁包含本實驗室的資料。 如需實驗導覽，請檢視本頁簡報區段中的實驗活頁簿。
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 3%

---

# 簡易搜尋實作指南{#simple-search-implementation-guide}

簡單搜尋實作是來自 **Adobe Summit實驗室AEM搜尋Demystified**. 本頁包含本實驗室的資料。 如需實驗導覽，請檢視本頁簡報區段中的實驗活頁簿。

![搜尋架構概觀](assets/l4080/simple-search-application.png)

## 簡報資料 {#bookmarks}

* [實驗室活頁簿](assets/l4080/l4080-lab-workbook.pdf)
* [演示文稿](assets/l4080/l4080-presentation.pdf)

## 書籤 {#bookmarks-1}

### 工具 {#tools}

* [索引管理器](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [说明查询](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak：index/cqPageLucene
* [CRX封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html？)
* [Oak索引定義產生器](https://oakutils.appspot.com/generate/index)

### 章 {#chapters}

*以下章節連結假設 [初始封裝](#initialpackages) 安裝在AEM Author上的`http://localhost:4502`*

* [第1章](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [第2章](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [第3章](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [第4章](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [第5章](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [第6章](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [第7章](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [第8章](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [第9章](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## 包 {#packages}

### 初始封裝 {#initial-packages}

* [标记](assets/l4080/summit-tags.zip)
* [簡易搜尋應用程式套件](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### 章節套件 {#chapter-packages}

* [第1章解決方案](assets/l4080/l4080-chapter1.zip)
* [第2章解決方案](assets/l4080/l4080-chapter2.zip)
* [第3章解決方案](assets/l4080/l4080-chapter3.zip)
* [第4章解決方案](assets/l4080/l4080-chapter4.zip)
* [第5章設定](assets/l4080/l4080-chapter5-setup.zip)
* [第5章解決方案](assets/l4080/l4080-chapter5-solution.zip)
* [第6章解決方案](assets/l4080/l4080-chapter6.zip)
* [第9章解決方案](assets/l4080/l4080-chapter9.zip)

## 參考的材料 {#reference-materials}

* [Github存放庫](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [Sling模型匯出工具](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://experienceleague.adobe.com/docs/)
* [AEM Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([檔案頁面](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 更正與跟進 {#corrections-and-follow-up}

從實驗室討論中更正和澄清，並回答與會者的後續問題。

1. **如何停止重新索引？**

   可以透過以下方式提供的IndexStats MBean停止重新索引： [AEM Web Console > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 執行 `abortAndPause()` 以中止重新索引。 這會將索引鎖定以進一步重新索引，直到 `resume()` 叫用的是。
      * 執行中 `resume()` 將重新啟動索引過程。
   * 檔案： [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Oak索引如何支援多個租使用者？**

   Oak支援將索引置入整個內容樹狀結構中，而且這些索引只會在該子樹狀結構中建立索引。 例如 **`/content/site-a/oak:index/cqPageLucene`** 只能建立來索引以下內容 **`/content/site-a`.**

   一個等效的方法是使用 **`includePaths`** 和 **`queryPaths`** 下的索引上的屬性 **`/oak:index`**. 例如：

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   此方法的考量事項包括：

   * 查詢必須指定等於索引的查詢路徑範圍的路徑限制，或為其子代。
   * 範圍更廣的索引(例如 `/oak:index/cqPageLucene`)也會將資料編入索引，導致重複擷取和磁碟使用成本。
   * 可能需要重複的設定管理(例如： 在多個租使用者索引中新增相同的indexRules （如果它們必須滿足相同的查詢集）
   * 此方法最適合在AEM Publish層級用於自訂網站搜尋，如同在AEM Author上一樣，對於不同的租使用者（例如，透過OmniSearch），查詢通常會在內容樹狀結構的高層執行 — 不同的索引定義可能僅根據路徑限制導致不同的行為。


3. **所有可用分析器的清單在何處？**

   Oak會公開一組供AEM使用的lucene提供的分析器設定元素。

   * [Apache Oak Analyzer檔案](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [过滤器](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [字元篩選器](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **如何在相同的查詢中搜尋頁面和資產？**

   AEM 6.3的新功能是在相同提供的查詢中查詢多個節點型別。 下列QueryBuilder查詢。 請注意，每個「子查詢」都可以解析為自己的索引，所以在此範例中， `cq:Page` 子查詢解析為 `/oak:index/cqPageLucene` 和 `dam:Asset` 子查詢解析為 `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   會產生下列查詢與查詢計畫：

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   透過以下方式探索查詢和結果： [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) 和 [AEM Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **如何在同一查詢中搜尋多個路徑？**

   AEM 6.3的新功能是在相同提供的查詢中，可跨多個路徑查詢。 下列QueryBuilder查詢。 請注意，每個「子查詢」都可以解析為自己的索引。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   產生下列查詢和查詢計畫

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   透過以下方式探索查詢和結果： [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) 和 [AEM Chrome外掛程式](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
