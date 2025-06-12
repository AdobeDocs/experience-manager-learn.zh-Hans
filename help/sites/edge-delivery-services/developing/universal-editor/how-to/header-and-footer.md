---
title: 页眉和页脚
description: 了解如何在 Edge Delivery Services 和通用编辑器中开发页眉和页脚。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
jira: KT-17470
duration: 300
exl-id: 70ed4362-d4f1-4223-8528-314b2bf06c7c
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1207'
ht-degree: 100%

---

# 开发页眉和页脚

![页眉和页脚](./assets/header-and-footer/hero.png){align="center"}

在 Edge Delivery Services (EDS) 中，页眉和页脚具有独特的作用，因为它们直接绑定到 HTML 的 `<header>` 和 `<footer>` 元素上。与常规页面内容不同，它们是单独管理的，可以独立更新，无需清除整个页面缓存。虽然它们的实施以代码块的形式存在于代码项目中，位于 `blocks/header` 和 `blocks/footer` 下，但作者可以通过可包含任何代码块组合的专用 AEM 页面来编辑其内容。

## 页眉块

![页眉块](./assets/header-and-footer/header-local-development-preview.png){align="center"}

页眉是一个特殊的块，它与 Edge Delivery Services HTML `<header>` 元素绑定。
`<header>` 元素以空状态传递，并通过 XHR (AJAX) 填充到单独的 AEM 页面。
这使得页眉可以独立于页面内容进行管理，并且无需清除所有页面的完整缓存即可进行更新。

页眉块负责请求包含页眉内容的 AEM 页面片段，并将其呈现在 `<header>` 元素中。

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="下面是代码示例的文件名。"}

```javascript
import { getMetadata } from '../../scripts/aem.js';
import { loadFragment } from '../fragment/fragment.js';

...

export default async function decorate(block) {
  // load nav as fragment

  // Get the path to the AEM page fragment that defines the header content from the <meta name="nav"> tag. This is set via the site's Metadata file.
  const navMeta = getMetadata('nav');

  // If the navMeta is not defined, use the default path `/nav`.
  const navPath = navMeta ? new URL(navMeta, window.location).pathname : '/nav';

  // Make an XHR (AJAX) call to request the AEM page fragment and serialize it to a HTML DOM tree.
  const fragment = await loadFragment(navPath);
  
  // Add the content from the fragment HTML to the block and decorate it as needed
  ...
}
```

`loadFragment()` 函数向 `${navPath}.plain.html` 发起一个 XHR (AJAX) 请求，后者返回存在于页面 `<main>` 标记中的 AEM 页面 HTML 的 EDS HTML 演绎版，并会处理其可能包含的任何块的内容，然后返回更新后的 DOM 树。

## 创作页眉页面

在开发页眉块之前，先在通用编辑器中创作其内容，以便有内容可供开发。

页眉内容位于名为 `nav` 的专用 AEM 页面中。

![默认页眉页面](./assets/header-and-footer/header-page.png){align="center"}

要创作页眉：

1. 在通用编辑器中打开 `nav` 页面
1. 将默认按钮替换为包含 WKND 标志的&#x200B;**图像块**
1. 通过以下方式更新&#x200B;**文本块**&#x200B;中的导航菜单：
   - 添加您想要的导航链接
   - 在需要的地方创建子导航项
   - 暂时将所有链接都设置为指向主页 (`/`)

![在通用编辑器中创作页眉块](./assets/header-and-footer/header-author.png){align="center"}

### 发布到预览环境

在页眉页面更新后，[将该页面发布到预览环境](../6-author-block.md)。

由于页眉内容存在于其独立的页面（即 `nav` 页面）上，因此必须单独发布该页面，页眉的更改才会生效。发布使用该页眉的其他页面不会更新 Edge Delivery Services 上的页眉内容。

## 区块 HTML

