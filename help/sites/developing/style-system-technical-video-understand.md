---
title: 了解如何为AEM样式系统编码
description: 在本视频中，我们将介绍CSS（或LESS）和JavaScript的剖析，这些样式用于通过样式系统为AdobeExperience Manager的核心标题组件设置样式，以及这些样式如何应用于HTML和DOM。
feature: Style System
version: 6.4, 6.5, Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1125
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# 了解如何为样式系统编码{#understanding-how-to-code-for-the-aem-style-system}

在本视频中，我们将介绍CSS的剖析(或 [!DNL LESS])和JavaScript，用于通过样式系统为Experience Manager的核心标题组件设置样式，以及这些样式如何应用于HTML和DOM。


## 了解如何为样式系统编码 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

提供的AEM包(**technical-review.sites.style-system-1.0.0.zip**)安装示例标题样式、We.Retail布局容器和标题组件的示例策略以及示例页面。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下是 [!DNL LESS] 在以下位置找到的示例样式的定义：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

对于首选CSS的代码，此代码片段的下面是CSS [!DNL LESS] 编译到。

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

以上 [!DNL LESS] 通过Experience Manager到以下CSS在本地编译。

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

将示例样式应用于标题组件时，以下JavaScript将在标题文本下方收集并注入当前页面的最后修改日期和时间。

jQuery的使用以及使用的命名约定都是可选的。

以下是 [!DNL LESS] 在以下位置找到的示例样式的定义：

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

## 开发最佳实践 {#development-best-practices}

### HTML最佳实践 {#html-best-practices}

* HTML（通过HTL生成）应尽可能具有结构语义；避免对元素进行不必要的分组/嵌套。
* HTML元素应可通过BEM样式的CSS类进行寻址。

**好**  — 组件中的所有元素均可通过BEM表示法寻址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**不良**  — 列表和列表元素只能按元素名称进行寻址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 公开更多数据并隐藏这些数据比公开太少需要未来后端开发才能公开的数据要好。

   * 实施可创作内容切换可有助于保持此HTML简洁，从而使作者能够选择将哪些内容元素写入HTML。 在将图像写入可能无法用于所有样式的HTML时，可能特别重要。
   * 此规则的例外情况是默认情况下会公开昂贵的资源（例如图像），因为在此例中，CSS隐藏的事件图像会被不必要地获取。

      * 现代图像组件通常将使用JavaScript选择和加载最适合用例（视区）的图像。

### CSS最佳做法 {#css-best-practices}

>[!NOTE]
>
>样式系统与 [BEM](https://en.bem.info/)，在 `BLOCK` 和 `BLOCK--MODIFIER` 未应用于指定的相同元素 [BEM](https://en.bem.info/).
>
>相反，由于产品限制， `BLOCK--MODIFIER` 应用于的父项 `BLOCK` 元素。
>
>所有其他租户 [BEM](https://en.bem.info/) 应该与。

* 使用预处理器，例如 [更少](https://lesscss.org/) (由AEM本机支持)或 [SCSS](https://sass-lang.com/) （需要自定义构建系统）以允许清除CSS定义以及可重复使用。

* 保持选择器权重/特异性一致；这有助于避免和解决难以识别的CSS级联冲突。
* 将每种样式组织为一个独立文件。
   * 可使用LESS/SCSS组合这些文件 `@imports` 或者，如果需要原始CSS，则通过HTML客户端库文件包含或自定义前端资源构建系统。
* 避免混合使用许多复杂的样式。
   * 可以一次性应用于某个组件的样式越多，排列的多样性就越大。 这可能变得难以维护/QA/确保品牌一致性。
* 始终使用CSS类（遵循BEM表示法）来定义CSS规则。
   * 如果绝对有必要选择不含CSS类的元素（即裸元素），请在CSS定义中将它们移到较高位置，以清楚地表明它们的特定性比与该类型具有可选择CSS类的元素的任何冲突都要低。
* 避免设置 `BLOCK--MODIFIER` 直接访问，因为它已附加到响应式网格。 更改此元素的显示可能会影响响应式网格的渲染和功能，因此，仅当要更改响应式网格的行为时，才应在此级别设置样式。
* 使用以下方式应用样式范围 `BLOCK--MODIFIER`. 此 `BLOCK__ELEMENT--MODIFIERS` 可以在组件中使用，但由于 `BLOCK` 表示组件，组件是已设置样式的组件，样式是“定义的”，其作用范围通过 `BLOCK--MODIFIER`.

CSS选择器结构示例应如下所示：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第1级选择器</p> <p>块 — 修饰符</p> </td> 
   <td valign="bottom"><p>第二级选择器</p> <p>块</p> </td> 
   <td valign="bottom"><p>第3级选择器</p> <p>块__素</p> </td> 
   <td> </td> 
   <td valign="middle">有效的CSS选择器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list — 深色</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list — 深色</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> 颜色：蓝色；</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> 颜色：红色；</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

对于嵌套组件，这些嵌套组件元素的CSS选择器深度将超过第三级选择器。 对嵌套组件重复相同的模式，但范围由父组件的 `BLOCK`. 换句话说，启动嵌套组件的 `BLOCK` 位于第3级，而嵌套组件的 `ELEMENT` 位于第4选择器级别。

### javascript最佳实践 {#javascript-best-practices}

此部分中定义的最佳实践与“style-JavaScript”或JavaScript相关，后者专门用于处理组件以达到风格目的而非功能目的。

* Style-JavaScript应谨慎使用，并且是少数用例。
* Style-JavaScript主要用于处理组件的DOM以支持CSS的样式。
* 如果组件在页面上出现多次，请重新评估Javascript的使用，并了解计算/和重新绘制成本。
* 如果Javascript异步(通过AJAX)引入新数据/内容，并且组件可能会在页面上出现多次，请重新评估其使用情况。
* 处理发布和创作体验。
* 尽可能重复使用style-Javascript。
   * 例如，如果组件的多种样式要求将其图像移动到背景图像，则可以实施一次style-JavaScript并将其附加到多个 `BLOCK--MODIFIERs`.
* 如果可能，请将style-JavaScript与功能性JavaScript分开。
* 评估JavaScript的成本与直接通过HTL在HTML中显示这些DOM更改的成本。
   * 当使用style-JavaScript的组件需要服务器端修改时，请评估此时是否可以引入JavaScript操作，以及这对组件的性能和可支持性有何影响/影响。

#### 性能注意事项 {#performance-considerations}

* Style-JavaScript应保持精简和简洁。
* 为避免闪烁和不必要的重新绘制，最初通过以下方式隐藏组件： `BLOCK--MODIFIER BLOCK`，并在JavaScript中的所有DOM操作完成时显示它。
* style-JavaScript操作的性能类似于附加和修改DOMReady上的元素的基本jQuery插件。
* 确保请求已压缩，并且CSS和JavaScript已缩小。

## 其他资源 {#additional-resources}

* [样式系统文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [创建AEM客户端库](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（块元素修饰符）文档网站](https://getbem.com/)
* [LESS文档网站](https://lesscss.org/)
* [jQuery网站](https://jquery.com/)
