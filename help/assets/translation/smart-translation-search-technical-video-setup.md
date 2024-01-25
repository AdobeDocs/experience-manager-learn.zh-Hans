---
title: 使用AEM Assets设置智能翻译搜索
description: 智能翻译搜索允许使用非英语搜索词来解析为英语内容。 要为Smart Translation Search设置AEM，必须安装和配置Apache Oak Search Machine Translation OSGi捆绑包，以及包含翻译规则的相关免费开放源代码Apache Joshua语言包。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 634
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# 使用AEM Assets设置智能翻译搜索{#set-up-smart-translation-search-with-aem-assets}

智能翻译搜索允许使用非英语搜索词来解析为英语内容。 要为Smart Translation Search设置AEM，必须安装和配置Apache Oak Search Machine Translation OSGi捆绑包，以及包含翻译规则的相关免费开放源代码Apache Joshua语言包。

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>必须在每个需要智能翻译搜索的AEM实例上设置该搜索。

1. 下载并安装Oak搜索机器翻译OSGi包
   * [下载Oak搜索机器翻译OSGi捆绑包](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) 对应于AEM Oak版本。
   * 通过将下载的Oak Search Machine Translation OSGi捆绑包安装到AEM中 [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. 下载并更新Apache Joshua语言包
   * 下载并解压缩所需的 [Apache Joshua语言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * 编辑 `joshua.config` 并注释掉以下两行开头的行：

     ```
     feature-function = LanguageModel ...
     ```

   * 确定并记录语言包模型文件夹的大小，因为这影响AEM所需的额外栈空间。
   * 移动解压缩的Apache Joshua语言包文件夹(使用 `joshua.config` 编辑)

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     例如：

     ```
      .../crx-quickstart/opt/es-en
     ```

3. 使用更新的栈内存分配重新启动AEM
   * 停止AEM
   * 确定AEM所需的新栈大小

      * AEM缺少语言的栈大小+模型目录的大小向上舍入到最接近的2GB
      * 例如：如果预安装语言包，则AEM安装需要运行8GB栈，并且语言包的模型文件夹为3.8GB未压缩，则新的栈大小为：

        原件 `8GB` + ( `3.75GB` 四舍五入到最接近的 `2GB`，即 `4GB`)，总共 `12GB`

   * 验证计算机是否具有此数量的额外可用内存。
   * 更新AEM启动脚本以调整新栈大小

      * 例如 `java -Xmx12g -jar cq-author-p4502.jar`

   * 在栈大小增加的情况下重新启动AEM。

   >[!NOTE]
   >
   >语言包所需的栈空间可能会增大，尤其是在使用多个语言包时。
   >
   >
   >始终确保 **实例具有足够的内存** 以适应已分配栈空间的增加。
   >
   >
   >此 **必须始终计算基栈以支持可接受的性能，而无需任何语言包** 已安装。

4. 通过Apache Jackrabbit Oak机器翻译全文查询词提供商OSGi配置注册语言包

   * 对于每个语言包， [创建新的Apache Jackrabbit Oak机器翻译全文查询词提供商OSGi配置](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) 通过AEM Web控制台的配置管理器。

      * `Joshua Config Path` 是joshua.config文件的绝对路径。 AEM进程必须能够读取语言包文件夹中的所有文件。
      * `Node types` 是全文搜索将与此语言包结合进行翻译的候选节点类型。
      * `Minimum score` 是要使用的翻译术语的最小置信度分数。

         * 例如，hombre（西班牙语中的“man”）可将置信分数为 `0.9` 并将它翻译成英文单词“human”带有一个置信分数 `0.2`. 将最小分数调整为 `0.3`，会保留“homre”到“man”的翻译，但会放弃“homre”到“human”的翻译，因为此翻译分数为 `0.2` 小于的最小得分 `0.3`.

5. 对资产执行全文搜索
   * 由于dam：Asset是此语言包再次注册的节点类型，因此我们必须使用全文搜索来搜索AEM Assets以验证其有效性。
   * 导航到AEM > Assets，然后打开Omnisearch。 使用安装了语言包的语言搜索术语。
   * 根据需要，调整OSGi配置中的最低分数以确保结果的准确性。

6. 更新语言包
   * Apache Joshua语言包完全由Apache Joshua项目维护，其更新或更正由Apache Joshua项目自行决定。
   * 如果更新了语言包，要在AEM中安装更新，必须执行上述步骤2 - 4，并根据需要调整栈大小。

      * 请注意，将解压缩的语言包移动到crx-quickstart/opt文件夹时，请先移动任何现有的语言包文件夹，然后再复制到新的中。

   * 如果AEM不需要重新启动，则必须重新保存与更新的语言包相关的相关Apache Jackrabbit Oak Machine翻译全文查询词提供程序OSGi配置，以便AEM处理更新的文件。

## 更新damAssetLucene索引 {#updating-damassetlucene-index}

为了 [AEM智能标记](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) 将受AEM智能翻译的影响，AEM `/oak   :index  /damAssetLucene` 必须更新索引，以将predictedTags（“智能标记”的系统名称）标记为资产汇总Lucene索引的一部分。

下 `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`，确保配置如下所示：

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## 其他资源{#additional-resources}

* [Apache Oak Search Machine Translation OSGi包](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua语言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM智能标记](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [有关查询和索引的最佳实践](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
