---
title: 添加网站品牌
description: 为Edge Delivery Services站点定义全局CSS、CSS变量和Web字体。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: ceb82c48af10191cece72fe5f53dd79287f805d0
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# 添加网站品牌

首先通过更新全局样式、定义CSS变量和添加Web字体来设置整体品牌。 这些基本元素可确保网站保持一致性和可维护性，并且应在整个站点中始终如一地应用。

## 创建GitHub问题

要保持一切井然有序，请使用GitHub跟踪工作。 首先，为以下工作主体创建一个GitHub问题：

1. 转到GitHub存储库（有关详细信息，请参阅[创建代码项目](./1-new-code-project.md)章节）。
2. 单击&#x200B;**问题**&#x200B;选项卡，然后单击&#x200B;**新问题**。
3. 为要完成的工作编写&#x200B;**标题**&#x200B;和&#x200B;**描述**。
4. 单击&#x200B;**提交新问题**。

稍后在[创建拉取请求](#merge-code-changes)时，将使用GitHub问题。

![GitHub创建新问题](./assets/4-website-branding/github-issues.png)

## 创建工作分支

要维护组织并确保代码质量，请为每个工作主体创建新分支。 此做法可防止新代码对性能产生负面影响，并确保所做的更改在完成之前不会生效。

在本章中（侧重于网站的基本样式），创建一个名为`wknd-styles`的分支。

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## 全局CSS

Edge Delivery Services使用位于`styles/styles.css`的全局CSS文件为整个网站设置通用样式。 `styles.css`控制颜色、字体和间距等方面，确保整个网站的所有内容看起来一致。

全局CSS应与较低级别的结构（如块）无关，应侧重于站点的整体外观和感觉，并且应共享视觉处理。

请记住，需要时可覆盖全局CSS样式。

### CSS变量

[CSS变量](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)是存储设计设置（如颜色、字体和大小）的绝佳方式。 通过使用变量，您可以在一个位置更改这些元素，并在整个网站中对其进行更新。

要开始自定义CSS变量，请执行以下步骤：

1. 在代码编辑器中打开`styles/styles.css`文件。
2. 查找存储全局CSS变量的`:root`声明。
3. 修改颜色和字体变量以匹配WKND品牌。

示例如下：


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

浏览`:root`部分中的其他变量并查看默认设置。

在开发网站时，如果您发现自己重复了相同的CSS值，请考虑创建新变量以使样式更易于管理。 其他可从CSS变量中受益的CSS属性示例包括： `border-radius`、`padding`、`margin`和`box-shadow`。

### 裸元素

裸元素的样式直接通过其元素名称来设置，而不使用CSS类。 例如，样式不是为`.page-heading` CSS类设置样式，而是使用`h1 { ... }`应用于`h1`元素。

在`styles/styles.css`文件中，一组基本样式应用于纯HTML元素。 Edge Delivery Services网站使用裸元素进行优先级排序，因为它们与Edge Delivery服务的本机语义HTML一致。

为了与WKND品牌化保持一致，让我们在`styles.css`中设置一些裸元素：

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

这些样式可确保`h2`元素（除非被覆盖）的样式始终与WKND品牌保持一致，从而帮助创建清晰的可视层次结构。 每个`h2`下面的部分黄色下划线为标题添加了独特触点。

### 推断的元素

在Edge Delivery Services中，项目的`scripts.js`和`aem.js`代码会根据HTML中特定纯HTML元素的上下文自动增强这些元素。

例如，根据此上下文，在锚点(`<a>`)元素自己的行上创作（而不是与周围的文本内联）被推断为按钮。 这些锚点将自动用包含CSS类`button-container`的容器`div`包装，并且锚点元素已添加`button` CSS类。

例如，当链接是在其自身的行上创作时，Edge Delivery ServicesJavaScript会将其DOM更新为以下内容：

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

这些按钮可以自定义以匹配WKND品牌 — 这指示按钮显示为带有黑色文本的黄色矩形。

以下是如何设置`styles.css`中“推断的按钮”样式的示例：

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

此CSS定义基本按钮样式并包含特定于WKND的处理，例如大写文本、黄色背景和黑色文本。 `background-color`和`color`属性使用允许按钮样式与品牌颜色保持一致的CSS变量。 此方法可确保整个站点的按钮样式保持一致，同时保持灵活性。

## Web字体

Edge Delivery Services项目可优化Web字体的使用，以保持高性能并最大程度地降低对Lighthouse得分的影响。 此方法可确保快速渲染，而不会损害站点的视觉身份。 下面是如何高效地实施Web字体以获得最佳性能的。

### 字体

在`styles/fonts.css`文件中使用CSS `@font-face`声明添加自定义Web字体。 将`@font-faces`添加到`fonts.css`可以确保在最佳时间加载Web字体，从而帮助维护Lighthouse得分。

1. 打开`styles/fonts.css`。
2. 添加以下`@font-face`声明以包含WKND品牌字体： `Asar`和`Source Sans Pro`。

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

本教程中使用的字体源自Google Fonts，但Web字体可以从任何字体提供程序获取，包括[Adobe Fonts](https://fonts.adobe.com/)。

+++使用本地Web字体文件

或者，将Web字体文件复制到`/fonts`文件夹中的项目中，并在`@font-face`声明中引用。

本教程使用远程、托管的Web字体，以便更轻松地遵循。

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

最后，更新`styles/styles.css` CSS变量以使用新字体：

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback`和`roboto-condensed-fallback`是[回退字体](#fallback-fonts)部分中更新的回退字体，以便调整以支持自定义`Asar`和`Source Sans Pro`网页字体。

### 回退字体

Web字体由于其大小而经常影响性能，可能会增加累积布局偏移(CLS)分数并降低总体Lighthouse分数。 为确保在Web字体加载时即时显示文本，Edge Delivery Services项目使用浏览器本机回退字体。 这种方法有助于在应用所需字体时保持流畅的用户体验。

要选择最佳回退字体，请使用Adobe的[Helix Font Fallback Chrome扩展](https://www.aem.live/developer/font-fallback)，该扩展可在加载自定义字体之前确定与浏览器使用的字体密切匹配的字体。 应将生成的回退字体声明添加到`styles/styles.css`文件中，以提高性能并确保用户获得无缝体验。

要使用[Helix Font Fallback Chrome扩展](https://www.aem.live/developer/font-fallback)，请确保该网页在Edge Delivery Services网站上使用的相同变体中应用了Web字体。 本教程演示了[wknd.site](http://wknd.site/us/en.html)上的扩展。 在开发网站时，请将扩展应用于正在处理的网站，而不是[wknd.site](http://wknd.site/us/en.html)。

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

将回退字体系列名称添加到`styles/styles.css`中的字体CSS变量中“真正的”字体系列名称之后。

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## 开发预览

添加CSS时，AEM CLI的本地开发环境会自动重新加载更改，以便快速轻松地查看CSS对块的影响。

![WKND品牌CSS的开发预览](./assets/4-website-branding/preview.png)


## 下载最终CSS文件

您可以通过以下链接下载更新的CSS文件：

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## 链接CSS文件

请确保[频繁lint](./3-local-development-environment.md#linting)您的代码更改以确保它们干净一致。 定期筛选有助于及早发现问题并减少总体开发时间。 请记住，在解决所有链接问题之前，您无法将工作合并到主分支！

```bash
$ npm run lint:css
```

## 合并代码更改

将更改合并到GitHub上的`main`分支中，以根据这些更新构建未来的工作。

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

将更改推送到`wknd-styles`分支后，请在GitHub上创建拉取请求以将其合并到`main`分支中。

1. 从[创建新项目](./1-new-code-project.md)章节导航到GitHub存储库。
2. 单击&#x200B;**拉取请求**&#x200B;选项卡，然后选择&#x200B;**新建拉取请求**。
3. 将`wknd-styles`设置为源分支，将`main`设置为目标分支。
4. 查看更改并单击&#x200B;**创建拉取请求**。
5. 在拉取请求详细信息中，**添加以下**：

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1`引用了之前创建的GitHub问题。
   * 测试URL告知AEM代码同步使用哪些分支进行验证和比较。 “之后”URL使用工作分支`wknd-styles`来检查代码更改对网站性能有何影响。

6. 单击&#x200B;**创建拉取请求**。
7. 等待[AEM代码同步GitHub应用](./1-new-code-project.md)到&#x200B;**完成质量检查**。 如果失败，请解决错误并重新运行检查。
8. 检查通过后，**将拉取请求**&#x200B;合并到`main`中。

将更改合并到`main`后，不会将它们视为部署到生产环境，并且新开发可以根据这些更新继续。
