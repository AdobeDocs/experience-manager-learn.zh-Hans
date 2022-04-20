---
title: 将富文本与AEM Headless结合使用
description: 了解如何使用包含Adobe Experience Manager内容片段的多行富文本编辑器来创作内容和嵌入引用内容，以及如何将富文本由AEM GraphQL API作为JSON交付，以供无标题应用程序使用。
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 0%

---

# 富文本，带AEM Headless

多行文本字段是内容片段的数据类型，使作者能够创建富文本内容。 对其他内容（如图像或其他内容片段）的引用可以动态地插入到文本流中的行内。 AEM GraphQL API提供了一项强大的功能，可将富文本作为HTML、纯文本或纯JSON返回。 JSON表示形式非常强大，因为它赋予客户端应用程序对如何渲染内容的完全控制权。

## 多行编辑器

>[!VIDEO](https://video.tv.adobe.com/v/342104/?quality=12&learn=on)

在内容片段编辑器中，多行文本字段的菜单栏为作者提供了标准富文本格式功能，例如 **粗体**, *斜体*、和下划线。 以全屏模式打开多行字段可启用 [其他格式工具，如段落类型、查找和替换、拼写检查等](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

## 多行文本数据类型 {#multi-line-data-type}

使用 **多行文本** 数据类型。

![多行富文本数据类型](assets/rich-text/multi-line-rich-text.png)

使用多行文本数据类型时，可以设置 **默认类型** 至：

* 富文本
* Markdown
* 纯文本

的 **默认类型** 选项会直接影响编辑体验，并确定富文本工具是否存在。

您还可以 [启用行内引用](#insert-fragment-references) 通过检查 **允许片段引用** 和配置 **允许的内容片段模型**.

## 使用GraphQL API的富文本响应

创建GraphQL查询时，开发人员可以从 `html`, `plaintext`, `markdown`和 `json` 中。

开发人员可以使用 [JSON预览](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) 在内容片段编辑器中，显示可使用GraphQL API返回的当前内容片段的所有值。

### JSON示例

的 `json` 响应为前端开发人员提供了处理富文本内容时的最大灵活性。 富文本内容以JSON节点类型数组的形式交付，这些节点类型可以基于客户端平台进行唯一处理。

以下是名为的多行字段的JSON响应类型 `main` 包含段落：&quot;*这是一个段落，其中包括&#x200B;**重要**内容。*&quot;其中&quot;important&quot;标记为 **粗体**.

**GraphQL查询：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
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

以下是名为的多行字段的响应类型的几个示例 `main` 包含段落：“这段话包括 **重要** 内容。” 其中，“important”标记为 **粗体**.

+++HTML示例

**GraphQL查询：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
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

**GraphQL查询：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
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

**GraphQL查询：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
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

的 `plaintext` 呈现选项会删除任何格式。

+++


## 呈现富文本JSON响应 {#render-multiline-json-richtext}

多行字段的富文本JSON响应的结构为层次树。 每个对象或节点表示富文本的不同HTML块。

下面是多行文本字段的JSON响应示例。 请注意，每个对象或节点都包含 `nodeType` 表示来自富文本（如）的HTML块 `paragraph`, `link`和 `text`. 每个节点（可选）包含 `content` 是包含当前节点的任何子项的子数组。

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

渲染多行的最简单方法 `json` 响应是处理响应中的每个对象或节点，然后处理当前节点的任何子项。 递归函数可用于遍历JSON树。

下面是说明递归遍历方法的示例代码。 这些示例基于JavaScript，并使用React的 [JSX](https://reactjs.org/docs/introducing-jsx.html)但是，编程概念可以应用于任何语言。

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

的 `renderNodeList` 函数是递归算法的入口点。 的 `renderNodeList` 函数需要数组 `childNodes`. 然后，数组中的每个节点都将传递到函数 `renderNode`.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

的 `renderNode` 函数需要名为的单个对象 `node`. 节点可以具有子节点，该子节点使用 `renderNodeList` 函数。 最后， `nodeMap` 用于根据节点的 `nodeType`.

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

的 `nodeMap` 是用作映射的JavaScript对象文本。 每个“键”代表不同的 `nodeType`. 的参数 `node` 和 `children` 可以传递到呈现节点的生成函数。 此示例中使用的返回类型是JSX，但是，可以采用此方法来构建表示HTML内容的字符串文本。

### 完整代码示例

可在 [WKND GraphQL React示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app/src/utils/renderRichText.js)  — 公开函数的可重用实用程序 `mapJsonRichText`. 此实用程序可供要将富文本JSON响应渲染为React JSX的组件使用。
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)  — 发出包含富文本的GraphQL请求的示例组件。 组件使用 `mapJsonRichText` 用于呈现富文本和任何引用的实用程序。


## 向富文本添加内嵌引用 {#insert-fragment-references}

多行字段允许作者在富文本流中插入来自AEM Assets的图像或其他数字资产。

![插入图像](assets/rich-text/insert-image.png)

上面的屏幕截图描述了使用 **插入资产** 按钮。

对其他内容片段的引用也可以使用链接或插入到多行字段中 **插入内容片段** 按钮。

![插入内容片段引用](assets/rich-text/insert-contentfragment.png)

上面的屏幕截图描述了另一个内容片段，即LA Skate Parks的Ultimate指南，该指南将插入到多行字段中。 可插入字段的内容片段类型由 **允许的内容片段模型** 配置 [多行数据类型](#multi-line-data-type) 在内容片段模型中。

## 使用GraphQL查询内嵌引用

GraphQL API允许开发人员创建一个查询，该查询包含有关在多行字段中插入的任何引用的其他属性。 JSON响应包含一个单独的 `_references` 列出这些额外属性的对象。 JSON响应使开发人员完全可以控制如何渲染引用或链接，而不必处理有意见的HTML。

例如，您可能希望：

* 包括自定义路由逻辑，用于在实施单页应用程序时管理指向其他内容片段的链接，如使用React Router或Next.js
* 使用AEM发布环境的绝对路径作为 `src` 值。
* 确定如何使用其他自定义属性呈现对其他内容片段的嵌入引用。

使用 `json` 返回类型并包含 `_references` 对象：

**GraphQL查询：**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _path
        _publishUrl
        width
      }
      ...on ArticleModel {
        _path
        author
      }
      
    }
  }
}
```

在上述查询中， `main` 字段，则会返回JSON格式。 的 `_references` 对象包含用于处理任何类型引用的片段 `ImageRef` 或类型 `ArticleModel`.

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
          "_path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "_publishUrl": "http://localhost:4503/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "width": 1920
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
        }
      ]
    }
  }
}
```

