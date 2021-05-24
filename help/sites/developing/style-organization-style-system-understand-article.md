---
title: 了解使用AEM Sites的样式系统最佳实践
description: 详细的文章，介绍在使用Adobe Experience Manager Sites实施样式系统时的最佳实践。
feature: 样式系统
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: 开发
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1541'
ht-degree: 2%

---


# 了解样式系统最佳实践{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>请在[了解如何为样式系统编码](style-system-technical-video-understand.md)中查看相关内容，以确保了解AEM样式系统使用的类似BEM的约定。

AEM样式系统主要实施两种风格或样式：

* **布局样式**
* **显示样式**

**布局** 样式会影响组件的许多元素，以创建组件的良好定义和可识别呈现版本（设计和布局），通常与特定的可重用品牌概念保持一致。例如，Teaser组件可以采用传统的基于卡的布局、水平促销样式或作为在图像上叠加文本的Hero布局来呈现。

**显示** 样式用于影响布局样式的次要变体，但它们不会更改布局样式的基本性质或意图。例如，“主页”布局样式可能具有将颜色方案从主品牌颜色方案更改为次品牌颜色方案的“显示”样式。

## 样式组织最佳实践{#style-organization-best-practices}

定义AEM作者可用的样式名称时，最好执行以下操作：

* 使用作者所理解的词汇命名样式
* 最大限度地减少样式选项的数量
* 仅显示品牌标准允许的样式选项和组合
* 仅显示有效的样式组合
   * 如果暴露无效组合，请确保它们至少不会产生不良影响

随着AEM作者可用的可能样式组合的数量增加，必须根据品牌标准进行QA和验证的排列组合也会越多。 太多的选项也会令作者感到困惑，因为不清楚需要哪个选项或组合才能产生所需的效果。

### 样式名称与CSS类{#style-names-vs-css-classes}

样式名称或呈现给AEM作者的选项，以及实施CSS类名称在AEM中是相互分离的。

这允许样式选项以词汇清晰的方式进行标记，并且AEM作者可以理解该选项，但允许CSS开发人员以未来的语义方式命名CSS类。 例如：

组件必须具有使用品牌&#x200B;**primary**&#x200B;和&#x200B;**secondary**&#x200B;颜色进行着色的选项，但是，AEM作者知道颜色为&#x200B;**green**&#x200B;和&#x200B;**yell**，而不是主要和次要的设计语言。

AEM样式系统可以使用对作者友好的标签&#x200B;**Green**&#x200B;和&#x200B;**Yellow**&#x200B;公开这些着色显示样式，同时允许CSS开发人员使用`.cmp-component--primary-color`和`.cmp-component--secondary-color`的语义命名来定义CSS中的实际样式实施。

将&#x200B;**Green**&#x200B;的样式名称映射到`.cmp-component--primary-color`，将&#x200B;**Yellow**&#x200B;映射到`.cmp-component--secondary-color`。

如果公司的品牌颜色在将来发生更改，则需要更改的所有内容都是`.cmp-component--primary-color`和`.cmp-component--secondary-color`的单个实施以及样式名称。

## Teaser组件作为示例用例{#the-teaser-component-as-an-example-use-case}

以下示例用例为Teaser组件设置样式，使其具有多个不同的布局和显示样式。

这将探索样式名称（向作者公开）的组织方式以及备用CSS类的组织方式。

### Teaser组件样式配置{#component-styles-configuration}

下图显示了用于用例中讨论的变体的Teaser组件的[!UICONTROL Styles]配置。

[!UICONTROL 样式组]名称、布局和显示（按偶然情况）与显示样式和布局样式的一般概念匹配，后者用于对本文中的样式类型进行概念性分类。

[!UICONTROL 样式组]名称和[!UICONTROL 样式组]的编号应根据组件用例和特定于项目的组件样式约定进行定制。

例如，**Display**&#x200B;样式组名称可能已命名为&#x200B;**Colors**。

![显示样式组](assets/style-config.png)

### 样式选择菜单{#style-selection-menu}

下图显示了[!UICONTROL Style]菜单作者与之交互以选择组件的相应样式。 请注意，[!UICONTROL 样式图形]名称以及样式名称都会显示给作者。

![样式下拉菜单](assets/style-menu.png)

### 默认样式{#default-style}

默认样式通常是组件最常用的样式，而Teaser添加到页面时的默认未设置样式视图。

根据默认样式的通用性，CSS可以直接应用于`.cmp-teaser`（不含任何修饰符）或`.cmp-teaser--default`。

