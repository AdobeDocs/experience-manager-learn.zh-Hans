---
title: 如何使用AEM React Editable Components v2
description: 瞭解如何使用AEM React Editable Components v2來增強React應用程式。
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

# 如何使用AEM React Editable Components v2

AEM提供 [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components)，此元件為以Node.js為基礎的SDK，可建立React元件，並支援使用AEM SPA編輯器編輯內容元件。

+ [npm模組](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Github專案](https://github.com/adobe/aem-react-editable-components)
+ [Adobe檔案](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


如需AEM React Editable Components v2的詳細資訊和程式碼範例，請檢閱技術檔案：

+ [與AEM檔案整合](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [可編輯的元件檔案](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [協助程式檔案](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM頁面

AEM React Editable Components可搭配SPA Editor或遠端SPA React應用程式運作。 填入可編輯React元件的內容必須透過可延伸的AEM頁面公開 [SPA頁面元件](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). 對應至可編輯React元件的AEM元件必須實作AEM [元件匯出工具架構](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html)  — 例如 [AEM核心WCM元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans).


## 依赖项

確認React應用程式正在Node.js 14+上執行。

使用AEM React Editable Components v2的React應用程式的最小相依性集為： `@adobe/aem-react-editable-components`， `@adobe/aem-spa-component-mapping`、和  `@adobe/aem-spa-page-model-manager`.


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
> [AEM React核心WCM元件庫](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [AEM React Core WCM Components SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) 與AEM React可編輯元件v2不相容。

## SPA編輯器

在搭配AEM Editor型React應用程式使用SPA React可編輯元件時，AEM `ModelManager` SDK （即SDK）：

1. 從AEM擷取內容
1. 以AEM內容填入React Eoperable元件

使用初始化的ModelManager包裝React應用程式，並轉譯React應用程式。 React應用程式應包含一個例項 `<Page>` 元件匯出自 `@adobe/aem-react-editable-components`. 此 `<Page>` 元件具備邏輯，可根據動態建立React元件 `.model.json` 由AEM提供。

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

此 `<Page>` 會透過JSON傳遞作為AEM頁面的表示法 `pageModel` 提供者： `ModelManager`. 此 `<Page>` 元件會為中的物件動態建立React元件 `pageModel` 透過比對 `resourceType` ，此React元件透過以下方式將其自身註冊至資源型別： `MapTo(..)`.

## 可編輯的元件

此 `<Page>` 會透過JSON格式傳遞AEM頁面的呈現方式 `ModelManager`. 此 `<Page>` 元件接著會比對JSON中每個物件的JS物件，以動態方式建立React元件 `resourceType` React元件的值，該元件透過元件的 `MapTo(..)` 叫用。 例如，下列專案將用於例項化執行個體

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

AEM提供的上述JSON可用來動態例項化及填入可編輯的React元件。

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

## 內嵌元件

可編輯的元件可重複使用並相互嵌入。 將一個可編輯元件嵌入另一個元件時，有兩個關鍵考量事項：

1. AEM內嵌元件的JSON內容必須包含滿足內嵌元件的內容。 方法是為AEM元件建立對話方塊，收集必要資料。
1. React元件的「不可編輯」例項必須內嵌，而非包住的「可編輯」例項 `<EditableComponent>`. 原因在於，如果內嵌元件具有 `<EditableComponent>` 包裝函式，SPA編輯器會嘗試使用編輯色域（藍色暫留方塊）塗抹內部元件，而非外部內嵌元件。

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

AEM提供的上述JSON可用於動態例項化和填入可編輯的React元件，該元件嵌入另一個React元件。


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