要开始开发区块，请首先查看 Edge Delivery Services 预览版呈现的 DOM 结构。DOM 通过 JavaScript 进行增强，并使用 CSS 进行样式设计，为构建和定制区块提供了基础。

由于页眉是以片段形式加载的，因此我们需要在将其注入 DOM 并通过 `loadFragment()` 进行装饰后，检查 XHR 请求返回的 HTML。这可以通过在浏览器开发者工具中检查 DOM 来实现。


>[!BEGINTABS]

>[!TAB 要装饰的 DOM]

以下是在使用提供的 `header.js` 加载并注入 DOM 后，页眉页面的 HTML 代码：

```html
<header class="header-wrapper">
  <div class="header block" data-block-name="header" data-block-status="loaded">
    <div class="nav-wrapper">
      <nav id="nav" aria-expanded="true">
        <div class="nav-hamburger">
          <button type="button" aria-controls="nav" aria-label="Close navigation">
            <span class="nav-hamburger-icon"></span>
          </button>
        </div>
        <div class="section nav-brand" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <p class="">
              <a href="#" title="Button" class="">Button</a>
            </p>
          </div>
        </div>
        <div class="section nav-sections" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <ul>
              <li aria-expanded="false">Examples</li>
              <li aria-expanded="false">Getting Started</li>
              <li aria-expanded="false">Documentation</li>
            </ul>
          </div>
        </div>
        <div class="section nav-tools" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <p>
              <span class="icon icon-search">
                <img data-icon-name="search" src="/icons/search.svg" alt="" loading="lazy">
              </span>
            </p>
          </div>
        </div>
      </nav>
    </div>
  </div>
</header>
```

>[!TAB 如何找到 DOM]

在 Web 浏览器的开发者工具中查找并检查页面的 `<header>` 元素。

![页眉 DOM](./assets/header-and-footer/header-dom.png){align="center"}

>[!ENDTABS]


## 区块 JavaScript

[AEM Boilerplate XWalk 项目模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)中的 `/blocks/header/header.js` 文件提供了用于导航的 JavaScript 脚本，功能包括下拉菜单和响应式移动视图。

尽管 `header.js` 脚本通常会根据网站设计进行大量定制，但务必要保留 `decorate()` 中的前几行代码，它们负责获取并处理页眉页面片段。

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="下面是代码示例的文件名。"}

```javascript
export default async function decorate(block) {
  // load nav as fragment
  const navMeta = getMetadata('nav');
  const navPath = navMeta ? new URL(navMeta, window.location).pathname : '/nav';
  const fragment = await loadFragment(navPath);
  ...
```

剩余的代码可以根据项目需求进行修改。

根据页眉的需求，可以对样板代码进行调整或移除。在本教程中，我们将使用提供的代码，并通过在首个创作的图像周围添加超链接，将其链接到网站主页，从而对其进行增强。

模板中的代码会处理页眉页面片段，前提是假设该片段由以下顺序排列的三个部分组成：

1. **品牌部分** – 包含标志，并使用 `.nav-brand` 类进行样式设置。
2. **导航部分** – 定义网站的主菜单，并使用 `.nav-sections` 类进行样式设置。
3. **工具部分** – 包含搜索、登录/登出、轮廓等元素，并使用 `.nav-tools` 类进行样式设置。

要将徽标图像超链接到主页，我们按如下方式更新该块的 JavaScript 代码：

>[!BEGINTABS]

>[!TAB 更新后的 JavaScript]

下面展示了将徽标图像用链接包裹，指向网站主页 (`/`) 的更新代码：

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="下面是代码示例的文件名。"}

