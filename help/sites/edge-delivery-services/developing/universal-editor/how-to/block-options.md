---
title: 区块选项
description: 了解如何构建具有多个显示选项的区块。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-17296
duration: 700
exl-id: f41dff22-bd47-4ea0-98cc-f5ca30b22c4b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1961'
ht-degree: 100%

---

# 开发带有选项的区块

本教程基于 Edge Delivery Services 和通用编辑器教程，引导您完成向区块添加区块选项的过程。通过定义区块选项，您可以自定义区块的外观和功能，从而支持不同的变体，以满足各种内容需求。这使您网站的设计系统更具灵活性和可重用性。

![并排区块选项](./assets/block-options/main.png){align="center"}

在本教程中，您将向 Teaser 区块添加区块选项，使作者能够在两种显示选项之间进行选择：**默认选项**&#x200B;和&#x200B;**并排选项**。**默认**&#x200B;选项会将图像显示在文本上方和后方，而&#x200B;**并排选项**&#x200B;则会将图像和文本并排显示。

## 常见用例

在 **Edge Delivery Services** 和&#x200B;**通用编辑器**&#x200B;开发中使用&#x200B;**区块选项**&#x200B;的常见用例包括但不限于：

1. **布局变化：**&#x200B;轻松切换布局。例如，水平与垂直布局，或网格与列表布局。
2. **样式变化：**&#x200B;轻松切换主题或视觉处理方式。例如，浅色模式与深色模式，或大字体文本与小字体文本。
3. **内容显示控制：**&#x200B;切换元素可见性或在内容样式（紧凑型与详细型）之间切换。

这些选项为构建动态且适应性强的区块提供了灵活性和效率。

本教程演示了布局变体用例，其中 Teaser 区块可以由两种不同的布局显示：**默认布局**&#x200B;和&#x200B;**并排布局**。

## 区块模型

若要在 Teaser 区块中添加区块选项，请打开位于 `/block/teaser/_teaser.json` 的 JSON 片段，并在模型定义中添加一个新字段。该字段将其 `name` 属性设置为 `classes`，这是一个受保护的字段，AEM 使用它来存储应用于区块的 Edge Delivery Services HTML 的区块选项。

### 字段配置

下面的选项卡展示了在区块模型中配置区块选项的不同方式，包括使用单个 CSS 类的单选、使用多个 CSS 类的单选以及使用多个 CSS 类的多选。本教程采用了使用&#x200B;**单个 CSS 类的单选**&#x200B;这一[更简单的方法](#field-configuration-for-this-tutorial)。

>[!BEGINTABS]

>[!TAB 使用单个 CSS 类的单选]

本教程演示了如何使用 `select`（下拉）输入类型让作者选择一个区块选项，然后将其应用为单个相应的 CSS 类。

![使用单个 CSS 类的单选](./assets/block-options/tab-1.png){align="center"}

#### 区块模型

**默认**&#x200B;选项以空字符串 (`""`) 表示，而&#x200B;**并排**&#x200B;选项则使用 `"side-by-side"`。选项的&#x200B;**名称**&#x200B;和&#x200B;**值**&#x200B;不必相同，但&#x200B;**值**&#x200B;决定了应用于区块级 HTML 的 CSS 类。例如，**并排**&#x200B;选项的值可以是 `layout-10`，而不是 `side-by-side`。然而，最好为 CSS 类使用语义上有意义的名称，以确保选项值的清晰性和一致性。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json{highlight="4,8,9-18"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            }
        ]
    }
]
...
```

#### 区块 HTML

当作者选择某个选项时，相应的值将作为 CSS 类添加到该区块的 HTML 中

- 如果选择了&#x200B;**默认**：

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- 如果选择了&#x200B;**并排**：

  ```html
  <div class="block teaser side-by-side">
      <!-- Block content here -->
  </div>
  ```

这样可以根据所选的选项应用不同的样式和条件式 JavaScript。


>[!TAB 使用多个 CSS 类的选择]

**本教程中不使用此方法，但它展示了一种替代方案以及更高级的区块配置选项。**

`select` 输入类型允许作者选择一个单一的区块选项，该选项可以选择性地映射到多个 CSS 类。为实现此功能，请将多个 CSS 类以空格分隔的形式列出。

![使用多个 CSS 类的选择](./assets/block-options/tab-2.png){align="center"}

#### 区块模型

例如，**并排**&#x200B;选项可以支持不同的变体，其中图像可以显示在左侧 (`side-by-side left`) 或右侧 (`side-by-side right`)。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json{highlight="4,8,9-21"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side with Image on left",
                "value": "side-by-side left"
            },
            {
                "name": "Side-by-side with Image on right",
                "value": "side-by-side right"
            }
        ]
    }
]
...
```

