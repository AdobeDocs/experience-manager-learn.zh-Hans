---
title: 了解使用AEM Sites的样式系统最佳实践
description: 一篇详细的文章，介绍在使用Adobe Experience Manager Sites实施样式系统时的最佳实践。
feature: 样式系统
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: 开发
role: 开发人员
level: 中级，经验丰富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1544'
ht-degree: 2%

---


# 了解样式系统最佳实践{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>请阅读[了解如何为样式系统编码](style-system-technical-video-understand.md)中的内容，以确保理解AEM样式系统使用的类似BEM的约定。

为AEM Style System实现了两种主要风格或样式：

* **布局样式**
* **显示样式**

**布局** 样式会影响组件的许多元素，以创建组件的良好定义和可识别的再现（设计和布局），通常与特定的可重用品牌概念相协调。例如，Teaser组件可以以基于卡的传统布局、水平提升样式或图像上覆盖文本的Hero布局显示。

**显** 示样式用于影响布局样式的细微变化，但它们不会更改布局样式的基本性质或用途。例如，Hero布局样式可能具有将颜色方案从主品牌颜色方案更改为辅助品牌颜色方案的显示样式。

## 设置组织最佳实践{#style-organization-best-practices}

定义可供AEM作者使用的样式名称时，最好：

* 使用作者所理解的词汇命名样式
* 将样式选项的数量减至最少
* 仅展示品牌标准允许的样式选项和组合
* 仅显示有效的样式组合
   * 如果暴露了无效的组合，则确保它们至少不会产生不良效果

随着AEM作者可用的样式组合数量的增加，必须根据品牌标准进行QA和验证的排列越多。 太多的选项也会让作者感到困惑，因为可能不清楚产生所需效果需要哪个选项或组合。

### 样式名称与CSS类{#style-names-vs-css-classes}

样式名称或呈现给AEM作者的选项，以及实现CSS类名称在AEM中是解耦的。

这样，CSS开发者就可以以未来验证的语义方式命名CSS类，而Style选项可以在词汇中清晰地标记并被AEM作者所理解。 例如：

组件必须具有用品牌的&#x200B;**主**&#x200B;和&#x200B;**次**&#x200B;颜色着色的选项，但AEM作者知道颜色为&#x200B;**绿色**&#x200B;和&#x200B;**黄色**，而不是主和次设计语言。

AEM样式系统可以使用适合作者的标签&#x200B;**Green**&#x200B;和&#x200B;**Yellow**&#x200B;来显示这些颜色显示样式，同时允许CSS开发人员使用`.cmp-component--primary-color`和`.cmp-component--secondary-color`的语义命名来定义CSS中的实际样式实现。

将&#x200B;**Green**&#x200B;的样式名称映射到`.cmp-component--primary-color`，将&#x200B;**Yellow**&#x200B;映射到`.cmp-component--secondary-color`。

如果公司的品牌颜色在将来发生变化，则需要更改的所有内容都是`.cmp-component--primary-color`和`.cmp-component--secondary-color`的单个实现以及样式名称。

## Teaser组件作为示例用例{#the-teaser-component-as-an-example-use-case}

以下是设置Teaser组件样式以具有多种不同布局和显示样式的示例用例。

这将探索样式名称（向作者公开）的组织方式以及支持CSS类的组织方式。

### Teaser组件样式配置{#component-styles-configuration}

下图显示了用例中讨论的变量的Teaser组件的[!UICONTROL Styles]配置。

[!UICONTROL 样式组]名称、布局和显示（偶然）与用于对本文中的样式类型进行概念分类的显示样式和布局样式的一般概念相匹配。

[!UICONTROL 样式组]名称和[!UICONTROL 样式组]的编号应根据组件用例和特定于项目的组件样式约定进行定制。

例如，**Display**&#x200B;样式组名称可能已命名为&#x200B;**Colors**。

![显示样式组](assets/style-config.png)

### 样式选择菜单{#style-selection-menu}

下图显示了作者与之交互的[!UICONTROL Style]菜单，以选择组件的相应样式。 请注意，[!UICONTROL 样式图形]名称以及样式名称都向作者公开。

![样式下拉菜单](assets/style-menu.png)

### 默认样式{#default-style}

默认样式通常是组件最常用的样式，在添加到页面时是Teaser的默认未设置样式的视图。

根据默认样式的通用性，CSS可以直接应用于`.cmp-teaser`（不带任何修饰符）或`.cmp-teaser--default`。

