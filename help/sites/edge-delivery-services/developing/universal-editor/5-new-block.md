---
title: 创建一个区块
description: 为 Edge Delivery Services 网站构建一个可通过通用编辑器进行编辑的区块。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 9698c17a-0ac8-426d-bccb-729b048cabd1
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1767'
ht-degree: 100%

---

# 创建新的区块

本章介绍了如何使用通用编辑器为 Edge Delivery Services 网站创建新的可编辑 teaser 区块的过程。

![新的 Teaser 区块](./assets//5-new-block/teaser-block.png)

该区块名为 `teaser`，展示了以下元素：

- **图像**：一张视觉上引人入胜的图像。
- **文本内容**：
   - **标题**：一个引人注目的标题，用以吸引关注。
   - **正文文本**：描述性内容，提供上下文或详细信息，包括可选条款和条件。
   - **行动号召 (CTA) 按钮**：一个旨在促进用户互动并引导他们进一步参与的链接。

`teaser` 区块的内容可在通用编辑器中进行编辑，从而确保在整个网站中易于使用且可复用。

请注意，`teaser` 区块与样板中的 `hero` 区块相似；因此，`teaser` 区块仅作为一个简单示例，用于阐释开发概念。

## 创建新的 Git 分支

为了保持清晰有序的工作流程，请为每个具体的开发任务创建一个新的分支。这样可以避免将不完整或未经测试的代码部署到生产环境时出现问题。

1. **从主分支开始**：在最新的生产代码基础上进行开发，可确保打下坚实的基础。
2. **获取远程更改**：从 GitHub 获取最新更新可确保在开始开发之前拥有最新的代码。
   - 示例：在将 `wknd-styles` 分支的更改合并到 `main` 之后，获取最新更新。
3. **创建一个新的分支**：

```bash
# ~/Code/aem-wknd-eds-ue

$ git fetch origin  
$ git checkout -b teaser origin/main  
```

创建 `teaser` 分支后，您就可以开始开发 Teaser 区块了。

## 区块文件夹

在项目的 `blocks` 目录中创建一个名为 `teaser` 的新文件夹。此文件夹包含该区块的 JSON、CSS 和 JavaScript 文件，将区块的所有文件集中管理于此：

```
# ~/Code/aem-wknd-eds-ue

/blocks/teaser
```

该区块文件夹名称作为区块的 ID，并在整个开发过程中用于引用该区块。

## 区块 JSON

区块 JSON 定义了区块的三个关键方面：

- **定义**：在通用编辑器中将该区块注册为可编辑组件，并将其链接到区块模型和（可选）过滤器。
- **模型**：指定区块的创作字段，以及这些字段如何呈现为语义化的 Edge Delivery Services HTML。
- **过滤器**：配置过滤规则，以限制通过通用编辑器可将模块添加到的容器。大多数区块本身并非容器，而是将它们的 ID 添加到其他容器区块的过滤器中。

在 `/blocks/teaser/_teaser.json` 创建一个新文件，并按照以下确切顺序设置初始结构。如果键位混乱，可能无法正确构建。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json
{
    "definitions": [],
    "models": [],
    "filters": []
}
```

### 区块模型

区块模型是区块配置的关键部分，因为它定义了：

1. 通过定义可编辑的字段来优化创作体验。

   ![通用编辑器字段](./assets/5-new-block/fields-in-universal-editor.png)

2. 字段的值如何渲染到 Edge Delivery Services HTML 中。

模型分配了一个 `id`，其与[区块的定义](#block-definition)相对应，并包含一个 `fields` 数组，用于指定可编辑的字段。

`fields` 数组中的每个字段都有一个 JSON 对象，该对象包含以下必需属性：

| JSON 属性 | 描述 |
|---------------|-----------------------------------------------------------------------------------------------------------------------|
| `component` | [字段类型](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#component-types)，例如 `text`、`reference` 或者 `aem-content`。 |
| `name` | 字段名称，映射到在 AEM 中存储该值的 JCR 属性。 |
| `label` | 在通用编辑器中向作者展示的标签。 |

如需查看包括可选属性在内的完整属性列表，请查阅[通用编辑器字段文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#fields)。

#### 区块设计

![Teaser 区块](./assets/5-new-block/block-design.png)

Teaser 区块包含以下可编辑元素：

1. **图像**：代表 Teaser 的视觉内容。
2. **文本内容**：包含标题、正文和行动号召按钮，并置于白色矩形内。
   - **标题**&#x200B;和&#x200B;**正文文本**&#x200B;可以通过相同的富文本编辑器进行创作。
   - **行动号召**&#x200B;可通过&#x200B;**标签**&#x200B;的 `text` 字段和&#x200B;**链接**&#x200B;的 `aem-content` 字段进行创作。

Teaser 区块的设计分为两个逻辑组件（图像和文本内容），确保为用户提供结构化且直观的创作体验。

### 区块字段

定义该区块所需的字段：图像、图像替代文本、文本、CTA 标签和 CTA 链接。

>[!BEGINTABS]

>[!TAB 正确的方法]

**此选项卡展示了对 Teaser 区块进行建模的正确方法。**

该 Teaser 包含两个逻辑部分：图像和文本。为了简化将 Edge Delivery Services HTML 显示为所需网页体验所需的代码，区块模型应反映这一结构。

- 使用[字段折叠](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)功能，将&#x200B;**图像**&#x200B;和&#x200B;**图像替代文本**&#x200B;组合在一起。
- 使用[元素分组](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)和[ CTA 字段折叠](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)将文本内容字段组合在一起。

如果您不熟悉[字段折叠](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)、[元素分组](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)或[类型推断](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)，请在继续之前查阅所链接的文档，因为它们对于创建结构良好的区块模型至关重要。

在下面的例子中：

- [类型推断](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)用于根据 `image` 字段自动创建一个 `<img>` HTML 元素。字段折叠与 `image` 和 `imageAlt` 字段一起使用，以创建 `<img>` HTML 元素。`src` 属性被设置为 `image` 字段的值，而 `alt` 属性被设置为 `imageAlt` 字段的值。
- `textContent` 是一个用于对字段进行分类的组名。它应该是语义性的，但可以是这个区块独有的任何内容。这会通知通用编辑器在最终的 HTML 输出中，将所有带有此前缀的字段都渲染在同一个 `<div>` 元素内。
- 在 `textContent` 组中，也采用了字段折叠的方式来呈现行动号召 (CTA)。CTA 是通过[类型推断](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference)创建的 `<a>`。`cta` 字段用于设置 `<a>` 元素的 `href` 属性，而 `ctaText` 字段则为 `<a ...>` 标记内的链接提供文本内容。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
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

此模型定义了该区块在通用编辑器中的创作输入。

此区块的 Edge Delivery Services HTML 结果会将图像放置在第一个 div 中，并将元素组 `textContent` 字段放置在第二个 div 中。

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image fields  -->
        <picture>
            ...
            <source .../>            
            <img src="..." alt="The authored alt text"/>
        </picture>
    </div>
    <div>
        <!-- This div, via element grouping contains the textContent fields -->
        <h2>The authored title</h2>
        <p>The authored body text</p>
        <a href="/authored/cta/link">The authored CTA label</a>
    </div>
</div>        
```

正如[下一章](./7a-block-css.md)所示，这种 HTML 结构简化了将区块作为一个整体进行样式设置的过程。

要了解不使用字段折叠和元素分组的后果，请参阅上方的&#x200B;**错误的方式**&#x200B;选项卡。

>[!TAB 错误的方式]

**此选项卡展示了构建 Teaser 区块的一种非最优方式，仅作为正确方法的对比。**

在区块模型中将每个字段定义为独立字段，而不使用[字段折叠](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)和[元素分组](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)，这似乎颇具吸引力。然而，这种疏忽使得将该区块作为一个整体进行样式设计变得更加复杂。

例如，Teaser 模型可以在&#x200B;**不**&#x200B;进行字段折叠或元素分组的情况下如下定义：

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
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
                    "name": "alt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "link",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "label",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

该区块的 Edge Delivery Services HTML 会在单独的 `div` 中渲染每个字段的值，这使得内容理解、样式应用以及 HTML 结构调整变得复杂，难以实现所需的设计。

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image  -->
        <picture>
            ...
            <source .../>            
            <img src="/authored/image/reference"/>
        </picture>
    </div>
    <div>
        <p>The authored alt text</p>
    </div>
    <div>
        <h2>The authored title</h2>
        <p>The authored body text</p>
    </div>
    <div>
        <a href="/authored/cta/link">/authored/cta/link</a>
    </div>
    <div>
        The authored CTA label
    </div>
</div>        
```

每个字段都独立存在于自己的 `div` 中，这使得无法将图像与文本内容作为一个整体进行样式设计。通过投入精力和创造力，仍然有可能实现预期的设计效果，但采用[元素分组](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping)将文本内容字段归类，配合[字段折叠](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse)将创作的值作为元素属性进行添加，不仅更简单、更易实现，而且在语义上也更为合理。

请参阅上方的&#x200B;**正确的方法**&#x200B;选项卡，了解如何更好地构建 Teaser 模型。

>[!ENDTABS]


### 区块定义

区块定义在通用编辑器中注册该区块。以下是区块定义中使用的 JSON 属性的细分：

| JSON 属性 | 描述 |
|---------------|-------------|
| `definition.title` | 在通用编辑器的&#x200B;**添加**&#x200B;区块中显示的区块标题。 |
| `definition.id` | 该区块的唯一标识符，用于控制其在 `filters` 中的使用。 |
| `definition.plugins.xwalk.page.resourceType` | 定义用于在通用编辑器中渲染组件的 Sling 资源类型。始终使用 `core/franklin/components/block/v#/block` 资源类型。 |
| `definition.plugins.xwalk.page.template.name` | 区块的名称。它应保持小写，并使用连字符连接，以与该区块的文件夹名称保持一致。该值还用于在通用编辑器中标记区块的实例。 |
| `definition.plugins.xwalk.page.template.model` | 将此定义与其 `model` 定义相关联，该定义控制通用编辑器中该块显示的创作字段。这里的值必须与 `model.id` 值匹配。 |
| `definition.plugins.xwalk.page.template.classes` | 可选属性，其值将添加到区块级 HTML 元素的 `class` 属性中。这允许同一区块存在变体。通过在块的[模型](#block-model)中[添加一个类字段](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)，可以使 `classes` 值变得可编辑。 |


以下是区块定义的示例 JSON：

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json
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

在此示例中：

- 该区块名为“Teaser”，并使用 `teaser` 模型，该模型决定了通用编辑器中哪些字段可供编辑。
- 该区块包含 `textContent_text` 字段的默认内容，该字段是一个富文本区域，用于输入标题和正文，以及用于 CTA（行动号召）链接和标签的 `textContent_cta` 和 `textContent_ctaText`。模板中包含初始内容的字段名称与[内容模型字段数组](#block-model)中定义的字段名称相匹配；

这种结构确保在通用编辑器中设置区块时，具有适当的字段、内容模型和资源类型，以进行渲染。

### 区块过滤器

该区块的 `filters` 数组定义了（对于[容器块](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container)）哪些其他区块可以添加到该容器中。过滤器定义了一个可添加到容器中的区块 ID (`model.id`) 列表。

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json
{
  "definitions": [... populated from previous section ...],
  "models": [... populated from previous section ...],
  "filters": []
}
```

Teaser 组件不是一个[容器块](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container)，这意味着您无法向其添加其他区块。因此，其 `filters` 数组为空。相反，应将 Teaser 的 ID 添加到分区区块的过滤器列表中，这样 Teaser 就可以被添加到分区中。

![区块过滤器](./assets/5-new-block/filters.png)

Adobe 提供的区块（如分区区块）将过滤器存储在项目的 `models` 文件夹中。要进行调整，请找到 Adobe 提供的区块（例如 `/models/_section.json`）的 JSON 文件，并将 Teaser 的 ID (`teaser`) 添加到过滤器列表中。该配置会向通用编辑器发出信号，表明可以将 Teaser 组件添加到分区容器区块中。

[!BADGE /models/_section.json]{type=Neutral tooltip="下面是代码示例的文件名。"}

```json
{
  "definitions": [],
  "models": [],
  "filters": [
    {
      "id": "section",
      "components": [
        "text",
        "image",
        "button",
        "title",
        "hero",
        "cards",
        "columns",
        "fragment",
        "teaser"
      ]
    }
  ]
}
```

`teaser` 的 Teaser 区块定义 ID 会被添加到 `components` 数组中。

## 对 JSON 文件进行规范检查

请确保您[经常对修改内容进行规范检查](./3-local-development-environment.md#linting)，以确保其干净一致。经常进行规范检查有助于及早发现问题，从而缩短整体开发周期。`npm run lint:js` 命令也会对 JSON 文件进行规范检查，及时捕捉任何语法错误。

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:js
```

## 构建项目 JSON

配置完区块 JSON 文件（例如 `blocks/teaser/_teaser.json`、`models/_section.json`）后，它们会自动编译成项目的 `component-models.json`、`component-definitions.json` 和 `component-filters.json` 文件。此编译是由包含在 [AEM Boilerplate XWalk 项目模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)中的 [Husky](https://typicode.github.io/husky/) 预提交钩子自动处理的。

构建也可以手动触发，或者使用项目的 [build JSON](./3-local-development-environment.md#build-json-fragments) NPM 脚本来以编程方式触发。

## 部署区块 JSON

为了使该区块在通用编辑器中可用，必须将项目提交并推送到 GitHub 存储库的一个分支，在本例中为 `teaser` 分支。

通用编辑器使用的确切分支名称可以由每个用户通过通用编辑器的 URL 进行调整。

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add teaser block JSON files so it is available in Universal Editor"
# JSON files are compiled automatically and added to the commit via a husky precommit hook
$ git push origin teaser
```

当通用编辑器使用查询参数 `?ref=teaser` 打开时，新的 `teaser` 区块将出现在区块面板中。请注意，该区块没有样式；它以语义化 HTML 的形式呈现区块的字段，仅通过[全局 CSS](./4-website-branding.md#global-css) 进行样式设置。
