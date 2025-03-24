---
title: 使用CSS开发块
description: 使用Edge Delivery Services的CSS开发块，该块可使用通用编辑器进行编辑。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 14cda9d4-752b-4425-a469-8b6f283ce1db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# 使用CSS开发块

Edge Delivery Services中的块使用CSS进行样式设置。 块的CSS文件存储在块的目录中，并且与该块具有相同的名称。 例如，名为`teaser`的块的CSS文件位于`blocks/teaser/teaser.css`。

理想情况下，块应当只需要CSS来设置样式，而无需依靠JavaScript修改DOM或添加CSS类。 对JavaScript的需求取决于块的[内容建模](./5-new-block.md#block-model)及其复杂性。 如果需要，可以添加[块JavaScript](./7b-block-js-css.md)。

使用仅限CSS的方法时，会定位块的（通常）裸语义HTML元素并进行样式设置。

## 阻止HTML

要了解如何设置块的样式，请首先查看Edge Delivery Services公开的DOM，因为该格式可用于设置样式。 通过检查AEM CLI的本地开发环境提供的块，可以找到DOM。 请避免使用通用编辑器的DOM，因为它略有不同。

>[!BEGINTABS]

>[!TAB 样式的DOM]

以下是作为样式设置目标的Teaser块的DOM。

请注意[自动作为Edge Delivery Services JavaScript推断的元素扩充的`<p class="button-container">...`。](./4-website-branding.md#inferred-elements)

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

>[!TAB 如何查找DOM]

要查找要设置样式的DOM，请在本地开发环境中打开包含未设置样式的块的页面，选择块，然后检查DOM。

![检查块DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]

## 阻止CSS

在块的文件夹中创建一个新的CSS文件，使用块的名称作为文件名。 例如，对于&#x200B;**Teaser**&#x200B;块，该文件位于`/blocks/teaser/teaser.css`。

当Edge Delivery Services的JavaScript在页面上检测到表示Teaser块的DOM元素时，将自动加载此CSS文件。

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="下面代码示例的文件名。"}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` using CSS nesting (https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in .6s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 

    /* The image is rendered to the first div in the block */
    picture {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;

        img {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
        }
    }

    /** 
    The teaser's text is rendered to the second (also the last) div in the block.

    These styles are scoped to the second (also the last) div in the block (.block.teaser > div:last-child).

    This div order can be used to target different styles to the same semantic elements in the block. 
    For example, if the block has two images, we could target the first image with `.block.teaser > div:first-child img`, 
    and the second image with `.block.teaser > div:nth-child(2) img`.
    **/
    & > div:last-child {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;

        /** 
        The following elements reside within `.block.teaser > div:last-child` and could be scoped as such, for example:

        .block.teaser > div:last-child p { .. }

        However since these element can only appear in the second/last div per our block's model, it's unnecessary to add this additional scope.
        **/

        /* Regardless of the authored heading level, we only want one style the heading */
        h1,
        h2,
        h3,
        h4,
        h5,
        h6 {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        h1::after,
        h2::after,
        h3::after,
        h4::after,
        h5::after,
        h6::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
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

## 开发预览

由于CSS是在代码项目中写入的，因此AEM CLI的热重新加载会进行更改，以便快速轻松地了解CSS如何影响块。

![仅CSS预览](./assets/7a-block-css/local-development-preview.png)

## 嵌入代码

确保您[频繁lint](./3-local-development-environment.md#linting)您的代码更改以确保它是干净一致的。 嵌套有助于及早发现问题，并缩短总体开发时间。 请记住，在解决您的所有Linting问题之前，您无法将开发工作合并到`main`！

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:css
```

## 在通用编辑器中预览

要在AEM的通用编辑器中查看更改，请添加、提交这些更改，并将其推送到通用编辑器使用的Git存储库分支。 此步骤有助于确保块实施不会中断创作体验。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add CSS-only implementation for teaser block"
$ git push origin teaser
```

现在，添加`?ref=teaser`查询参数时，您可以在通用编辑器中预览更改。

通用编辑器中的![Teaser](./assets/7a-block-css/universal-editor-preview.png)
