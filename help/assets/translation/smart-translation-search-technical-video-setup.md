---
title: 使用AEM Assets设置智能翻译搜索
seo-title: 使用AEM Assets设置智能翻译搜索
description: 智能翻译搜索允许使用非英语搜索词解析为英语内容。 要为智能翻译搜索设置AEM，必须安装和配置Apache Oak Search Machine Translation OSGi捆绑包，以及包含翻译规则的相关免费和开放源代码Apache Joshua语言包。
seo-description: 智能翻译搜索允许使用非英语搜索词解析为英语内容。 要为智能翻译搜索设置AEM，必须安装和配置Apache Oak Search Machine Translation OSGi捆绑包，以及包含翻译规则的相关免费和开放源代码Apache Joshua语言包。
uuid: b0e8dab2-6bc4-4158-91a1-4b9811359798
discoiquuid: 4db1b4db-74f4-4646-b5de-cb891612cc90
topics: authoring, search, metadata, localization
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 0%

---


# 使用AEM Assets{#set-up-smart-translation-search-with-aem-assets}设置智能翻译搜索

智能翻译搜索允许使用非英语搜索词解析为英语内容。 要为智能翻译搜索设置AEM，必须安装和配置Apache Oak Search Machine Translation OSGi捆绑包，以及包含翻译规则的相关免费和开放源代码Apache Joshua语言包。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>智能翻译搜索必须在需要智能翻译的每个AEM实例上设置。

1. 下载并安装Oak搜索机器翻译OSGi捆绑包
   * [下载与AEM Oak版本对应的](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) Oak Search Machine Translation OSGi bundle。
   * 通过[ `/system/console/bundles`](http://localhost:4502/system/console/bundles)将下载的Oak Search Machine Translation OSGi捆绑包安装到AEM中。

2. 下载和更新Apache Joshua语言包
   * 下载并解压缩所需的[Apache Joshua语言包](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)。
   * 编辑`joshua.config`文件并注释掉以下2行开头：

      ```
      feature-function = LanguageModel ...
      ```

   * 确定并记录语言包的模型文件夹的大小，因为这会影响AEM需要多少额外的堆空间。
   * 将解压缩的Apache Joshua语言包文件夹（带有`joshua.config`编辑）移动到

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      例如：

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 使用更新的堆内存分配重新启动AEM
   * 停止AEM
   * 确定AEM的新所需堆大小

      * AEM prelanguage-lack堆大小+模型目录的大小舍入到最接近的2GB
      * 例如：如果AEM安装需要8GB堆才能运行，并且语言包的模型文件夹未压缩3.8GB，则新堆大小为：

         原始`8GB` +（`3.75GB`舍入到最接近的`2GB`，即`4GB`），共`12GB`
   * 验证计算机是否有此数量的额外可用内存。
   * 更新AEM开始脚本以调整新的堆大小

      * 例如. `java -Xmx12g -jar cq-author-p4502.jar`
   * 使用增加的堆大小重新启动AEM。

   >[!NOTE]
   >
   >语言包所需的堆空间可能会增大，尤其是当使用多语言包时。
   >
   >
   >请始终确保&#x200B;**实例具有足够的内存**&#x200B;以适应已分配堆空间的增加。
   >
   >
   >必须始终计算&#x200B;**基堆以支持可接受的性能，而不安装任何语言包**。

4. 通过Apache Jackrabbit Oak机器翻译全文查询条款提供商OSGi配置注册语言包

   * 对于每个语言包，[通过AEM Web Console的配置管理器新建一个Apache Jackrabbit Oak机器翻译全文查询条款提供程序OSGi配置](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)。

      * `Joshua Config Path` 是joshua.config文件的绝对路径。AEM进程必须能够读取语言包文件夹中的所有文件。
      * `Node types` 是候选节点类型，其全文搜索将与此语言包交互进行翻译。
      * `Minimum score` 是要使用的已翻译术语的最低置信度得分。

         * 例如，本位（西班牙语为“man”）可以译成带有`0.9`置信度分数的英语单词“man”，也可以译成带有置信度分数`0.2`的英语单词“human”。 将最低得分调整为`0.3`，将保留“hombre”到“man”的翻译，但将“hombre”转换为“human”，因为此`0.2`的翻译得分小于`0.3`的最低得分。

5. 对资产执行全文搜索
   * 因为dam:Asset是重新注册此语言包的节点类型，我们必须使用全文搜索搜索来搜索AEM Assets以验证此类型。
   * 导航到AEM >资产，然后打开Omnisearch。 以已安装语言包的语言搜索术语。
   * 根据需要，调整OSGi配置中的最低得分以确保结果的准确性。

6. 更新语言包
   * Apache Joshua语言包由Apache Joshua项目完整维护，其更新或更正由Apache Joshua项目自行决定。
   * 如果语言包已更新，为了在AEM中安装更新，必须执行上述步骤2 - 4，根据需要向上或向下调整堆大小。

      * 请注意，将解压缩的语言包移至crx-quickstart/opt文件夹时，请移动任何现有语言包文件夹，然后再复制到新文件夹中。
   * 如果AEM不需要重新启动，则必须重新保存与更新的语言包相关的相关Apache Jackrabbit Oak Machine翻译全文查询条款提供程序OSGi配置，以便AEM处理更新的文件。


## 更新damAssetLucene索引{#updating-damassetlucene-index}

要使[AEM智能标记](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)受AEM智能翻译影响，必须更新AEM `/oak   :index  /damAssetLucene`索引，以将预测的标记（“智能标记”的系统名称）标记为资产聚合Lucene索引的一部分。

在`/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`下，确保配置如下：

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

* [Apache Oak Search Machine Translation OSGi bundle](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua Language Pack](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM智能标记](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [查询和索引的最佳实践](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)