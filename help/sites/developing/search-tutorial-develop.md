---
title: 简单的搜索实施指南
description: 简单搜索实施是2017年峰会实验室AEM Search Demystified的材料。 本页包含本实验的材料。 有关实验的指导性导览，请视图本页“演示”部分的实验室工作簿。
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 1%

---


# 简单的搜索实施指南{#simple-search-implementation-guide}

简单搜索实施是Adobe峰会实验室AEM **搜索演示中提供的材料**。 本页包含本实验的材料。 有关实验的指导性导览，请视图本页“演示”部分的实验室工作簿。

![搜索架构概述](assets/l4080/simple-search-application.png)

## 演示材料 {#bookmarks}

* [实验室工作簿](assets/l4080/l4080-lab-workbook.pdf)
* [演示文稿](assets/l4080/l4080-presentation.pdf)

## 书签 {#bookmarks-1}

### 工具 {#tools}

* [索引管理器](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [说明查询](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [CRX包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder调试器](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak Index Definition Generator](https://oakutils.appspot.com/generate/index)

### 章 {#chapters}

*以下章节链接假定初[始包](#initialpackages)安装在AEM作者的`http://localhost:4502`*

* [第一章](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [第二章](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [第三章](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [第四章](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [第五章](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [第6章](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [第七章](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [第8章](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [第九章](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## 包 {#packages}

### 初始包 {#initial-packages}

* [标记](assets/l4080/summit-tags.zip)
* [简单搜索应用程序包](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### 章节包 {#chapter-packages}

* [第1章解决方案](assets/l4080/l4080-chapter1.zip)
* [第二章解决](assets/l4080/l4080-chapter2.zip)
* [第三章解决](assets/l4080/l4080-chapter3.zip)
* [第四章解决](assets/l4080/l4080-chapter4.zip)
* [第五章设置](assets/l4080/l4080-chapter5-setup.zip)
* [第五章解决](assets/l4080/l4080-chapter5-solution.zip)
* [第6章解决方案](assets/l4080/l4080-chapter6.zip)
* [第9章解决](assets/l4080/l4080-chapter9.zip)

## Reference Materials {#reference-materials}

* [Github存储库](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling Models](https://sling.apache.org/documentation/bundles/models.html)
* [Sling Model Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://docs.adobe.com/docs/en/aem/6-2/develop/search/querybuilder-api.html)
* [AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([文档页](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 更正和后续 {#corrections-and-follow-up}

实验室讨论中的更正和说明以及与会者对后续问题的回答。

1. **如何停止重新索引？**

   可通过AEM Web Console > JMX提供的IndexStats MBean [停止重新索引](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 执 `abortAndPause()` 行以中止重新索引。 这将锁定索引以进一步重新编制索引，直到被 `resume()` 调用为止。
      * 执行 `resume()` 将重新启动索引建立过程。
   * 文档： [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Oak索引如何支持多个租户？**

   Oak支持通过内容树放置索引，这些索引将仅在该子树中进行索引。 例如， **`/content/site-a/oak:index/cqPageLucene`** 可以创建内容索引，只在 **`/content/site-a`下。**

   等效的方法是使用下 **`includePaths`** 的索 **`queryPaths`** 引上的和属性 **`/oak:index`**。 例如：

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   此方法的考虑事项包括：

   * 查询必须指定与索引的查询路径范围相等的路径限制，或者是其中的子代。
   * 范围更广的索引(例 `/oak:index/cqPageLucene`如)还会为数据编制索引，从而导致重复引入和磁盘使用成本。
   * 可能需要重复的配置管理(例如 在多个租户索引中添加相同的indexRules(如果它们必须满足相同的查询集)
   * 此方法在AEM发布层中最适合进行自定义站点搜索，与在AEM作者中一样，在内容树中为不同租户（例如，通过OmniSearch）高层执行查询很常见——不同的索引定义可能只根据路径限制导致不同的行为。


3. **所有可用分析器的列表位于何处？**

   Oak公开了一组lucene提供的分析器配置元素，供AEM使用。

   * [Apache Oak Analyzers文档](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [标记器](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [筛选器](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **如何在同一查询中搜索页面和资产？**

   AEM 6.3中的新增功能是在提供的同一查询中查询多个节点类型。 以下QueryBuilder查询。 请注意，每个“子查询”都可解析为自己的索引，因此在本例 `cq:Page` 中，子查询解析 `/oak:index/cqPageLucene` 为，子 `dam:Asset` 查询解析为 `/oak:index/damAssetLucene`。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   将产生以下查询和查询计划：

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   通过QueryBuilder调试器和 [AEM](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) Chrome插 [件浏览查询和结果](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)。

5. **如何在同一查询中搜索多个路径？**

   AEM 6.3中的新增功能是在同一提供的查询中跨多个路径进行查询。 以下QueryBuilder查询。 请注意，每个“子查询”都可解析为自己的索引。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   产生以下查询和查询计划

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   通过QueryBuilder调试器和 [AEM](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) Chrome插 [件浏览查询和结果](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)。