如果默认样式规则比不应用于所有变体更频繁，最好使用`.cmp-teaser`作为默认样式的CSS类，因为所有变体都应隐式继承它们，假定遵循类似BEM的约定。 否则，应通过默认修饰符（如`.cmp-teaser--default`）应用这些样式，而后者又需要添加到[组件的样式配置的“默认CSS类](#component-styles-configuration)”字段中，否则，这些样式规则必须在每个变体中覆盖。

甚至可以将“named”样式指定为默认样式，例如，下面定义的Hero样式`(.cmp-teaser--hero)`，但更清楚的是针对`.cmp-teaser`或`.cmp-teaser--default` CSS类实现实现实现默认样式。

>[!NOTE]
>
>请注意，默认布局样式没有显示样式名称，但作者将能够在AEM样式系统选择工具中选择显示选项。
>
>这违反了最佳做法：
>
>**仅显示有效的样式组合**
>
>如果作者选择&#x200B;**Green**&#x200B;的“显示”样式，则不会发生任何情况。
>
>在此用例中，我们将承认此违规，因为所有其他布局样式都必须使用品牌颜色可着色。
>
>在下面的&#x200B;**促销（右对齐）**&#x200B;部分，我们将了解如何防止不需要的样式组合。

![默认样式](assets/default.png)

* **布局样式**
   * 默认
* **显示样式**
   * 无
* **有效的CSS类**: `.cmp-teaser--promo` 或  `.cmp-teaser--default`

### 促销样式{#promo-style}

**促销布局样式**&#x200B;用于提升网站上的高价值内容，并水平排列以占用网页上的一段空间，且必须按品牌颜色设置样式，默认的促销布局样式为使用黑色文本。

为此，在Teaser组件的AEM样式系统中配置了&#x200B;**Promo**&#x200B;的&#x200B;**布局样式**&#x200B;和&#x200B;****&#x200B;绿色&#x200B;**和**&#x200B;黄色&#x200B;**的显示样式**。

#### 促销默认

![promo默认值](assets/promo-default.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--promo`
* **显示样式**
   * 无
* **有效的CSS类**:  `.cmp-teaser--promo`

#### 促销主

![promo primary](assets/promo-primary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--promo`
* **显示样式**
   * 样式名称：**绿色**
   * CSS 类: `cmp-teaser--primary-color`
* **有效的CSS类**:  `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo Secondary

![Promo Secondary](assets/promo-secondary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--promo`
* **显示样式**
   * 样式名称：**黄色**
   * CSS 类: `cmp-teaser--secondary-color`
* **有效的CSS类**:  `cmp-teaser--promo.cmp-teaser--secondary-color`

### 促销右对齐样式{#promo-r-align}

**右对齐促销**&#x200B;布局样式是促销样式的变体，该样式会翻转图像和文本的位置（图像在右侧，文本在左侧）。

右对齐的核心是显示样式，它可以作为与促销布局样式一起选择的显示样式输入AEM样式系统。 这违反了以下最佳做法：

**仅显示有效的样式组合**

..已在[默认样式](#default-style)中违反。

由于右对齐仅影响促销活动布局样式，而不影响其他2种布局样式：默认和hero，我们可以创建一个新的布局样式Promo（右对齐），它包含CSS类，该类右对齐Promo布局样式内容：`cmp -teaser--alternate`。

将多个样式组合到单个Style项中还有助于减少可用样式和样式排列的数量，这最好能使其最小化。

请注意，CSS类的名称`cmp-teaser--alternate`不必与“right aligned”的对作者友好的命名法匹配。

#### 促销右对齐默认

![促销活动右对齐](assets/promo-alternate-default.png)

* **布局样式**
   * 样式名称：**促销（右对齐）**
   * CSS 类: `cmp-teaser--promo cmp-teaser--alternate`
* **显示样式**
   * 无
* **有效的CSS类**:  `.cmp-teaser--promo.cmp-teaser--alternate`

#### 促销右对齐主

![促销右对齐主](assets/promo-alternate-primary.png)

* **布局样式**
   * 样式名称：**促销（右对齐）**
   * CSS 类: `cmp-teaser--promo cmp-teaser--alternate`
* **显示样式**
   * 样式名称：**绿色**
   * CSS 类: `cmp-teaser--primary-color`
* **有效的CSS类**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### 促销右对齐辅助

![促销右对齐辅助](assets/promo-alternate-secondary.png)

* **布局样式**
   * 样式名称：**促销（右对齐）**
   * CSS 类: `cmp-teaser--promo cmp-teaser--alternate`
* **显示样式**
   * 样式名称：**黄色**
   * CSS 类: `cmp-teaser--secondary-color`
* **有效的CSS类**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Hero样式{#hero-style}

“主角”布局样式将组件的图像显示为背景，并且标题和链接已覆盖。 Hero布局样式（如Promo布局样式）必须可用品牌颜色着色。

要使用品牌颜色为Hero布局样式着色，可以利用与用于Promo布局样式相同的显示样式。

对于每个组件，样式名称将映射到一组CSS类，这意味着为Promo布局样式的背景设置颜色的CSS类名称必须为Hero布局样式的文本和链接设置颜色。

这可以通过范围化CSS规则来实现，但这确实需要CSS开发人员了解如何在AEM上实施这些排列。

用于用主（绿色）颜色为&#x200B;**提升**&#x200B;布局样式的背景着色的CSS:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

用于用主（绿色）颜色为&#x200B;**Hero**&#x200B;布局样式的文本着色的CSS:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### 主角默认

![英雄风格](assets/hero.png)

* **布局样式**
   * 样式名称：**Hero**
   * CSS 类: `cmp-teaser--hero`
* **显示样式**
   * 无
* **有效的CSS类**:  `.cmp-teaser--hero`

#### 主角

![主角](assets/hero-primary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--hero`
* **显示样式**
   * 样式名称：**绿色**
   * CSS 类: `cmp-teaser--primary-color`
* **有效的CSS类**:  `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Secondary

![Hero Secondary](assets/hero-secondary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--hero`
* **显示样式**
   * 样式名称：**黄色**
   * CSS 类: `cmp-teaser--secondary-color`
* **有效的CSS类**:  `cmp-teaser--hero.cmp-teaser--secondary-color`

## 其他资源 {#additional-resources}

* [样式系统文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [创建AEM Client库](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（块元素修饰符）文档网站](https://getbem.com/)
* [LESS文档网站](https://lesscss.org/)
* [jQuery网站](https://jquery.com/)