JSON响应包括将引用插入富文本中的位置，其中包含 `"nodeType": "reference"`. 的 `_references` 然后，对象包含每个引用以及请求的其他属性。 例如， `ImageRef` 返回 `width` 文章中引用的图像。

## 以富文本呈现内嵌引用

要渲染内联引用，请参阅 [呈现多行JSON响应](#render-multiline-json-richtext) 可以展开。

其中 `nodeMap` 是呈现JSON节点的映射。

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if(node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if(node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

高级方法是在 `nodeType` 等于 `reference` 在多行JSON响应中。 随后可以调用自定义呈现函数，该函数包含 `_references` 在GraphQL响应中返回的对象。

然后，可以将内嵌参考路径与 `_references` 对象和另一个自定义映射 `renderReference` 的次数。

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._path} alt={'in-line reference'} /> 
    },
    'AdventureModel': (node) => {
        // when __typename === AdventureModel
        return <Link to={`/adventure:${node._path}`}>{`${node.adventureTitle}: ${node.adventurePrice}`}</Link>;
    }
    ...
}
```

的 `__typename` 的 `_references` 对象可用于将不同的引用类型映射到不同的渲染函数。

### 完整代码示例

有关编写自定义引用渲染器的完整示例，请参阅 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) 作为 [WKND GraphQL React示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## 端到端示例

>[!VIDEO](https://video.tv.adobe.com/v/342105/?quality=12&learn=on)

以上视频演示了一个端到端示例：

1. 更新内容片段模型的多行文本字段以允许片段引用
1. 使用内容片段编辑器在多行文本字段中包含图像和对其他片段的引用。
1. 创建GraphQL查询，该查询包含JSON形式的多行文本响应以及任何 `_references` 已使用。
1. 编写可呈现富文本响应的内嵌引用的React SPA。
