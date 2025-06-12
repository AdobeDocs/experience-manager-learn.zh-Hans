---
title: 使用 CSS 和 JS 开发一个区块
description: 使用 CSS 和 JavaScript 为 Edge Delivery Services 开发一个区块，该区块可通过通用编辑器进行编辑。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 41c4cfcf-0813-46b7-bca0-7c13de31a20e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '772'
ht-degree: 100%

---

# 使用 CSS 和 JavaScript 开发一个区块

在[上一章](./7b-block-js-css.md)中，我们讨论了仅使用 CSS 对块进行样式设置。现在，我们将重点转向使用 JavaScript 和 CSS 共同开发一个区块。

这个例子展示了如何通过三种方式来增强区块：

1. 添加自定义 CSS 类。
1. 使用事件监听器来添加移动效果。
1. 处理 Teaser 文本中可选择包含的条款和条件。

## 常见用例

这种方法在以下场景中尤为有用：

- **外部 CSS 管理：**&#x200B;当区块的 CSS 在 Edge Delivery Services 之外进行管理，且与其 HTML 结构不一致时。
- **附加属性：**&#x200B;当需要额外的属性时，例如用于辅助功能的 [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) 或[微数据](https://developer.mozilla.org/en-US/docs/Web/HTML/Microdata)。
- **JavaScript 增强功能：**&#x200B;当需要事件监听器等交互功能时。

这种方法依赖于浏览器原生 JavaScript DOM 操作，但在修改 DOM 时需要谨慎，尤其是在移动元素时更是如此。此类更改可能会破坏通用编辑器的创作体验。理想情况下，应精心设计区块的[内容模型](./5-new-block.md#block-model)，以尽量减少对大量更改 DOM 的需求。

## 区块 HTML

要着手进行区块开发，首先需要查看 Edge Delivery Services 所展示的 DOM。该结构通过 JavaScript 进行增强，并使用 CSS 进行样式设置。

>[!BEGINTABS]

>[!TAB 要装饰的 DOM]

以下是使用 JavaScript 和 CSS 进行装饰的目标 Teaser 区块的 DOM。

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB 如何找到 DOM]

要找到需要装饰的 DOM，请在本地开发环境中打开包含未装饰的区块的页面，选中该区块，并检查其 DOM 结构。

![检查区块 DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## 区块 JavaScript

若要向某个区块添加 JavaScript 功能，请在该区块的目录中创建一个与该区块同名的 JavaScript 文件，例如，`/blocks/teaser/teaser.js`。

JavaScript 文件应导出一个默认函数：

```javascript
export default function decorate(block) { ... }
```

默认函数接收 Edge Delivery Services HTML 中代表区块的 DOM 元素/树，并包含在区块呈现时执行的自定义 JavaScript。

这个 JavaScript 示例执行了三个主要操作：

1. 为 CTA 按钮添加一个事件监听器，当鼠标悬停时缩放图像。
1. 为区块的元素添加语义化 CSS 类，这在集成现有 CSS 设计系统时非常有用。
1. 为以 `Terms and conditions:` 开头的段落添加一个特殊的 CSS 类。

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="下面是代码示例的文件名。"}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## 区块 CSS

如果你在[上一章](./7a-block-css.md)中创建了 `teaser.css`，请删除它，或者将其重命名为 `teaser.css.bak`，因为本章为 Teaser 区块实施了不同的 CSS。

在区块的文件夹中创建一个 `teaser.css` 文件。该文件包含用于设置区块样式的 CSS 代码。该 CSS 代码针对的是区块的元素，以及由 `teaser.js` 中的 JavaScript 添加的特定语义 CSS 类。

裸元素仍然可以直接设置样式，或者使用自定义的 CSS 类进行样式设置。对于更复杂的区块，应用语义化 CSS 类有助于使 CSS 更易于理解和维护，尤其是在较长时期与大型团队合作时更是如此。

[和之前一样](./7a-block-css.md#develop-a-block-with-css)，使用 [CSS 嵌套](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting)将 CSS 限定在 `.block.teaser` 上，以避免与其他区块发生冲突。

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="下面是代码示例的文件名。"}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 

    /* The teaser image */
    .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;

            .zoom {
                transform: scale(1.1);
            }            
        }
    }

    /* The teaser text content */
    .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        .title::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        
            &.terms-and-conditions {
                font-size: var(--body-font-size-xs);
                color: var(--secondary-color);
                padding: .5rem 1rem;
                font-style: italic;
                border: solid var(--light-color);
                border-width: 0 0 0 10px;
            }
        }

        /* Add underlines to links in the text */
        a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        .button-container {
            margin: 0;
            padding: 0;
        
            .button {   
                background-color: var(--primary-color);
                border-radius: 0;
                color: var(--dark-color);
                font-size: var(--body-font-size-xs);
                font-weight: bold;
                padding: 1em 2.5em;
                margin: 0;
                text-transform: uppercase;
            }
        }
    }
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## 添加条款和条件

上述实施增加了对以 `Terms and conditions:` 文本开头的段落进行特殊样式设置的支持。为了验证此功能，请在通用编辑器中更新 Teaser 区块的文本内容，使其包含条款和条件。

按照[创作区块](./6-author-block.md)中的步骤操作，并编辑文本以在最后添加一段&#x200B;**条款和条件**：

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

确认该段落已在本地开发环境中按照条款和条件样式进行呈现。请记住，这些代码变更在[推送到通用编辑器使用的 GitHub 分支](#preview-in-universal-editor)后，才会反映在通用编辑器中。

## 开发预览

在添加 CSS 和 JavaScript 的过程中，AEM CLI 的本地开发环境会实时热加载更改，方便快速直观地查看代码对区块的影响。将鼠标悬停在 CTA 上，确认 Teaser 图像是否有放大和缩小的效果。

![使用 CSS 和 JS 进行 Teaser 的本地开发预览](./assets/7b-block-js-css/local-development-preview.png)

## 对代码进行规范检查

请确保[经常](./3-local-development-environment.md#linting)对代码更改进行规范检查，以保持代码的整洁和一致性。定期进行代码规范检查有助于及早发现问题，减少整体开发时间。请记住，只有在所有代码规范问题解决后，才能将开发工作合并到 `main` 分支！

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## 在通用编辑器中预览

要在 AEM 的通用编辑器中查看更改，请将更改添加、提交并推送到通用编辑器使用的 Git 存储库分支。这样做可以确保区块的实施不会影响创作体验。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

现在，在添加 `?ref=teaser` 查询参数时，即可在通用编辑器中预览这些更改。

![通用编辑器中的 Teaser](./assets/7b-block-js-css/universal-editor-preview.png)