#### 区块 HTML

当作者选择某个选项时，对应的值会作为一组以空格分隔的 CSS 类，应用到该区块的 HTML 中：

- 如果选择了&#x200B;**默认**：

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- 如果选择了&#x200B;**图像在左侧的并排显示**：

  ```html
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- 如果选择了&#x200B;**图像在右侧的并排显示**：

  ```html
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

这样可以根据所选的选项应用不同的样式和条件式 JavaScript。


>[!TAB 使用多个 CSS 类进行多选]

**本教程中不使用此方法，但它展示了一种替代方案以及更高级的区块配置选项。**

`"component": "multiselect"` 输入类型允许作者同时选择多个选项。这使得通过组合多个设计选项来实现区块外观的复杂变体成为可能。

![使用多个 CSS 类进行多选](./assets/block-options/tab-3.png){align="center"}

### 区块模型

例如，**并排显示**、**图像在左侧**&#x200B;和&#x200B;**图像在右侧**&#x200B;可以支持图像显示在左侧 (`side-by-side left`) 或右侧 (`side-by-side right`) 的不同变体。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json{highlight="4,6,8,10-21"}
...
"fields": [
    {
        "component": "multiselect",
        "name": "classes",
        "value": [],
        "label": "Teaser options",
        "valueType": "array",
        "options": [
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            },
            {
                "name": "Image on left",
                "value": "left"
            },
            {
                "name": "Image on right",
                "value": "right"
            }
        ]
    }
]
...
```

#### 区块 HTML

当作者选择多个选项时，对应的值会作为以空格分隔的 CSS 类，应用到区块的 HTML 中：

- 如果选择了&#x200B;**并排**&#x200B;和&#x200B;**图像在左侧**：

  ```html{highlight="1"}
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- 如果选择了&#x200B;**并排**&#x200B;和&#x200B;**图像在右侧**：

  ```html{highlight="1"}
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

虽然多选功能提供了更高的灵活性，但也增加了管理设计变体的复杂度。如果没有限制，冲突的选择可能导致功能异常或偏离品牌规范的体验。

例如：

- 如果选择了&#x200B;**图像在左侧**&#x200B;或&#x200B;**图像在右侧**，但未选择&#x200B;**并排显示**，则会隐式地将它们应用到&#x200B;**默认**，该选项始终会将图像设置为背景，因此左侧和右侧的对齐方式无关紧要。
- 同时选择&#x200B;**图像在左侧**&#x200B;和&#x200B;**图像在右侧**&#x200B;是相互矛盾的。
- 选择&#x200B;**并排**&#x200B;而不选择&#x200B;**图像在左侧**&#x200B;或&#x200B;**图像在右侧**&#x200B;可能会被视为不明确，因为图像的位置未指定。

为避免出现问题及作者在使用多选时产生困惑，请确保选项设计合理且对所有组合情况进行充分测试。多选功能最适合用于简单且不冲突的增强选项，如“large”（大号）或“highlight”（高亮），而不适用于改变布局的选择。


>[!TAB 默认选项]

**本教程中不使用此方法，但它展示了一种替代方案以及更高级的区块配置选项。**

在通用编辑器中，添加新区块实例时，可以将区块选项设置为默认值。这是通过在[区块定义](../5-new-block.md#block-definition)中设置 `classes` 属性的默认值来实现。

#### 区块定义

在下面的示例中，通过将 `classes` 字段的 `value` 属性设置为 `side-by-side`，将默认选项设为&#x200B;**并排显示**。区块模型中对应的区块选项输入是可选的。

您也可以为同一个区块定义多个条目，每个条目拥有不同的名称和类。这使得通用编辑器能够显示不同的区块条目，每个条目都预先配置了特定的区块选项。虽然在编辑器中它们表现为独立的区块，但代码库实际上只包含一个区块，该区块会根据所选选项动态渲染内容。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json{highlight="12"}
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "classes": "side-by-side",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

>[!ENDTABS]


### 本教程的字段配置


在本教程中，我们将使用上文在第一个选项卡中描述的带有单个 CSS 类的选择方法，该方法允许两个独立的区块选项：**默认选项**&#x200B;和&#x200B;**并排选项**。

在区块 JSON 片段的模型定义中，为区块选项添加一个单选字段。该字段允许作者在默认布局和并排布局之间进行选择。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json{highlight="7-24"}
{
    "definitions": [...],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "select",
                    "name": "classes",
                    "value": "",
                    "label": "Teaser options",
                    "description": "",
                    "valueType": "string",
                    "options": [
                        {
                            "name": "Default",
                            "value": ""
                        },
                        {
                            "name": "Side-by-side",
                            "value": "side-by-side"
                        }
                    ]
                },
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

## 在通用编辑器中更新区块

为了在通用编辑器中使更新后的区块选项输入可用，请将 JSON 代码更改部署到 GitHub，创建一个新页面，添加并创作带有&#x200B;**并排**&#x200B;选项的 Teaser 区块，然后发布页面，以进行预览。发布后，在本地开发环境中加载页面，以进行编码。

### 将更改推送到 GitHub

为了在通用编辑器中提供更新的区块选项输入，以便设置区块选项并针对生成的 HTML 进行开发，必须对项目进行代码检查，并将更改推送到 GitHub 分支——在本例中为 `block-options` 分支。

```bash
# ~/Code/aem-wknd-eds-ue

