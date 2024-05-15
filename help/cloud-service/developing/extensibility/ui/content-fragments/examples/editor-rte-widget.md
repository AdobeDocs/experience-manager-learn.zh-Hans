---
title: 将构件添加到富文本编辑器(RTE)
description: 了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加构件
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 476
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# 将构件添加到富文本编辑器(RTE)

了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加构件。

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

要在富文本编辑器(RTE)中添加动态内容，请 **构件** 功能可用。 这些小组件有助于在RTE中集成简单或复杂的UI，并且可以使用您选择的JS框架创建UI。 它们可以视为通过按打开的对话框 `{` RTE中的特殊键。

通常，小组件用于插入具有外部系统依赖关系或可以根据当前上下文更改的动态内容。

此 **构件** 都会添加到 **RTE** 在内容片段编辑器中使用 `rte` 扩展点。 使用 `rte` 扩展点的 `getWidgets()` 方法添加一个或多个构件。 它们通过按下 `{` 用于打开上下文菜单选项的特殊键，然后选择所需的构件以加载自定义对话框UI。

此示例说明如何添加名为的小部件 _折扣代码列表_ 在RTE内容中查找、选择并添加WKND冒险特定折扣代码。 这些折扣代码可以在外部系统中进行管理，如订单管理系统(OMS)、产品信息管理(PIM)、自主开发的应用程序或AdobeAppBuilder操作。

为了保持简单，本示例使用 [AdobeReact频谱](https://react-spectrum.adobe.com/react-spectrum/index.html) 框架来开发构件或对话框UI和硬编码的WKND冒险名称、折扣代码数据。

## 扩展点

此示例将扩展到扩展点 `rte` 在内容片段编辑器中将小组件添加到RTE。

| AEM UI已扩展 | 扩展点 |
| ------------------------ | --------------------- | 
| [内容片段编辑器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [富文本编辑器小组件](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 扩展示例

以下示例创建 _折扣代码列表_ 构件。 通过按下 `{` 在RTE中打开上下文菜单，然后通过选择 _折扣代码列表_ 选项，这将打开对话框UI。

WKND内容作者可以查找、选择和添加当前特定于冒险的折扣代码（如果可用）。

### 延期注册

`ExtensionRegistration.js`，映射到index.html路由，是AEM扩展的入口点，并定义：

+ 中的构件定义 `getWidgets()` 函数为 `id, label and url` 属性。
+ 此 `url` 属性值，相对URL路径(`/index.html#/discountCodes`)以加载对话框UI。

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {
          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget", // Provide a unique ID for the widget
              label: "Discount Code List", // Provide a label for the widget
              url: "/index.html#/discountCodes", // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
        }, // Add a comma here
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 添加 `discountCodes` 路由 `App.js`{#add-widgets-route}

在主React组件中 `App.js`，添加 `discountCodes` 用于呈现上述相对URL路径的UI的路由。

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### 创建 `DiscountCodes` React组件{#create-widget-react-component}

使用创建构件或对话框UI [AdobeReact频谱](https://react-spectrum.adobe.com/react-spectrum/index.html) 框架。 此 `DiscountCodes` 组件代码如下所示，以下是主要亮点：

+ UI使用React Spectrum组件渲染，例如 [组合框](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)， [按钮组](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)， [按钮](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ 此 `adventureDiscountCodes` 数组具有冒险名称和折扣代码的硬编码映射。 在真实场景中，此数据可以从AdobeAppBuilder操作或外部系统（如PIM、OMS或自主开发的或基于云提供商的API网关）中检索。
+ 此 `guestConnection` 已使用 `useEffect` [React挂钩](https://react.dev/reference/react/useEffect) 并作为组件状态进行管理。 用于与AEM主机通信。
+ 此 `handleDiscountCodeChange` 函数获取所选冒险名称的折扣代码并更新状态变量。
+ 此 `addDiscountCode` 函数使用 `guestConnection` 对象提供要执行的RTE指令。 在本例中 `insertContent` 要插入RTE中的实际折扣代码的说明和HTML代码段。

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
