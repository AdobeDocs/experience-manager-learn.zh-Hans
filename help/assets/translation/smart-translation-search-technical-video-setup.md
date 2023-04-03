---
title: 使用AEM Assets设置智能翻译搜索
description: 智能翻译搜索允许使用非英语搜索词解析为英文内容。 要设置AEM以进行智能翻译搜索，必须安装和配置Apache Oak Search Machine Translation OSGi包，以及包含翻译规则的相关免费开源Apache Joshua语言包。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 0%

---

# 使用AEM Assets设置智能翻译搜索{#set-up-smart-translation-search-with-aem-assets}

智能翻译搜索允许使用非英语搜索词解析为英文内容。 要设置AEM以进行智能翻译搜索，必须安装和配置Apache Oak Search Machine Translation OSGi包，以及包含翻译规则的相关免费开源Apache Joshua语言包。

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>必须在需要智能翻译的每个AEM实例上设置智能翻译搜索。

1. 下载并安装Oak搜索机翻译OSGi包
   * [下载Oak搜索机翻译OSGi包](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) 对应于AEM Oak版本。
   * 通过将下载的Oak搜索机翻译OSGi包安装到AEM中 [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. 下载和更新Apache Joshua语言包
   * 下载并解压缩所需内容 [Apache Joshua语言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * 编辑 `joshua.config` 文件并注释掉以下2行开头：

      ```
      feature-function = LanguageModel ...
      ```

   * 确定并记录语言包的模型文件夹的大小，因为这会影响AEM需要的额外堆空间量。
   * 移动未压缩的Apache Joshua语言包文件夹(使用 `joshua.config` 编辑)

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      例如：

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 使用更新的堆内存分配重新启动AEM
   * 停止AEM
   * 确定AEM所需的新堆大小

      * AEM预语言缺堆大小+模型目录的大小四舍五入到最接近的2GB
      * 例如：如果预语言包安装AEM需要运行8GB的堆，并且语言包的模型文件夹未压缩为3.8GB，则新堆大小为：

         原始 `8GB` +( `3.75GB` 四舍五入到最接近的值 `2GB`，其中 `4GB`) `12GB`
   * 验证计算机是否具有此数量的额外可用内存。
   * 更新AEM启动脚本以根据新堆大小进行调整

      * 例如. `java -Xmx12g -jar cq-author-p4502.jar`
   * 通过增加堆大小重新启动AEM。

   >[!NOTE]
   >
   >语言包所需的堆空间可能会增大，尤其是当使用多个语言包时。
   >
   >
   >总是确保 **实例具有足够的内存** 以适应已分配堆空间的增加。
   >
   >
   >的 **必须始终计算基本堆以支持可接受的性能，而不使用任何语言包** 已安装。

4. 通过Apache Jackrabbit Oak机器翻译全文查询术语提供程序OSGi配置注册语言包

   * 对于每个语言包， [创建新的Apache Jackrabbit Oak机器翻译全文查询术语提供程序OSGi配置](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) 通过AEM Web Console的配置管理器。

      * `Joshua Config Path` 是joshua.config文件的绝对路径。 AEM进程必须能够读取语言包文件夹中的所有文件。
      * `Node types` 是候选节点类型，其全文搜索将使用此语言包进行翻译。
      * `Minimum score` 是要使用的已翻译术语的最小置信度分数。

         * 例如，同音（西班牙语的“man”）可以翻译成英语单词“man”，置信度得分为 `0.9` 还翻译成“人”这个英文单词，并且有置信度 `0.2`. 将最小分数调整为 `0.3`，将“hombre”保留为“man”翻译，但将“hombre”改为“human”翻译，作为 `0.2` 小于 `0.3`.

5. 对资产执行全文搜索
   * 由于dam:Asset是再次注册此语言包的节点类型，因此我们必须使用全文搜索来搜索AEM Assets以验证此内容。
   * 导航到AEM >资产，然后打开Omnisearch。 使用已安装语言包的语言搜索术语。
   * 根据需要，在OSGi配置中调整最小分数，以确保结果的准确性。

6. 更新语言包
   * Apache Joshua语言包由Apache Joshua项目完整维护，其更新或更正由Apache Joshua项目自行决定。
   * 如果语言包已更新，则要在AEM中安装更新，则必须执行上述步骤2 - 4，并根据需要调整堆大小。

      * 请注意，在将未压缩的语言包移动到crx-quickstart/opt文件夹时，请先移动任何现有的语言包文件夹，然后再复制到新文件夹中。
   * 如果AEM不需要重新启动，则必须重新保存与更新的语言包相关的Apache Jackrabbit Oak Machien翻译全文查询术语提供程序OSGi配置，以便AEM处理更新的文件。


## 更新damAssetLucene索引 {#updating-damassetlucene-index}

为 [AEM智能标记](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) 受AEM智能翻译、AEM的影响 `/oak   :index  /damAssetLucene` 必须更新索引，以将预测的标记（“智能标记”的系统名称）标记为资产聚合Lucene索引的一部分。

在 `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`，请确保配置如下所示：

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

* [Apache Oak搜索机翻译OSGi包](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua语言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM智能标记](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [查询和索引的最佳实践](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
