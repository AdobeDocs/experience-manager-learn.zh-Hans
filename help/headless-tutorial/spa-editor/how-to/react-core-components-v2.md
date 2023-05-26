---
title: 如何使用AEM React可编辑组件v2
description: 了解如何使用AEM React可编辑组件v2来支持React应用程序。
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---

# 如何使用AEM React可编辑组件v2

AEM提供 [AEM React可编辑组件v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components)，这是一个基于Node.js的SDK，允许创建React组件，并支持使用AEM SPA编辑器在上下文中编辑组件。

+ [npm模块](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Github项目](https://github.com/adobe/aem-react-editable-components)
+ [Adobe文档](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


有关AEM React可编辑组件v2的更多详细信息和代码示例，请查看技术文档：

+ [与AEM集成文档](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [可编辑组件文档](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [帮助程序文档](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM页面

AEM React可编辑组件可与SPA编辑器或Remote SPA React应用程序一起使用。 AEM填充可编辑React组件的内容必须通过扩展 [SPA页面组件](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). 映射到可编辑React组件的AEM组件必须实施AEM [组件导出器框架](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html)  — 例如 [AEM核心WCM组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans).


## 依赖项

确保React应用程序在Node.js 14及更高版本上运行。

React应用程序使用AEM React Editable Components v2所需的最低依赖项集包括： `@adobe/aem-react-editable-components`， `@adobe/aem-spa-component-mapping`、和  `@adobe/aem-spa-page-model-manager`.


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
> [AEM React核心WCM组件库](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [AEM React核心WCM组件SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) 与AEM React可编辑组件v2不兼容。

## SPA编辑器

在基于AEM编辑器的React应用程序中使用SPA React可编辑组件时，AEM `ModelManager` SDK（即SDK）：

1. 从AEM检索内容
1. 用AEM内容填充React可食用组件

使用初始化的ModelManager封装React应用程序，并渲染React应用程序。 React应用程序应包含一个实例 `<Page>` 组件导出自 `@adobe/aem-react-editable-components`. 此 `<Page>` 组件具有逻辑，可根据 `.model.json` 由AEM提供。

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

此 `<Page>` 作为AEM页面的表示形式通过 `pageModel` 由 `ModelManager`. 此 `<Page>` 组件为中的对象动态创建React组件 `pageModel` 通过匹配 `resourceType` 通过React组件向资源类型注册自身 `MapTo(..)`.

## 可编辑的组件

此 `<Page>` 通过JSON格式传递AEM页面的表示形式 `ModelManager`. 此 `<Page>` 组件然后通过匹配JS对象的，为JSON中的每个对象动态创建React组件 `resourceType` 值以及一个React组件，该组件通过组件的 `MapTo(..)` 调用。 例如，以下内容将用于实例化实例

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

可编辑的组件可以重复使用并相互嵌入。 将一个可编辑组件嵌入另一个可编辑组件时，有两个关键注意事项：

1. AEM中用于嵌入组件的JSON内容必须包含满足嵌入组件的内容。 这是通过为AEM组件创建一个收集所需数据的对话框来完成的。
1. 必须嵌入React组件的“不可编辑”实例，而不是使用封装的“可编辑”实例 `<EditableComponent>`. 原因是，如果嵌入的组件具有 `<EditableComponent>` 包装时，SPA编辑器会尝试使用编辑chrome（蓝色悬停框）而不是外部嵌入组件来修饰内部组件。

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