# Lint the changes to catch any syntax errors
$ npm run lint 

$ git add .
$ git commit -m "Add Teaser block option to JSON file so it is available in Universal Editor"
$ git push origin teaser
```

### 创建一个测试页面

在 AEM Author 服务中，创建一个新的页面，以添加用于开发的 Teaser 区块。遵循 [Edge Delivery Services 和通用编辑器开发人员教程](../0-overview.md)中[创作区块](../6-author-block.md)章节中的约定，在 `branches` 页面下创建一个测试页面，并将其命名为您正在处理的 Git 分支名称——在本例中为 `block-options`。

### 创作区块

在通用编辑器中编辑新的&#x200B;**区块选项**&#x200B;页面，并添加&#x200B;**Teaser**&#x200B;区块。请确保在 URL 中添加查询参数 `?ref=block-options`，以便使用来自 `block-options` GitHub 分支的代码加载页面，

区块对话框现在包含一个带有&#x200B;**默认**&#x200B;和&#x200B;**并排**&#x200B;选项的&#x200B;**Teaser 选项**&#x200B;下拉菜单。选择&#x200B;**并排**&#x200B;并完成剩余的内容创作。

![带有选项区块对话框的 Teaser](./assets/block-options/block-dialog.png){align="center"}

（可选）添加两个 **Teaser** 区块——一个设置为&#x200B;**默认**，另一个设置为&#x200B;**并排**。这样，您可以在开发过程中并排预览两个选项，并确保实施&#x200B;**并排**&#x200B;选项不会影响&#x200B;**默认**&#x200B;选项。

### 发布到预览环境

在将 Teaser 区块添加到页面后，请使用&#x200B;**发布**&#x200B;按钮并在通用编辑器中选择发布到&#x200B;**预览**&#x200B;环境来[发布页面，以进行预览](../6-author-block.md)。

## 区块 HTML

要开始开发区块，请首先查看 Edge Delivery Services 预览版呈现的 DOM 结构。DOM 通过 JavaScript 进行增强，并使用 CSS 进行样式设计，为构建和定制区块提供了基础。

>[!BEGINTABS]

>[!TAB 要装饰的 DOM]

以下是选择了&#x200B;**并排**&#x200B;区块选项的 Teaser 区块的 DOM，这是使用 JavaScript 和 CSS 进行装饰的目标对象。

```html{highlight="7"}
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block side-by-side" data-block-name="teaser" data-block-status="loaded">
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
                    <p>Terms and conditions: By signing up, you agree to the rules for participation and booking.</p>
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

要查找需要装饰的 DOM，请在本地开发环境中打开包含该区块的页面，使用 Web 浏览器的开发人员工具选择该区块，并检查其 DOM 结构。这样您就可以识别出需要装饰的相关元素。

![检查区块 DOM](./assets/block-options/dom.png){align="center"}

>[!ENDTABS]

## 区块 CSS

编辑 `blocks/teaser/teaser.css`，为&#x200B;**并排**&#x200B;选项添加特定的 CSS 样式。该文件包含该区块的默认 CSS。

要修改&#x200B;**并排**&#x200B;选项的样式，请在 `teaser.css` 文件中添加一个新的作用域 CSS 规则，该规则针对的是配置了 `side-by-side` 类的 Teaser 区块。

```css
.block.teaser.side-by-side { ... }
```

或者，你可以使用 CSS 嵌套来实现更简洁的版本：

```css
.block.teaser {
    ... Default teaser block styles ...

    &.side-by-side {
        ... Side-by-side teaser block styles ...
    }
}
```

在 `&.side-by-side` 规则中，添加必要的 CSS 属性，以便在应用 `side-by-side` 类时对区块进行样式设置。

