---
title: 使用AEM Assets設定智慧型翻譯搜尋
description: 智慧型翻譯搜尋允許使用非英文搜尋辭彙來解析成英文內容。 若要設定AEM以進行智慧型翻譯搜尋，必須安裝並設定Apache Oak Search Machine Translation OSGi套件組合，以及包含翻譯規則的相關免費開放原始碼Apache Joshua語言套件。
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

# 使用AEM Assets設定智慧型翻譯搜尋{#set-up-smart-translation-search-with-aem-assets}

智慧型翻譯搜尋允許使用非英文搜尋辭彙來解析成英文內容。 若要設定AEM以進行智慧型翻譯搜尋，必須安裝並設定Apache Oak Search Machine Translation OSGi套件組合，以及包含翻譯規則的相關免費開放原始碼Apache Joshua語言套件。

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>必須在需要智慧型翻譯搜尋的每個AEM執行個體上設定它。

1. 下載並安裝Oak Search Machine Translation OSGi套件
   * [下載Oak Search Machine Translation OSGi套件](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) 對應至AEM Oak版本。
   * 透過將下載的Oak Search Machine Translation OSGi套件組合安裝到AEM中 [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. 下載並更新Apache Joshua語言套件
   * 下載並解壓縮所需的 [Apache Joshua語言套件](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * 編輯 `joshua.config` 檔案，並註釋掉以下列開頭的2行：

      ```
      feature-function = LanguageModel ...
      ```

   * 判斷並記錄語言套件模型資料夾的大小，因為這會影響AEM所需的額外棧積空間量。
   * 移動解壓縮的Apache Joshua語言套件資料夾(使用 `joshua.config` 編輯)

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      例如：

      ```
       .../crx-quickstart/opt/es-en
      ```

3. 使用更新的棧積記憶體配置重新啟動AEM
   * 停止AEM
   * 決定AEM所需的新棧積大小

      * AEM缺乏語言前的棧積大小+模型目錄的大小四捨五入到最接近的2GB
      * 例如：如果預先安裝語言套件時，AEM安裝需要8GB的棧積才能執行，而語言套件的模型資料夾為3.8GB未壓縮，則新的棧積大小為：

         原始 `8GB` + ( `3.75GB` 四捨五入到最接近的值 `2GB`，亦即 `4GB`)，總共 `12GB`
   * 驗證電腦是否有此數量的額外可用記憶體。
   * 更新AEM啟動指令碼以調整新的棧積大小

      * 例如. `java -Xmx12g -jar cq-author-p4502.jar`
   * 以增加的棧積大小重新啟動AEM。

   >[!NOTE]
   >
   >語言套件所需的棧積空間可能會變得很大，尤其是使用多個語言套件時。
   >
   >
   >一律確定 **執行個體有足夠的記憶體** 以因應配置棧積空間的增加。
   >
   >
   >此 **必須一律計算基礎棧積以支援可接受的效能，而不使用任何語言套件** 已安裝。

4. 透過Apache Jackrabbit Oak機器翻譯全文查詢條款提供者OSGi設定註冊語言套件

   * 對於每個語言套件， [建立新的Apache Jackrabbit Oak機器翻譯全文查詢條款提供者OSGi設定](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) 透過AEM Web主控台的Configuration Manager。

      * `Joshua Config Path` 是joshua.config檔案的絕對路徑。 AEM程式必須能夠讀取語言套件資料夾中的所有檔案。
      * `Node types` 是全文檢索搜尋將使用此語言套件進行翻譯的候選節點型別。
      * `Minimum score` 是要使用的翻譯辭彙的最低信賴分數。

         * 例如，hombre （西班牙文中的「man」）可翻譯成英文單詞「man」，其信賴分數為 `0.9` 加上信賴分數，即可翻譯成human這個英文單字 `0.2`. 將最低分數調整為 `0.3`，會保留「hombre」到「man」的翻譯，但捨棄「hombre」到「human」的翻譯，因為此翻譯分數為 `0.2` 小於的最低分數 `0.3`.

5. 對資產執行全文搜尋
   * 由於dam：Asset是此語言套件再次註冊的節點型別，我們必須使用全文搜尋來搜尋AEM Assets以驗證此功能。
   * 導覽至「AEM >資產」，然後開啟Omnisearch。 以已安裝語言套件的語言搜尋字詞。
   * 如有需要，請調整OSGi設定中的最低分數，以確保結果的準確性。

6. 更新語言套件
   * Apache Joshua語言套件是由Apache Joshua專案所維護，其更新或更正由Apache Joshua專案自行決定。
   * 如果更新了語言套件，為了在AEM中安裝更新，必須執行上述步驟2到4，視需要調整棧積大小。

      * 請注意，將解壓縮的語言套件移動到crx-quickstart/opt資料夾時，請先移動任何現有的語言套件資料夾，然後再複製成新的語言套件。
   * 如果AEM不需要重新啟動，則必須重新儲存與更新語言套件相關的相關Apache Jackrabbit Oak Machine翻譯全文查詢條款提供者OSGi設定，以便AEM處理更新的檔案。


## 更新damAssetLucene索引 {#updating-damassetlucene-index}

為了 [AEM智慧標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) 將受AEM Smart Translation、AEM影響 `/oak   :index  /damAssetLucene` 必須更新索引，才能將predictedTags （「智慧標籤」的系統名稱）標示為資產彙總Lucene索引的一部分。

下 `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`，請確定設定如下：

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

* [Apache Oak Search Machine Translation OSGi套件](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua語言套件](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM智慧標籤](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [查詢和建立索引的最佳實務](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
