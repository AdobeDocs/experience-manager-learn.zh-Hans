---
title: 简单搜索实施指南
description: 简单搜索实现来自2017 Summit实验室AEM Search Demystified的材料。 本页包含本实验中的资料。 有关引导式实验教程，请查看本页“演示文稿”部分中的“实验”工作簿。
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 202
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 1%

---

# 简单搜索实施指南{#simple-search-implementation-guide}

Simple search实施是来自 **Adobe Summit实验室AEM Search Demystified**. 本页包含本实验中的资料。 有关引导式实验教程，请查看本页“演示文稿”部分中的“实验”工作簿。

![搜索架构概述](assets/l4080/simple-search-application.png)

## 演示材料 {#bookmarks}

* [实验室工作簿](assets/l4080/l4080-lab-workbook.pdf)
* [演示文稿](assets/l4080/l4080-presentation.pdf)

## 书签 {#bookmarks-1}

### 工具 {#tools}

* [索引管理器](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [说明查询](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak：index/cqPageLucene
* [CRX包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html？)
* [Oak索引定义生成器](https://oakutils.appspot.com/generate/index)

### 章 {#chapters}

*以下章节链接采用 [初始包](#initialpackages) 在AEM Author上安装`http://localhost:4502`*

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

### 初始包 {#initial-packages}

* [标记](assets/l4080/summit-tags.zip)
* [简单搜索应用程序包](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### 章节包 {#chapter-packages}

* [第1章解决方案](assets/l4080/l4080-chapter1.zip)
* [第2章解决方案](assets/l4080/l4080-chapter2.zip)
* [第3章解决方案](assets/l4080/l4080-chapter3.zip)
* [第4章解决方案](assets/l4080/l4080-chapter4.zip)
* [第五章　设置](assets/l4080/l4080-chapter5-setup.zip)
* [第5章解决方案](assets/l4080/l4080-chapter5-solution.zip)
* [第6章解决方案](assets/l4080/l4080-chapter6.zip)
* [第9章解决方案](assets/l4080/l4080-chapter9.zip)

## 引用的材料 {#reference-materials}

* [Github存储库](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [Sling模型导出程序](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://experienceleague.adobe.com/docs/)
* [AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([文档页面](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 更正和跟进 {#corrections-and-follow-up}

实验室讨论的更正和说明，以及与会者后续提问的回答。

1. **如何停止重新编制索引？**

   可以通过以下方式提供的IndexStats MBean停止重新索引： [AEM Web控制台> JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 执行 `abortAndPause()` 以中止重新编制索引。 这将锁定索引以进一步重新索引，直至 `resume()` 将调用。
      * 正在执行 `resume()` 将重新启动索引过程。
   * 文档： [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Oak索引如何支持多个租户？**

   Oak支持将索引放置到内容树之外，并且这些索引将仅在该子树中索引。 例如 **`/content/site-a/oak:index/cqPageLucene`** 只能创建以索引以下内容 **`/content/site-a`.**

   等效的方法是使用 **`includePaths`** 和 **`queryPaths`** 下的索引的属性 **`/oak:index`**. 例如：

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   此方法的注意事项包括：

   * 查询必须指定与索引的查询路径范围相等的路径限制，或为其子级。
   * 范围更广的索引(例如 `/oak:index/cqPageLucene`)也将索引数据，从而导致重复摄取和磁盘使用成本。
   * 可能需要重复的配置管理(例如 在多个租户索引中添加相同的indexRules（如果它们必须满足相同的查询集）
   * 这种方法最好在AEM发布层上用于自定义站点搜索，就像在AEM Author上一样，对于不同的租户，通常在内容树的高处执行查询（例如，通过OmniSearch） — 不同的索引定义可能会导致仅基于路径限制的不同行为。

3. **所有可用Analyser的列表在何处？**

   Oak公开了一组用于AEM的lucene提供的分析器配置元素。

   * [Apache Oak Analysers文档](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [令牌器](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [过滤器](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **如何在同一查询中搜索页面和资产？**

   AEM 6.3的新增功能是在同一提供的查询中查询多个节点类型。 以下QueryBuilder查询。 请注意，每个“子查询”都可以解析为自己的索引，因此在此示例中， `cq:Page` 子查询解析为 `/oak:index/cqPageLucene` 和 `dam:Asset` 子查询解析为 `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   查询和查询计划中的结果：

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   通过以下方式浏览查询和结果 [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) 和 [AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **如何在同一查询中搜索多个路径？**

   AEM 6.3的新增功能是在同一提供的查询中跨多个路径进行查询。 以下QueryBuilder查询。 请注意，每个“子查询”都可以解析为自己的索引。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   以下查询和查询计划中的结果

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   通过以下方式浏览查询和结果 [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) 和 [AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