如果默认样式规则比不适用于所有变体更频繁，则最好使用`.cmp-teaser`作为默认样式的CSS类，因为所有变体都应隐式继承它们，并假定遵循BEM类似惯例。 如果没有，则应通过默认修饰符（例如`.cmp-teaser--default`）来应用这些类型，而默认修饰符又需要添加到[组件样式配置的默认CSS类](#component-styles-configuration)字段中，否则，必须在每个变体中覆盖这些样式规则。

甚至可以将“name”样式指定为默认样式，例如下面定义的主页样式`(.cmp-teaser--hero)`，但更清楚的是，要针对`.cmp-teaser`或`.cmp-teaser--default` CSS类实施实施默认样式。

>[!NOTE]
>
>请注意，默认布局样式没有显示样式名称，但作者将能够在AEM样式系统选择工具中选择显示选项。
>
>这违反了最佳做法：
>
>**仅显示有效的样式组合**
>
>如果作者选择&#x200B;**Green**&#x200B;的显示样式，则不会发生任何情况。
>
>在此用例中，我们将承认此违规，因为所有其他布局样式都必须使用品牌颜色可着色。
>
>在下面的&#x200B;**促销（右对齐）**&#x200B;部分中，我们将了解如何防止不需要的样式组合。

![默认样式](assets/default.png)

* **布局样式**
   * 默认
* **显示样式**
   * 无
* **有效的CSS类**: `.cmp-teaser--promo` 或  `.cmp-teaser--default`

### 促销样式{#promo-style}

**促销活动布局样式**&#x200B;用于促销网站上的高价值内容，并且水平布局可占用网页上的一段空间，且必须按品牌颜色设置样式，默认促销活动布局样式使用黑色文本。

为此，在Teaser组件的AEM样式系统中配置了&#x200B;**布局样式**&#x200B;的&#x200B;**Promo**&#x200B;和&#x200B;**显示样式**&#x200B;的&#x200B;**Green**&#x200B;和&#x200B;**Yellow**。

#### 促销活动默认

![促销活动默认](assets/promo-default.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--promo`
* **显示样式**
   * 无
* **有效的CSS类**:  `.cmp-teaser--promo`

#### 促销主要

![促销主要](assets/promo-primary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--promo`
* **显示样式**
   * 样式名称：**绿色**
   * CSS 类: `cmp-teaser--primary-color`
* **有效的CSS类**:  `cmp-teaser--promo.cmp-teaser--primary-color`

#### 促销次级

![促销次级](assets/promo-secondary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--promo`
* **显示样式**
   * 样式名称：**黄色**
   * CSS 类: `cmp-teaser--secondary-color`
* **有效的CSS类**:  `cmp-teaser--promo.cmp-teaser--secondary-color`

### 促销右对齐样式{#promo-r-align}

**促销活动右对齐**&#x200B;布局样式是促销活动样式的变体，该样式会反转图像和文本的位置（图像位于右侧，文本位于左侧）。

右对齐的核心是显示样式，可以将其作为与促销活动布局样式一起选择的显示样式输入AEM样式系统中。 这违反了以下最佳实践：

**仅显示有效的样式组合**

..已在[默认样式](#default-style)中违反。

由于右对齐仅影响促销活动布局样式，而不会影响其他2种布局样式：默认和主页，我们可以创建新的布局样式促销（右对齐），它包含CSS类，该类将促销布局样式内容右对齐：`cmp -teaser--alternate`。

将多个样式组合到单个样式条目中，还有助于减少可用样式和样式排列的数量，这最好能够最大限度地减少这些数量。

请注意，CSS类的名称`cmp-teaser--alternate`不必与“right aligned”的作者友好命名法匹配。

#### 促销右对齐默认值

![促销活动右对齐](assets/promo-alternate-default.png)

* **布局样式**
   * 样式名称：**促销（右对齐）**
   * CSS 类: `cmp-teaser--promo cmp-teaser--alternate`
* **显示样式**
   * 无
* **有效的CSS类**:  `.cmp-teaser--promo.cmp-teaser--alternate`

#### 促销右对齐的主促销活动

![促销右对齐的主促销活动](assets/promo-alternate-primary.png)

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

### 主页样式{#hero-style}

主页布局样式将组件的图像显示为背景，并且标题和链接被覆盖。 Hero布局样式（与Promo布局样式类似）必须可以使用品牌颜色着色。

要使用品牌颜色为Hero布局样式设置颜色，可以使用与促销活动布局样式相同的显示样式。

对于每个组件，样式名称会映射到一组CSS类，这意味着为促销活动布局样式背景设置颜色的CSS类名称必须为Hero布局样式的文本和链接设置颜色。

通过范围化CSS规则可以轻松实现这一点，但是，这确实要求CSS开发人员了解如何在AEM上实施这些排列。

用于将&#x200B;**Promote**&#x200B;布局样式的背景着色为主（绿色）颜色的CSS:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

用于将&#x200B;**Hero**&#x200B;布局样式的文本着色为主（绿色）颜色的CSS:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### 主页默认

![英雄风格](assets/hero.png)

* **布局样式**
   * 样式名称：**Hero**
   * CSS 类: `cmp-teaser--hero`
* **显示样式**
   * 无
* **有效的CSS类**:  `.cmp-teaser--hero`

#### 主页

![主页](assets/hero-primary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--hero`
* **显示样式**
   * 样式名称：**绿色**
   * CSS 类: `cmp-teaser--primary-color`
* **有效的CSS类**:  `cmp-teaser--hero.cmp-teaser--primary-color`

#### 主页辅助

![主页辅助](assets/hero-secondary.png)

* **布局样式**
   * 样式名称：**促销**
   * CSS 类: `cmp-teaser--hero`
* **显示样式**
   * 样式名称：**黄色**
   * CSS 类: `cmp-teaser--secondary-color`
* **有效的CSS类**:  `cmp-teaser--hero.cmp-teaser--secondary-color`

## 其他资源 {#additional-resources}

* [样式系统文档](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [创建AEM客户端库](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM（块元素修饰符）文档网站](https://getbem.com/)
* [LESS文档网站](https://lesscss.org/)
* [jQuery网站](https://jquery.com/)
