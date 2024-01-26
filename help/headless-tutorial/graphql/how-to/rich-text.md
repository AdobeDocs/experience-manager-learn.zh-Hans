---
title: 在AEM Headless中使用富文本
description: 了解如何使用带Adobe Experience Manager内容片段的多行富文本编辑器创作内容并嵌入引用的内容，以及如何通过AEM GraphQL API以JSON形式提供RTF以供Headless应用程序使用。
version: Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 821
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# AEM Headless多信息文本

多行文本字段是内容片段的数据类型，它允许作者创建富文本内容。 对其他内容（如图像或其他内容片段）的引用可以动态地内嵌在文本流中。 单行文本字段是内容片段的另一种数据类型，应当用于简单文本元素。

AEM GraphQL API提供了一项强大的功能，可将RTF作为HTML、纯文本或纯JSON返回。 JSON表示法非常强大，因为它使客户端应用程序可以完全控制如何呈现内容。

## 多行编辑器

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

在内容片段编辑器中，多行文本字段的菜单栏为作者提供了标准的富文本格式功能，例如 **粗体**， *斜体*、和下划线。 以全屏模式打开多行字段会启用 [其他格式工具，如段落文字、查找和替换、拼写检查等](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

>[!NOTE]
>
> 无法自定义多行编辑器中的富文本插件。

## 多行文本数据类型 {#multi-line-data-type}

使用 **多行文本** 定义内容片段模型以启用富文本创作时的数据类型。

![多行富文本数据类型](assets/rich-text/multi-line-rich-text.png)

可以配置多行字段的多个属性。

此 **呈现为** 属性可以设置为：

* 文本区域 — 呈现单个多行字段
* 多个字段 — 呈现多个多行字段


此 **默认类型** 可以设置为：

* 富文本
* Markdown
* 纯文本

此 **默认类型** 选项直接影响编辑体验，并决定富文本工具是否存在。

您还可以 [启用内联引用](#insert-fragment-references) 到其他内容片段，方法是选中 **允许片段引用** 并配置 **允许的内容片段模型**.

查看 **可翻译** 框（如果要对内容进行本地化）。 只能本地化富文本和纯文本。 请参阅 [使用本地化内容以了解更多详细信息](./localized-content.md).

## GraphQL API富文本响应

创建GraphQL查询时，开发人员可以从中选择不同的响应类型 `html`， `plaintext`， `markdown`、和 `json` 来自多行字段。

开发人员可以使用 [JSON预览](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) 在内容片段编辑器中，显示可使用GraphQL API返回的当前内容片段的所有值。

## GraphQL持久查询

选择 `json` 处理富文本内容时，多行字段的响应格式可提供最大的灵活性。 富文本内容以JSON节点类型数组的形式交付，可以基于客户端平台唯一处理这些类型。

以下是名为的多行字段的JSON响应类型 `main` 包含以下段落： ”*此段落包括&#x200B;**重要**内容。*&quot;，其中“重要”标记为 **粗体**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

此 `$path` 中使用的变量 `_path` 过滤器需要内容片段的完整路径(例如 `/content/dam/wknd/en/magazine/sample-article`)。

**GraphQL响应：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### 其他示例

以下是名为的多行字段的响应类型的几个示例 `main` 包含一段：“这是一个包含 **重要** 内容。” 其中，“重要”标记为 **粗体**.

+++HTML示例

**GraphQL持久查询：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**GraphQL响应：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

+++Markdown示例

**GraphQL持久查询：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**GraphQL响应：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++纯文本示例

**GraphQL持久查询：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**GraphQL响应：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

此 `plaintext` 渲染选项会去除任何格式。

+++


## 呈现富文本JSON响应 {#render-multiline-json-richtext}

多行字段的富文本JSON响应被构造为分层树。 每个对象或节点表示富文本的不同HTML块。

以下是多行文本字段的JSON响应示例。 请注意，每个对象或节点都包含 `nodeType` 表示富文本中的HTML块，如 `paragraph`， `link`、和 `text`. 每个节点（可选）包含 `content` 即包含当前节点的任何子级的子数组。

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

呈现多行内容的最简单方法 `json` 响应是处理响应中的每个对象或节点，然后处理当前节点的任何子节点。 递归函数可用于遍历JSON树。

以下是说明递归遍历方法的示例代码。 这些示例基于JavaScript，并使用React的 [JSX](https://reactjs.org/docs/introducing-jsx.html)但是，编程概念可以应用于任何语言。

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

`renderNodeList` 是一个递归函数，它接受 `childNodes`. 然后，将数组中的每个节点传递到函数 `renderNode`，依次调用 `renderNodeList` 如果节点具有子节点，则为。

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

此 `renderNode` 函数需要一个名为的对象 `node`. 节点可以具有子节点，这些子节点使用 `renderNodeList` 函数中。 最后， `nodeMap` 用于根据节点的 `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

此 `nodeMap` 是用于映射的JavaScript对象文字。 每个“键”代表不同的 `nodeType`. 参数 `node` 和 `children` 可以传递到渲染节点的生成函数。 此示例中使用的返回类型是JSX，但该方法可以适用于构建表示HTML内容的字符串文字。

### 完整代码示例

可重复使用的富文本呈现实用程序可在 [WKND GraphQL React示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js)  — 公开函数的可重用实用程序 `mapJsonRichText`. 此实用程序可供希望将富文本JSON响应渲染为React JSX的组件使用。
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)  — 发出包含富文本的GraphQL请求的示例组件。 组件使用 `mapJsonRichText` 用于呈现富文本和任何引用的实用程序。


## 添加对富文本的内联引用 {#insert-fragment-references}

利用多行字段，作者可以在富文本流中插入来自AEM Assets的图像或其他数字资产。

![插入图像](assets/rich-text/insert-image.png)

上述屏幕快照描述了使用插入多行字段中的图像 **插入资源** 按钮。

对其他内容片段的引用也可以使用在多行字段中链接或插入 **插入内容片段** 按钮。

![插入内容片段引用](assets/rich-text/insert-contentfragment.png)

上面的屏幕截图描述了另一个内容片段，洛杉矶滑板公园终极指南，插入多行字段中。 可插入到字段中的内容片段的类型由控制 **允许的内容片段模型** 中的配置 [多行数据类型](#multi-line-data-type) 在内容片段模型中。

## 使用GraphQL查询内联引用

GraphQL API允许开发人员创建查询，查询中包含有关插入到多行字段中的任何引用的其他属性。 JSON响应包含一个单独的 `_references` 列出这些额外属性的对象。 JSON响应使开发人员能够完全控制如何呈现引用或链接，而不必处理教条式HTML。

例如，您可能希望：

* 包含自定义路由逻辑，用于在实施单页应用程序（如使用React Router或Next.js）时管理指向其他内容片段的链接
* 使用指向AEM Publish环境的绝对路径作为呈现内联图像 `src` 值。
* 确定如何使用其他自定义属性呈现对其他内容片段的嵌入引用。

使用 `json` 返回类型并包括 `_references` 构建GraphQL查询时的对象：

**GraphQL持久查询：**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

在上述查询中， `main` 字段作为JSON返回。 此 `_references` 对象包括用于处理任何类型引用的片段 `ImageRef` 或类型 `ArticleModel`.

**JSON响应：**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

JSON响应包括引用在富文本中插入的位置，其中 `"nodeType": "reference"`. 此 `_references` 对象然后包含每个引用。

## 以富文本呈现内联引用

要呈现内联引用，中介绍了递归方法。 [呈现多行JSON响应](#render-multiline-json-richtext) 可以展开。

位置 `nodeMap` 是渲染JSON节点的映射。

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

高级方法是 `nodeType` 等于 `reference` 在多行JSON响应中。 然后，可以调用自定义渲染函数，该函数包括 `_references` GraphQL响应中返回的对象。

然后，可以将内联参考路径与 `_references` 对象和另一个自定义映射 `renderReference` 可以调用。

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

此 `__typename` 的 `_references` 对象可用于将不同的引用类型映射到不同的渲染函数。

### 完整代码示例

有关编写自定义引用渲染器的完整示例，请参见 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) 作为 [WKND GraphQL React示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## 端到端示例

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> 上述视频使用 `_publishUrl` 渲染图像引用。 相反，首选 `_dynamicUrl` 如 [Web优化图像操作说明](./images.md)；


前面的视频展示了一个端到端示例：

1. 更新内容片段模型的多行文本字段以允许片段引用
2. 使用内容片段编辑器在多行文本字段中包含图像和对其他片段的引用。
3. 创建一个GraphQL查询，该查询包括以JSON和任意 `_references` 已使用。
4. 编写React SPA以呈现富文本响应的内联引用。