一种常见的方法是，先对共享选择器应用 `all: initial` 来重置默认样式，然后为 `side-by-side` 变体添加所需的样式。如果大多数样式在各个选项之间是共享的，那么覆盖特定属性可能会更容易。然而，如果多个选择器需要更改，则重置所有样式并仅重新应用必要的样式可以使代码更清晰、更易于维护。
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
        }

        p.terms-and-conditions {
            font-size: var(--body-font-size-xs);
            color: var(--secondary-color);
            padding: .5rem 1rem;
            font-style: italic;
            border: solid var(--light-color);
            border-width: 0 0 0 10px;
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

    /**
    *  Add styling for the side-by-side variant 
    **/

    /* This evaluates to .block.teaser.side-by-side */
    &.side-by-side {    
        /* Since this default teaser option doesn't have a style (such as `.default`), we use `all: initial` to reset styles rather than overriding individual styles. */
        all: initial;
        display: flex;
        margin: auto;
        max-width: 900px;

        .image-wrapper {
            all: initial;
            flex: 2;
            overflow: hidden;                 
            
            * {
                height: 100%;
            }        

            .image {
                object-fit: cover;
                object-position: center;
                width: 100%;
                height: 100%;
                transform: scale(1); 
                transition: transform 0.6s ease-in-out;                

                &.zoom {
                    /* This option has a different zoom level than the default */
                    transform: scale(1.5);
                }
            }
        }

        .content {
            all: initial;
            flex: 1;
            background-color: var(--light-color);
            padding: 3.5em 2em 2em;
            font-size: var(--body-font-size-s);
            font-family: var(--body-font-family);
            text-align: justify;
            text-justify: newspaper;
            hyphens: auto;

            p.terms-and-conditions {
                border: solid var(--text-color);
                border-width: 0;
                padding-left: 0;
                text-align: left;
            }
        }

        /* Media query for mobile devices */
        @media (width <= 900px) {
            flex-direction: column; /* Stack elements vertically on mobile */
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


## 区块 JavaScript

通过检查应用于区块元素的类，可以轻松识别该区块的活动选项。在此示例中，我们需要根据活动选项调整 `.image-wrapper` 样式的应用位置。

`getOptions` 函数返回一个应用于该区块的类的数组，其中不包括 `block` 和 `teaser`（因为所有区块都有 `block` 类，所有 Teaser 区块都有 `teaser` 类）。数组中剩余的任何类都表示活动选项。如果数组为空，则应用默认选项。

```javascript
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'; anything remaining is a block option.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}
```

此选项列表可用于在区块的 JavaScript 中有条件地执行自定义逻辑：

```javascript
if (getOptions(block).includes('side-by-side')) {
  /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
  block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
} else if (!getOptions(block)) {
  /* For the default option, add the image-wrapper to the picture element to support CSS */
  block.querySelector('picture').classList.add('image-wrapper');
}
```

带有默认选项和并排选项的 Teaser 区块完整更新后的 JavaScript 文件如下：

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="下面是代码示例的文件名。"}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Block options are applied as classes to the block's DOM element
 * alongside the `block` and `<block-name>` classes.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}

/**
 * Adds a zoom effect to the image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's DOM tree
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
 * Entry point to the block's JavaScript.
 * Must be exported as default and accept a block's DOM element.
 * This function is called by the project's style.js, passing the block's element.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
export default function decorate(block) {
  /* Common treatments for all options */
  block.querySelector(':scope > div:last-child').classList.add('content');
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');
  block.querySelector('img').classList.add('image');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();
    if (innerHTML?.startsWith('Terms and conditions:')) {
      p.classList.add('terms-and-conditions');
    }
  });

  /* Conditional treatments for specific options */
  if (getOptions(block).includes('side-by-side')) {
    /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
    block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
  } else if (!getOptions(block)) {
    /* For the default option, add the image-wrapper to the picture element to support CSS */
    block.querySelector('picture').classList.add('image-wrapper');
  }

  addEventListeners(block);
}
```

## 开发预览

在添加 CSS 和 JavaScript 的过程中，AEM CLI 的本地开发环境会实时热加载更改，方便快速直观地查看代码对区块的影响。将鼠标悬停在 CTA 上，确认 Teaser 图像是否有放大和缩小的效果。

![使用 CSS 和 JS 进行 Teaser 的本地开发预览](./assets/block-options//local-development-preview.png)

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
$ git commit -m "CSS and JavaScript implementation for Teaser block option Side-by-side"
# JSON files are compiled automatically and added to the commit via a Husky pre-commit hook
$ git push origin block-options
```

现在，在使用 `?ref=block-options` 查询参数时，通用编辑器中即可看到这些更改。

![通用编辑器中的 Teaser](./assets/block-options/universal-editor-preview.png){align="center"}


## 恭喜！

您现在已经了解了 Edge Delivery Services 和通用编辑器中的区块选项，掌握了更加灵活地定制和简化内容编辑的工具。开始在您的项目中应用这些选项，以提升效率并保持一致性。

如需了解更多最佳实践和高级技巧，请查阅[通用编辑器文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)。
