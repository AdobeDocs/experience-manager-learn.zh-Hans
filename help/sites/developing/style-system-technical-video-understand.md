---
title: 瞭解如何為AEM樣式系統編寫程式碼
description: 在這段影片中，我們將瞭解CSS （或LESS）和JavaScript的剖析（用於使用樣式系統設定Adobe Experience Manager的核心標題元件的樣式），以及這些樣式如何套用至HTML和DOM。
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 1%

---

# 瞭解如何為樣式系統編寫程式碼{#understanding-how-to-code-for-the-aem-style-system}

在本影片中，我們將瞭解CSS的剖析(或 [!DNL LESS])和JavaScript，用來使用樣式系統設定Experience Manager核心標題元件的樣式，以及這些樣式套用至HTML和DOM的方式。


## 瞭解如何為樣式系統編寫程式碼 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

提供的AEM套件(**technical-review.sites.style-system-1.0.0.zip**)安裝範例標題樣式、We.Retail配置容器和標題元件的範例原則，以及範例頁面。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下為 [!DNL LESS] 範例樣式的定義，位於：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

對於偏好CSS的使用者，此程式碼片段下方是CSS [!DNL LESS] 編譯至。

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

以上 [!DNL LESS] 會透過Experience Manager至下列CSS以原生方式編譯。

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### JavaScript {#example-javascript}

將範例樣式套用至Title元件時，下列JavaScript會收集目前頁面的上次修改日期及時間，並將其插入至標題文字下方。

jQuery的使用是選用的，也可以選擇使用的命名慣例。

以下為 [!DNL LESS] 範例樣式的定義，位於：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## 開發最佳實務 {#development-best-practices}

### HTML最佳實務 {#html-best-practices}

* HTML（透過HTL產生）應儘可能在結構上符合語意，以避免對元素進行不必要的分組/巢狀化。
* HTML元素應可透過BEM樣式CSS類別進行定址。

**好**  — 元件中的所有元素均可透過BEM標籤法來定址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**不良**  — 清單和清單元素僅可依元素名稱定址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 揭露更多資料並隱藏資料總比揭露太少的資料（未來需要後端開發）要好。

   * 實作可製作的內容切換可協助保持此HTML簡潔，讓作者可以選擇將哪些內容元素寫入HTML。 在將影像寫入可能無法用於所有樣式的HTML時，可能特別重要。
   * 此規則的例外情況是預設會公開昂貴的資源（例如影像），因為在此情況下，會不必要擷取CSS隱藏的事件影像。

      * 現代影像元件通常會使用JavaScript來選取和載入最適合使用案例（檢視區）的影像。

### CSS最佳作法 {#css-best-practices}

>[!NOTE]
>
>樣式系統造成技術上的小差異 [BEM](https://en.bem.info/)，其中 `BLOCK` 和 `BLOCK--MODIFIER` 不會套用至指定的相同元素 [BEM](https://en.bem.info/).
>
>由於產品限制， `BLOCK--MODIFIER` 套用至的父項 `BLOCK` 元素。
>
>所有其他租使用者 [BEM](https://en.bem.info/) 應與「 」對齊。

* 使用前置處理器，例如 [更少](https://lesscss.org/) (AEM原生支援)或 [SCSS](https://sass-lang.com/) （需要自訂建置系統）才能清除CSS定義並重複使用。

* 保持選取器權數/特性一致；這有助於避免和解決難以識別的CSS重疊顯示衝突。
* 將每個樣式組織成獨立檔案。
   * 這些檔案可以使用LESS/SCSS合併 `@imports` 或如果需要原始CSS，可透過HTML使用者端資料庫檔案包含或自訂前端資產建置系統。
* 避免混合許多複雜的樣式。
   * 可一次套用至元件的樣式越多，排列的多樣性就越多。 這可能變得難以維護/QA/確保品牌一致性。
* 一律使用CSS類別（遵循BEM標籤法）來定義CSS規則。
   * 如果選取不含CSS類別的元素（即裸元素）是絕對必要的，請在CSS定義中將其移至較高的位置，以清楚表示其專屬性低於與該型別元素發生具有可選取CSS類別的衝突。
* 避免樣式化 `BLOCK--MODIFIER` 直接建立，因為它已附加至回應式格線。 變更此元素的顯示可能會影響回應式格線的演算和功能，因此只有在意圖變更回應式格線的行為時，才會在此層級設定樣式。
* 套用樣式範圍，使用 `BLOCK--MODIFIER`. 此 `BLOCK__ELEMENT--MODIFIERS` 可用於元件中，但由於 `BLOCK` 代表元件，而元件是設定樣式的專案，樣式是透過下列方式「定義」和設定範圍 `BLOCK--MODIFIER`.

CSS選取器結構範例應如下所示：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第1層選擇器</p> <p>區塊 — 修飾元</p> </td> 
   <td valign="bottom"><p>第2層級選擇器</p> <p>區塊</p> </td> 
   <td valign="bottom"><p>第3層選擇器</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">有效CSS選取器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list — 深色</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list — 深色</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> color： blue；</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> color： red；</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

若是巢狀元件，這些巢狀元件元素的CSS選取器深度將超過第3層級選取器。 對巢狀元件重複相同的模式，但範圍為父元件的範圍 `BLOCK`. 換句話說，啟動巢狀元件的 `BLOCK` 位於第3層級，而巢狀元件的 `ELEMENT` 位於第4個選擇器層級。

### JavaScript最佳作法 {#javascript-best-practices}

本節中定義的最佳實務與「style-JavaScript」有關，或JavaScript的專屬目的是為了文體而非功能目的而操作元件。

* Style-JavaScript應謹慎使用，且為少數使用案例。
* Style-JavaScript主要用於操控元件的DOM，以支援CSS的樣式設定。
* 如果元件會在頁面上出現多次，請重新評估Javascript的使用，並瞭解運算成本/和重新繪製成本。
* 如果元件在頁面上可能出現多次時，Javascript非同步(透過AJAX)提取新資料/內容，則重新評估其使用。
* 處理發佈和編寫體驗。
* 儘可能重複使用style-Javascript。
   * 例如，如果元件的多個樣式需要將影像移至背景影像，style-JavaScript可以實作一次，然後附加至多個 `BLOCK--MODIFIERs`.
* 儘可能將style-JavaScript與功能性JavaScript分開。
* 評估JavaScript的成本與直接透過HTL在HTML中顯示這些DOM變更的成本。
   * 當使用style-JavaScript的元件需要伺服器端修改時，請評估此時是否可以引入JavaScript操作，以及這對元件的效能和可支援性有何影響/影響。

#### 效能考量事項 {#performance-considerations}

* Style-JavaScript應保持精簡和簡潔。
* 若要避免忽隱忽現及不必要的重新繪製，請先透過以下方式隱藏元件： `BLOCK--MODIFIER BLOCK`，並在JavaScript中的所有DOM操作完成時顯示。
* style-JavaScript操作的效能類似於附加及修改DOMReady上元素的基本jQuery外掛程式。
* 確保請求已壓縮，且CSS和JavaScript已縮制。

## 其他资源 {#additional-resources}

* [樣式系統檔案](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [建立AEM使用者端資料庫](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM （區塊元素修飾元）檔案網站](https://getbem.com/)
* [較少說明檔案網站](https://lesscss.org/)
* [jQuery網站](https://jquery.com/)
