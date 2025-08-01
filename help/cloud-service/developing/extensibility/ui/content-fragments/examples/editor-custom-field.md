---
title: 内容片段控制台 — 自定义字段
description: 了解如何在AEM内容片段编辑器中创建自定义字段。
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 315
last-substantial-update: 2024-02-27T00:00:00Z
jira: KT-14903
thumbnail: KT-14903.jpeg
exl-id: 563bab0e-21e3-487c-9bf3-de15c3a81aba
source-git-commit: be710750cd6f2c71fa6b5eb8a6309d5965d67c82
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 1%

---

# 自定义字段

了解如何在AEM内容片段编辑器中创建自定义字段。

>[!VIDEO](https://video.tv.adobe.com/v/3437644?learn=on&captions=chi_hans)

AEM UI扩展应使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)框架进行开发，因为这与AEM的其他部分保持一致的外观，并且还有大量预建功能，从而缩短了开发时间。

## 扩展点

此示例将内容片段编辑器中的现有字段替换为自定义实施。

| AEM UI已扩展 | 扩展点 |
| ------------------------ | --------------------- | 
| [内容片段编辑器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [自定义表单元素呈现](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/) |

## 扩展示例

此示例演示了通过将标准字段替换为预定义SKU的自定义下拉列表，将内容片段编辑器中的字段值限制为预定的集。 作者可以从此特定的SKU列表中选择。 虽然SKU通常来自产品信息管理(PIM)系统，但此示例可通过静态列出SKU来简化。

此示例的源代码为[可供下载](./assets/editor-custom-field/content-fragment-editor-custom-field-src.zip)。

### 内容片段模型定义

此示例绑定到名称为`sku`的任何内容片段字段（通过[的](#extension-registration)正则表达式匹配`^sku$`），并将其替换为自定义字段。 此示例使用已更新的WKND冒险内容片段模型，其定义如下：

![内容片段模型定义](./assets/editor-custom-field/content-fragment-editor.png)

尽管自定义SKU字段显示为下拉列表，但其基础模型配置为文本字段。 自定义字段实施只需要与相应的属性名称和类型保持一致，从而便于将标准字段替换为自定义下拉版本。


### 应用程序路由

在主React组件`App.js`中，包含用于呈现`/sku-field` React组件的`SkuField`路由。

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
import React from "react";
import ErrorBoundary from "react-error-boundary";
import { HashRouter as Router, Routes, Route } from "react-router-dom";
import ExtensionRegistration from "./ExtensionRegistration";
import SkuField from "./SkuField"; // Custom component implemented below

function App() {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          <Route index element={<ExtensionRegistration />} />
          <Route
            exact path="index.html"
            element={<ExtensionRegistration />}
          />
          {/* This is the React route that maps to the custom field */}
          <Route
            exact path="/sku-field"
            element={<SkuField />}/>
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
...
```

此映射到`/sku-field`组件的`SkuField`自定义路由在[扩展注册](#extension-registration)下使用。

### 延期注册

`ExtensionRegistration.js`映射到index.html路由，是AEM扩展的入口点，并定义：

+ `getDefinitions()`中的构件定义具有`fieldNameExp`和`url`属性。 [自定义表单元素渲染API引用](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference)中提供了可用属性的完整列表。
+ `url`属性值，用于加载字段UI的相对URL路径(`/index.html#/skuField`)。

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        field: {
          getDefinitions() {
            return [
              // Multiple fields can be registered here.
              {
                fieldNameExp: '^sku$',  // This is a regular expression that matches the field name in the Content Fragment Model to replace with React component specified in the `url` key.
                url: '/#/sku-field',    // The URL, which is mapped vai the Route in App.js, to the React component that will be used to render the field.
              },
              // Other bindings besides fieldNameExp, other bindings can be used as well as defined here:
              // https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference
            ];
          },
        },
      }
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 自定义字段

`SkuField` React组件使用自定义UI更新内容片段编辑器，并使用其选取器表单的Adobe React Spectrum。 重点包括：

+ 利用`useEffect`进行初始化并连接到AEM的内容片段编辑器，在安装完成之前显示加载状态。
+ 在iFrame中渲染时，它会通过`onOpenChange`函数动态调整iFrame的高度，以适应Adobe React光谱选取器的下拉列表。
+ 使用`connection.host.field.onChange(value)`函数中的`onSelectionChange`将字段选择传回主机，确保根据内容片段模型的指南验证和自动保存所选值。

自定义字段在注入到内容片段编辑器中的iFrame中渲染。 自定义字段代码和内容片段编辑器之间的通信仅通过`connection`对象进行，该对象由`attach`包中的`@adobe/uix-guest`函数建立。

`src/aem-cf-editor-1/web-src/src/components/SkuField.js`

```javascript
import React, { useEffect, useState } from "react";
import { extensionId } from "./Constants";
import { attach } from "@adobe/uix-guest";
import { Provider, View, lightTheme } from "@adobe/react-spectrum";
import { Picker, Item } from "@adobe/react-spectrum";

const SkuField = (props) => {
  const [connection, setConnection] = useState(null);
  const [validationState, setValidationState] = useState(null);
  const [value, setValue] = useState(null);
  const [model, setModel] = useState(null);
  const [items, setItems] = useState(null);

  /**
   * Mock function that gets a list of Adventure SKUs to display.
   * The data could come anywhere, AEM's HTTP APIs, a PIM, or other system.
   * @returns a list of items for the picker
   */
  async function getItems() {
    // Data to drive input field validation can come from anywhere.
    // Fo example this commented code shows how it could be fetched from an HTTP API.
    // fetch(MY_API_URL).then((response) => response.json()).then((data) => { return data; }

    // In this example, for simplicity, we generate a list of 25 SKUs.
    return Array.from({ length: 25 }, (_, i) => ({ 
        name: `WKND-${String(i + 1).padStart(3, '0')}`, 
        id: `WKND-${String(i + 1).padStart(3, '0')}` 
    }));
  }

  /**
   * When the fields changes, update the value in the Content Fragment Editor
   * @param {*} value the selected value in the picker
   */
  const onSelectionChange = async (value) => {
    // This sets the value in the React state of the custom field
    setValue(value);
    // This calls the setValue method on the host (AEM's Content Fragment Editor)
    connection.host.field.onChange(value);
  };

  /**
   * Some widgets, like the Picker, have a variable height.
   * In these cases adjust the Content Fragment Editor's iframe's height so the field doesn't get cut off.     *
   * @param {*} isOpen true if the picker is open, false if it's closed
   */
  const onOpenChange = (isOpen) => {
    if (isOpen) {
      // Calculate the height of the picker box and its label, and surrounding padding.
      const pickerHeight = Number(document.body.clientHeight.toFixed(0));
      // Calculate the height of the picker options dropdown, and surrounding padding.
      // We do this  by multiplying the number of items by the height of each item (32px) and adding 12px for padding.
      const optionsHeight = items.length * 32 + 12;

      // Set the height of the iframe to the height of the picker + the height of the options, or 400px, whichever is smaller.
      // The options will scroll if they they cannot fit into 400px
      const height = Math.min(pickerHeight + optionsHeight, 400);

      // Set the height of the iframe in the Content Fragment Editor
      connection.host.field.setStyles({
        current: { height, },
        parent: { height, },
      })
    } else {
      connection.host.field.setStyles({
        current: { height: 74 },
        parent: { height: 74 },
      })
    }
  };

  useEffect(() => {
    const init = async () => {
      // Connect to the host (AEM's Content Fragment Editor)
      const conn = await attach({ id: extensionId });
      setConnection(conn);

      // get the Content Fragment Model
      setModel(await conn.host.field.getModel());

      // Share the validation state back to the client.
      // When conn.host.field.setValue(value) is called, the
      await conn.host.field.onValidationStateChange((state) => {
        // state can be `valid` or `invalid`.
        setValidationState(state);
      });
      // Get default value from the Content Fragment Editor
      // (either the default value set in the model, or a perviously set value)
      setValue(await conn.host.field.getDefaultValue());

      // Get the list of items for the picker; in this case its a list of adventure SKUs 
      // This could come from elsewhere in AEM or from an external system.
      setItems(await getItems());
    };

    init().catch(console.error);
  }, []);

  // If the component is not yet initialized, return a loading state.
  if (!connection || !model || !items) {
    // Put whatever loader you like here...
    return <Provider theme={lightTheme}>Loading custom field...</Provider>;
  }

  // Wrap the Spectrum UI component in a Provider theme, such that it is styled appropriately.
  // Render the picker, and bind to the data and custom event handlers.

  // Set as much of the model as we can, to allow maximum authoring flexibility without developer support.
  return (
    <Provider theme={lightTheme}>
      <View width="100%">
        <Picker
          label={model.fieldLabel}
          isRequired={model.required}
          placeholder={model.emptyText}
          errorMessage={model.customErrorMsg}
          selectedKey={value}
          necessityIndicator="icon"
          shouldFlip={false}
          width={"90%"}
          items={items}
          isInvalid={validationState === "invalid"}
          onSelectionChange={onSelectionChange}
          onOpenChange={onOpenChange}
        >
          {(item) => <Item key={item.value}>{item.name}</Item>}
        </Picker>
      </View>
    </Provider>
  );
};

export default SkuField;
```
