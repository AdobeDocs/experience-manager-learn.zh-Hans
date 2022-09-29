---
title: 如何使用AEM React Editable Components v2
description: 了解如何使用AEM React Editable Components v2为React应用程序提供支持。
version: Cloud Service
feature: SPA Editor
topic: Headless
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 1%

---


# 如何使用AEM React Editable Components v2

AEM提供 [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components)，一个基于Node.js的SDK，允许创建React组件，该SDK支持使用AEM SPA编辑器进行上下文内组件编辑。

+ [npm模块](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Github项目](https://github.com/adobe/aem-react-editable-components)
+ [Adobe文档](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


有关AEM React Editable Components v2的更多详细信息和代码示例，请查看技术文档：

+ [与AEM文档集成](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [可编辑的组件文档](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [帮助程序文档](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM页面

AEM React可编辑的组件可与SPA Editor或远程SPA React应用程序配合使用。 填充可编辑React组件的内容，必须通过扩展 [SPA页面组件](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html). AEM组件（映射到可编辑的React组件）必须实施AEM [组件导出器框架](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html)  — 例如 [AEM核心WCM组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans).


## 依赖项

确保React应用程序在Node.js 14+上运行。

React应用程序使用AEM React可编辑组件v2的最小依赖项集为： `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`和  `@adobe/aem-spa-page-model-manager`.


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [AEM React核心WCM组件库](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [AEM React Core WCM Components SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) 与AEM React Editable Components v2不兼容。

## SPA编辑器

将AEM React可编辑组件与基于SPA编辑器的React应用程序结合使用时，AEM `ModelManager` SDK，作为SDK:

1. 从AEM检索内容
1. 使用AEM内容填充“反应可食组件”

使用初始化的ModelManager封装React应用程序，并渲染React应用程序。 React应用程序应包含 `<Page>` 从导出的组件 `@adobe/aem-react-editable-components`. 的 `<Page>` 组件具有根据 `.model.json` 由AEM提供。

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

的 `<Page>` 将作为AEM页面的表示形式JSON，通过 `pageModel` 由提供 `ModelManager`. 的 `<Page>` 组件会为 `pageModel` 通过匹配 `resourceType` 使用通过向资源类型注册自身的React组件 `MapTo(..)`.

## 可编辑的组件

的 `<Page>` 将AEM页面的表示形式作为JSON通过 `ModelManager`. 的 `<Page>` 组件，然后通过匹配JS对象的 `resourceType` 值，该组件通过组件的 `MapTo(..)` 调用。 例如，以下内容将用于实例化实例

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

AEM提供的上述JSON可用于动态实例化和填充可编辑的React组件。

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## 嵌入组件

可编辑的组件可以重复使用并相互嵌入。 在另一个中嵌入一个可编辑组件时，需注意以下两个关键事项：

1. 嵌入组件的AEM中的JSON内容必须包含满足嵌入组件要求的内容。 这是通过为收集必需数据的AEM组件创建对话框来完成的。
1. React组件的“不可编辑”实例必须被嵌入，而不是封装在 `<EditableComponent>`. 原因是，如果嵌入的组件具有 `<EditableComponent>` 包装器时，SPA编辑器会尝试使用编辑chrome（蓝色悬停框）来包装内部组件，而不是使用外部嵌入组件。

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

AEM提供的上述JSON可用于动态实例化和填充嵌入其他React组件的可编辑React组件。


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```



