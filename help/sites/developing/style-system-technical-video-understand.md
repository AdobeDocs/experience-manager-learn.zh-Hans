---
title: 了解如何编码AEM样式系统
description: 在此视频中，我们将了解用于使用样式系统来设置Adobe Experience Manage核心标题组件样式的CSS（或更低版本）和JavaScript的结构，以及这些样式如何应用于HTML和DOM。
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 0%

---


# 了解如何对样式系统进行代码{#understanding-how-to-code-for-the-aem-style-system}

在此视频中，我们将了解用于使用样式系统来设置Experience Manage核心标题组件样式的CSS（或[!DNL LESS]）和JavaScript的解剖结构，以及这些样式如何应用于HTML和DOM。


## 了解如何对样式系统进行代码 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=12&learn=on)

提供的AEM包(**technical-review.sites.style-system-1.0.0.zip**)安装示例标题样式、We.Retail布局容器和标题组件的示例策略以及示例页面。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下是位于的示例样式的[!DNL LESS]定义：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

对于偏好CSS的用户，此代码片段下面是此[!DNL LESS]编译到的CSS。

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

上述[!DNL LESS]由Experience Manager到以下CSS在本地编译。

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

当将示例样式应用于标题组件时，以下JavaScript会收集并注入当前页面在标题文本下方的上次修改日期和时间。

可以选择使用jQuery以及使用的命名约定。

以下是位于的示例样式的[!DNL LESS]定义：

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

* HTML（通过HTL生成）应尽可能具有结构语义；避免元素的不必要分组/嵌套。
* HTML元素应可通过BEM样式的CSS类寻址。

**良好**  — 组件中的所有元素均可通过BEM符号进行寻址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**错误**  — 列表和列表元素只能通过元素名称寻址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 更好的做法是公开更多数据并隐藏这些数据，而不是公开太少的数据，这需要未来的后端开发来公开这些数据。

   * 实施可创作内容切换有助于保持此HTML的精简，从而作者能够选择将哪些内容元素写入HTML。 在将图像写入HTML时，可能并非所有样式都使用的图像时，可能尤其重要。
   * 此规则的例外情况是，默认情况下会显示昂贵的资源（例如图像），因为CSS隐藏的事件图像将（在这种情况下）不必要地获取。

      * 现代图像组件通常会使用JavaScript为用例（视区）选择并加载最合适的图像。

### CSS最佳实践 {#css-best-practices}

>[!NOTE]
>
>样式系统与[BEM](https://en.bem.info/)有小的技术差异，因为`BLOCK`和`BLOCK--MODIFIER`未应用于[BEM](https://en.bem.info/)指定的同一元素。
>
>由于产品限制，`BLOCK--MODIFIER`将应用于`BLOCK`元素的父元素。
>
>[BEM](https://en.bem.info/)的所有其他租户都应与对齐。

* 使用预处理器，如[LESS](https://lesscss.org/)(本地受AEM支持)或[SCSS](https://sass-lang.com/)（需要自定义构建系统），以便清晰定义CSS并重新使用。

* 保持选择器重量/特异性一致；这有助于避免和解决难以识别的CSS级联冲突。
* 将每种样式组织为一个离散的文件。
   * 这些文件可以使用LESS/SCSS `@imports`进行组合，如果需要原始CSS，也可以通过HTML客户端库文件包含或自定义前端资产构建系统进行组合。
* 避免混用许多复杂的样式。
   * 一次可以应用于组件的样式越多，排列的类型就越多。 这可能会变得难以维护/QA/确保品牌一致性。
* 始终使用CSS类（遵循BEM符号）来定义CSS规则。
   * 如果绝对需要选择不带CSS类的元素（即裸机元素），请在CSS定义中将其移到较高位置，以明确表示它们的特异性低于与具有可选CSS类的此类元素的任何冲突。
* 当`BLOCK--MODIFIER`附加到响应式网格时，请避免直接为其设置样式。 更改此元素的显示可能会影响响应式网格的呈现和功能，因此当更改响应式网格的行为的意图时，仅会影响此级别的样式。
* 使用`BLOCK--MODIFIER`应用样式范围。 `BLOCK__ELEMENT--MODIFIERS`可以在组件中使用，但由于`BLOCK`表示组件，并且组件是样式设置的，因此样式为“defined”，并通过`BLOCK--MODIFIER`的范围。

CSS选择器结构示例应如下所示：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>第1级选择器</p> <p>块 — 修饰符</p> </td> 
   <td valign="bottom"><p>第2级选择器</p> <p>块</p> </td> 
   <td valign="bottom"><p>3级选择器</p> <p>块__元素</p> </td> 
   <td> </td> 
   <td valign="middle">有效的CSS选择器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list-dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list-dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item {  </span></strong></p> <p><strong> 颜色：蓝色；</strong></p> <p><strong> }</strong></p> </td> 
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

对于嵌套的组件，这些嵌套的组件元素的CSS选择器深度将超过第3级选择器。 为嵌套组件重复相同的模式，但范围由父组件的`BLOCK`确定。 换言之，在第3级启动嵌套组件的`BLOCK`，而嵌套组件的`ELEMENT`将在第4个选择器级别启动。

### JavaScript最佳实践 {#javascript-best-practices}

本节中定义的最佳实践与“style-JavaScript”或专门用于处理组件以实现风格而非功能目的的JavaScript有关。

* Style-JavaScript应谨慎使用，属于少数用例。
* Style-JavaScript主要用于处理组件的DOM以支持由CSS设置样式。
* 如果组件在页面上出现多次，请重新评估Javascript的使用情况，并了解计算/重绘成本。
* 如果组件在页面上可能显示多次，则Javascript会异步(通过AJAX)提取新数据/内容，从而重新评估其使用情况。
* 处理发布和创作体验。
* 尽可能重复使用样式Javascript。
   * 例如，如果组件的多个样式要求将其图像移动到背景图像，则可以实施style-JavaScript一次，并将其附加到多个`BLOCK--MODIFIERs`。
* 尽可能将style-JavaScript与功能性JavaScript分开。
* 评估JavaScript的成本与通过HTL直接在HTML中显示这些DOM更改的成本。
   * 当使用style-JavaScript的组件需要服务器端修改时，请评估此时是否可以引入JavaScript操作，以及这些操作/影响对组件的性能和支持性有何影响。

#### 性能注意事项 {#performance-considerations}

* Style-JavaScript应保持轻薄。
* 为避免闪烁和不必要的重绘，最初通过`BLOCK--MODIFIER BLOCK`隐藏组件，并在JavaScript中的所有DOM操作完成时显示组件。
* style-JavaScript操作的性能类似于附加到DOMReady上的元素并对其进行修改的基本jQuery插件。
* 确保对请求进行压缩，并缩小CSS和JavaScript。

## 其他资源 {#additional-resources}

* [样式系统文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [创建AEM客户端库](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（块元素修饰符）文档网站](https://getbem.com/)
* [LESS文档网站](https://lesscss.org/)
* [jQuery网站](https://jquery.com/)
