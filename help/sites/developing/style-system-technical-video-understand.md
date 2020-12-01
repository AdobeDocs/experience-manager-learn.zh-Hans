---
title: 了解如何为AEM Style System编码
description: 在此视频中，我们将了解CSS（或更少）和JavaScript的剖析，它们用于使用样式系统设计Adobe Experience Manage的核心标题组件的样式，以及这些样式如何应用于HTML和DOM。
feature: style-system
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 664d3964df796d508973067f8fa4fe5ef83c5fec
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 1%

---


# 了解如何编写样式系统的代码{#understanding-how-to-code-for-the-aem-style-system}

在此视频中，我们将了解CSS（或[!DNL LESS]）和JavaScript的剖析，它们使用样式系统来设计Experience Manage核心标题组件的样式，以及这些样式如何应用于HTML和DOM。

>[!NOTE]
>
>AEM Style System是随[AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp1-release-notes.html) + [功能包20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593)引入的。
>
>视频假定We.Retail Title组件已更新为继承[核心组件v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases)。

## 了解如何编写样式系统{#understanding-how-to-code-for-the-style-system}的代码

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

提供的AEM包(**technical-review.sites.style-system-1.0.0.zip**)安装示例标题样式、We.Retail布局容器和标题组件的示例策略以及示例页。

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

以下是示例样式的[!DNL LESS]定义：

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

对于喜欢CSS的人，此代码片断下面是此[!DNL LESS]编译为的CSS。

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

以上[!DNL LESS]是通过Experience Manager到以下CSS本机编译的。

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

将示例样式应用于标题组件时，以下JavaScript会收集并注入当前页面的上次修改日期和时间，并将其注入到标题文本下。

使用jQuery是可选的，也是使用的命名约定。

以下是示例样式的[!DNL LESS]定义：

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

## 开发最佳实践{#development-best-practices}

### HTML最佳实践{#html-best-practices}

* HTML（通过HTL生成）应尽可能具有结构语义；避免元素的不必要的分组／嵌套。
* HTML元素应可通过BEM样式的CSS类寻址。

**良好** -组件中的所有元素都可通过边界元记号寻址：

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**错误** -列表和列表元素只能通过元素名称寻址：

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 暴露更多数据和隐藏数据要比暴露太少的数据（需要未来后端开发才能公开）更好。

   * 实施可创作的内容切换有助于保持此HTML的精简性，从而作者能够选择将哪些内容元素写入HTML。 在将图像写入HTML时（可能并非用于所有样式），这一点尤其重要。
   * 此规则的例外情况是，默认情况下会公开昂贵的资源（例如图像），因为CSS隐藏的事件图像将不必要地获取。

      * 现代图像组件通常使用JavaScript来选择和加载最适合用例（视图端口）的图像。

### CSS最佳实践{#css-best-practices}

>[!NOTE]
>
>样式系统与[BEM](https://en.bem.info/)有很小的技术差异，即`BLOCK`和`BLOCK--MODIFIER`不应用于由[BEM](https://en.bem.info/)指定的同一元素。
>
>而是由于产品约束，`BLOCK--MODIFIER`应用于`BLOCK`元素的父项。
>
>应将[BEM](https://en.bem.info/)的所有其他租户对准。

* 使用预处理器，如[LESS](https://lesscss.org/)(受AEM本机支持)或[SCSS](https://sass-lang.com/)（需要自定义构建系统），可以清晰定义CSS并重新使用。

* 保持选择器权重/特异性一致；这有助于避免和解决难以识别的CSS层叠冲突。
* 将每种样式组织到一个离散文件中。
   * 这些文件可以使用LESS/SCSS `@imports`组合，如果需要原始CSS，可以通过HTML客户端库文件包含或自定义前端资源构建系统组合。
* 避免混用许多复杂的样式。
   * 一次可应用于组件的样式越多，排列的种类越多。 这可能变得难以维护/QA/确保品牌协调。
* 始终使用CSS类（遵循BEM记号）来定义CSS规则。
   * 如果绝对需要选择不带CSS类的元素（即裸元素），请在CSS定义中将它们移到较高位置，以明确它们与具有可选CSS类的元素的任何冲突相比，具有较低的特异性。
* 请避免直接为`BLOCK--MODIFIER`设置样式，因为它已附加到响应式网格。 更改此元素的显示可能会影响响应式网格的呈现和功能，因此当更改响应式网格的行为时，只有此级别的样式才会受到影响。
* 使用`BLOCK--MODIFIER`应用样式范围。 `BLOCK__ELEMENT--MODIFIERS`可用于组件中，但由于`BLOCK`表示组件，并且组件是样式，样式是“定义的”，并通过`BLOCK--MODIFIER`作用范围。

CSS选择器结构示例应如下：

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>1级选择器</p> <p>块——修饰符</p> </td> 
   <td valign="bottom"><p>2级选择器</p> <p>块</p> </td> 
   <td valign="bottom"><p>3级选择器</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">有效的CSS选择器</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-列表-深色</span></td> 
   <td valign="middle"><span class="code">.cmp-列表</span></td> 
   <td valign="middle"><span class="code">.cmp-列表__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-列表-深色</span></p> <p><span class="code"> .cmp-列表</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-列表__item {  </span></strong></p> <p><strong> 颜色：蓝色；</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> 颜色：红色；</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

对于嵌套组件，这些嵌套组件元素的CSS选择器深度将超过第3级选择器。 为嵌套组件重复相同的模式，但范围由父组件的`BLOCK`确定。 换言之，将嵌套组件的`BLOCK`开始到第3级，而嵌套组件的`ELEMENT`将位于第4个选择器级别。

### JavaScript最佳实践{#javascript-best-practices}

本节中定义的最佳实践与“style-JavaScript”或JavaScript有关，JavaScript专门用于为风格而非功能目的操作组件。

* Style-JavaScript应慎重使用，是少数用例。
* Style-JavaScript应主要用于处理组件的DOM以支持CSS设计样式。
* 如果组件在页面上出现多次，请重新评估Javascript的使用，并了解计算／重绘成本。
* 如果当组件可能多次出现在页面上时，以异步方式(通过AJAX)导入新数据／内容，请重新评估Javascript的使用。
* 处理发布和创作体验。
* 尽可能重用样式Javascript。
   * 例如，如果某个组件的多个样式要求将其图像移动到背景图像，则可以实现style-JavaScript一次，并将其附加到多个`BLOCK--MODIFIERs`。
* 尽可能将style-JavaScript与功能JavaScript分开。
* 评估JavaScript与直接通过HTL在HTML中体现这些DOM更改的成本。
   * 当使用样式JavaScript的组件需要服务器端修改时，请评估此时是否可以引入JavaScript操作，以及影响／影响组件的性能和支持性。

#### 性能注意事项{#performance-considerations}

* Style-JavaScript应保持轻松。
* 要避免闪烁和不必要的重绘，请首先通过`BLOCK--MODIFIER BLOCK`隐藏组件，并在JavaScript中的所有DOM操作完成时显示它。
* 样式-JavaScript操作的性能类似于在DOMRady上附加和修改元素的基本jQuery插件。
* 确保对请求进行压缩，并缩小CSS和JavaScript。

## 其他资源 {#additional-resources}

* [样式系统文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [创建AEM客户端库](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（块元素修改工具）文档网站](https://getbem.com/)
* [LESS文档网站](https://lesscss.org/)
* [jQuery网站](https://jquery.com/)