```javascript
export default async function decorate(block) {

  ...
  const navBrand = nav.querySelector('.nav-brand');
  
  // WKND: Turn the picture (image) into a linked site logo
  const logo = navBrand.querySelector('picture');
  
  if (logo) {
    // Replace the first section's contents with the authored image wrapped with a link to '/' 
    navBrand.innerHTML = `<a href="/" aria-label="Home" title="Home" class="home">${logo.outerHTML}</a>`;
    // Make sure the logo is not lazy loaded as it's above the fold and can affect page load speed
    navBrand.querySelector('img').settAttribute('loading', 'eager');
  }

  const navSections = nav.querySelector('.nav-sections');
  if (navSections) {
    // WKND: Remove Edge Delivery Services button containers and buttons from the nav sections links
    navSections.querySelectorAll('.button-container, .button').forEach((button) => {
      button.classList = '';
    });

    ...
  }
  ...
}
```

>[!TAB 原始 JavaScript]

以下是模板生成的原始 `header.js`：

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="下面是代码示例的文件名。"}

```javascript
export default async function decorate(block) {
  ...
  const navBrand = nav.querySelector('.nav-brand');
  const brandLink = navBrand.querySelector('.button');
  if (brandLink) {
    brandLink.className = '';
    brandLink.closest('.button-container').className = '';
  }

  const navSections = nav.querySelector('.nav-sections');
  if (navSections) {
    navSections.querySelectorAll(':scope .default-content-wrapper > ul > li').forEach((navSection) => {
      if (navSection.querySelector('ul')) navSection.classList.add('nav-drop');
      navSection.addEventListener('click', () => {
        if (isDesktop.matches) {
          const expanded = navSection.getAttribute('aria-expanded') === 'true';
          toggleAllNavSections(navSections);
          navSection.setAttribute('aria-expanded', expanded ? 'false' : 'true');
        }
      });
    });
  }
  ...
}
```

>[!ENDTABS]


## 区块 CSS

更新 `/blocks/header/header.css`，使其符合 WKND 品牌的风格。

我们将在 `header.css` 的底部添加自定义 CSS，以便更直观地展示和理解本教程中的更改。虽然这些样式可以直接整合到模板的 CSS 规则中，但将它们单独保留有助于清晰展示所做的修改内容。

由于我们是在原有规则之后添加新的规则，因此会用 `header .header.block nav` CSS 选择器包裹它们，以确保新规则优先于模板规则生效。

[!BADGE /blocks/header/header.css]{type=Neutral tooltip="下面是代码示例的文件名。"}

```css
/* /blocks/header/header.css */

... Existing CSS generated by the template ...

/* Add the following CSS to the end of the header.css */

/** 
* WKND customizations to the header 
* 
* These overrides can be incorporated into the provided CSS,
* however they are included discretely in thus tutorial for clarity and ease of addition.
* 
* Because these are added discretely
* - They are added to the bottom to override previous styles.
* - They are wrapped in a header .header.block nav selector to ensure they have more specificity than the provided CSS.
* 
**/

header .header.block nav {
  /* Set the height of the logo image.
     Chrome natively sets the width based on the images aspect ratio */
  .nav-brand img {
    height: calc(var(--nav-height) * .75);
    width: auto;
    margin-top: 5px;
  }
  
  .nav-sections {
    /* Update menu items display properties */
    a {
      text-transform: uppercase;
      background-color: transparent;
      color: var(--text-color);
      font-weight: 500;
      font-size: var(--body-font-size-s);
    
      &:hover {
        background-color: auto;
      }
    }

    /* Adjust some spacing and positioning of the dropdown nav */
    .nav-drop {
      &::after {
        transform: translateY(-50%) rotate(135deg);
      }
      
      &[aria-expanded='true']::after {
        transform: translateY(50%) rotate(-45deg);
      }

      & > ul {
        top: 2rem;
        left: -1rem;      
       }
    }
  }
```

## 开发预览

在开发 CSS 和 JavaScript 的过程中，AEM CLI 的本地开发环境会实时热加载更改，方便快速直观地查看代码对区块的影响。将鼠标悬停在 CTA 上，确认 Teaser 图像是否有放大和缩小的效果。

