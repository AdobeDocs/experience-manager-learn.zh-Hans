---
title: 简单搜索实施指南
description: “简单搜索”实施是2017年Summit lab AEM Search Demystified中的材料。 本页包含本实验的材料。 有关实验室的指导教程，请查看本页演示部分的实验室工作簿。
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
feature: 搜索
topic: 开发
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '899'
ht-degree: 2%

---


# 简单搜索实施指南{#simple-search-implementation-guide}

“简单搜索”实施是&#x200B;**Adobe Summit实验室AEM Search Demystified**&#x200B;中的材料。 本页包含本实验的材料。 有关实验室的指导教程，请查看本页演示部分的实验室工作簿。

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
* [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak索引定义生成器](https://oakutils.appspot.com/generate/index)

### 章 {#chapters}

*以下章节链接假定AEM作者 [在](#initialpackages) 以下位置安装了初始包`http://localhost:4502`*

* [第1章](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [第二章](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [第三章](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [第四章](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [第五章](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [第六章](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [第七章](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [第八章](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
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
* [第五章设置](assets/l4080/l4080-chapter5-setup.zip)
* [第5章解决方案](assets/l4080/l4080-chapter5-solution.zip)
* [第6章解决方案](assets/l4080/l4080-chapter6.zip)
* [第9章解决方案](assets/l4080/l4080-chapter9.zip)

## 参考资料 {#reference-materials}

* [Github存储库](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [Sling模型导出程序](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://experienceleague.adobe.com/docs/)
* [AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([文档页面](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 更正和后续行动 {#corrections-and-follow-up}

实验室讨论的更正和澄清以及与会者对后续问题的回答。

1. **如何停止重新索引？**

   可通过[AEM Web控制台> JMX](http://localhost:4502/system/console/jmx)提供的IndexStats MBean停止重新索引

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 执行`abortAndPause()`以中止重新索引。 这将锁定索引以进一步重新编入索引，直到调用`resume()`。
      * 执行`resume()`将重新启动索引过程。
   * 文档：[https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **oak索引如何支持多个租户？**

   Oak支持将索引贯穿内容树，并且这些索引将仅在该子树中进行索引。 例如，可以创建&#x200B;**`/content/site-a/oak:index/cqPageLucene`**&#x200B;以仅在&#x200B;**`/content/site-a`下索引内容。**

   等效的方法是在&#x200B;**`/oak:index`**&#x200B;下的索引上使用&#x200B;**`includePaths`**&#x200B;和&#x200B;**`queryPaths`**&#x200B;属性。 例如：

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   此方法的考虑事项包括：

   * 查询必须指定一个与索引的查询路径范围相等的路径限制，或者该路径限制是该索引的子体。
   * 范围更广的索引（例如`/oak:index/cqPageLucene`）也将对数据进行索引，从而导致重复摄取和磁盘使用成本。
   * 可能需要重复的配置管理(例如 在多个租户索引中添加相同的indexRules（如果它们必须满足相同的查询集）
   * 在AEM发布层中，这种方法最适用于自定义站点搜索，与在AEM作者中一样，对于不同租户（例如，通过OmniSearch），通常在内容树的上方执行查询 — 不同的索引定义可能只导致基于路径限制的不同行为。


3. **所有可用分析程序的列表位于何处？**

   Oak公开了一组lucene提供的分析器配置元素，以供在AEM中使用。

   * [Apache Oak分析程序文档](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [令牌器](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [筛选器](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **如何在同一查询中搜索页面和资产？**

   AEM 6.3中的新增功能是在同一提供的查询中查询多个节点类型。 以下QueryBuilder查询。 请注意，每个“子查询”都可以解析为其自己的索引，因此在此示例中，`cq:Page`子查询解析为`/oak:index/cqPageLucene`,`dam:Asset`子查询解析为`/oak:index/damAssetLucene`。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   生成以下查询和查询计划：

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   通过[QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group)和[AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)浏览查询和结果。

5. **如何在同一查询中跨多个路径进行搜索？**

   AEM 6.3中的新增功能是在同一查询中跨多个路径进行查询。 以下QueryBuilder查询。 请注意，每个“子查询”都可以解析为其自己的索引。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   生成以下查询和查询计划

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   通过[QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group)和[AEM Chrome插件](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US)浏览查询和结果。