![使用 CSS 和 JS 进行页眉的本地开发预览](./assets/header-and-footer/header-local-development-preview.png){align="center"}

## 对代码进行规范检查

请确保[经常](../3-local-development-environment.md#linting)对代码更改进行规范检查，以保持代码的整洁和一致性。定期进行代码规范检查有助于及早发现问题，减少整体开发时间。请记住，只有在所有代码规范问题解决后，才能将开发工作合并到 `main` 分支！

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## 在通用编辑器中预览

要在 AEM 的通用编辑器中查看更改，请将更改添加、提交并推送到通用编辑器使用的 Git 存储库分支。这样做可以确保区块的实施不会影响创作体验。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for Header block"
# JSON files are compiled automatically and added to the commit via a Husky pre-commit hook
$ git push origin header-and-footer
```

现在，在使用 `?ref=header-and-footer` 查询参数时，通用编辑器中即可看到这些更改。

![通用编辑器中的页眉](./assets/header-and-footer/header-universal-editor-preview.png){align="center"}

## 页脚

与页眉类似，页脚内容也是在专用的 AEM 页面上创作的——在本例中为页脚页面 (`footer`)。页脚遵循相同的模式，即以片段形式加载，并通过 CSS 和 JavaScript 进行装饰。

>[!BEGINTABS]

>[!TAB 页脚]

页脚应采用三栏布局，其中包含以下内容：

- 左栏包含一个促销内容（图像和文本）
- 中间栏包含导航链接
- 右栏包含社交媒体链接
- 底部有一行跨越三栏，其中显示版权信息

![页脚预览](./assets/header-and-footer/footer-preview.png){align="center"}

>[!TAB 页脚内容]

在页脚页面中使用“列”区块来实现三栏布局效果。

| 列 1 | 列 2 | 列 3 |
| ---------|----------------|---------------|
| 图像 | 标题 3 | 标题 3 |
| 文本 | 链接列表 | 链接列表 |

![页眉 DOM](./assets/header-and-footer/footer-author.png){align="center"}

>[!TAB 页脚代码]

以下 CSS 为页脚区块设置了三栏布局、一致的间距和排版规则。页脚的实施仅使用了模板提供的 JavaScript。

[!BADGE /blocks/footer/footer.css]{type=Neutral tooltip="下面是代码示例的文件名。"}

```css
/* /blocks/footer/footer.css */

footer {
  background-color: var(--light-color);

  .block.footer {
    border-top: solid 1px var(--dark-color);
    font-size: var(--body-font-size-s);

    a { 
      all: unset;
      
      &:hover {
        text-decoration: underline;
        cursor: pointer;
      }
    }

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border: solid 1px white;
    }

    p {
      margin: 0;
    }

    ul {
      list-style: none;
      padding: 0;
      margin: 0;

      li {
        padding-left: .5rem;
      }
    }

    & > div {
      margin: auto;
      max-width: 1200px;
    }

    .columns > div {
      gap: 5rem;
      align-items: flex-start;

      & > div:first-child {
        flex: 2;
      }
    }

    .default-content-wrapper {
      padding-top: 2rem;
      margin-top: 2rem;
      font-style: italic;
      text-align: right;
    }
  }
}

@media (width >= 900px) {
  footer .block.footer > div {
    padding: 40px 32px 24px;
  }
}
```


>[!ENDTABS]

## 恭喜！

您现在已经了解了如何在 Edge Delivery Services 和通用编辑器中管理和开发页眉与页脚。您已经学会了它们是如何：

- 在独立于主内容的专用 AEM 页面上创作
- 以片段形式异步加载，以实现独立更新
- 通过 JavaScript 和 CSS 进行装饰，以实现响应式导航体验
- 与通用编辑器无缝集成，以便于内容管理

这种模式为实施全站导航组件提供了灵活且易维护的方案。

如需了解更多最佳实践和高级技巧，请查阅[通用编辑器文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)。
