---
title: 将构件添加到富文本编辑器(RTE)
description: 了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加构件
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 476
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# 将构件添加到富文本编辑器(RTE)

了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加构件。

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

要在富文本编辑器(RTE)中添加动态内容，可以使用&#x200B;**小组件**&#x200B;功能。 这些小组件有助于在RTE中集成简单或复杂的UI，并且可以使用您选择的JS框架创建UI。 可将其视为通过在RTE中按`{`特殊键打开的对话框。

通常，小组件用于插入具有外部系统依赖关系或可以根据当前上下文更改的动态内容。

使用&#x200B;**扩展点将**&#x200B;构件&#x200B;**添加到内容片段编辑器中的** RTE`rte`。 使用`rte`扩展点的`getWidgets()`方法添加了一个或多个小组件。 它们是通过按`{`特殊键打开上下文菜单选项来触发的，然后选择所需的构件以加载自定义对话框UI。

此示例说明如何添加名为&#x200B;_折扣代码列表_&#x200B;的小组件以在RTE内容中查找、选择和添加特定于WKND冒险的折扣代码。 这些折扣代码可以在外部系统中管理，如Order Management System (OMS)、产品信息管理(PIM)、自主开发的应用程序或Adobe AppBuilder操作。

为了简单起见，此示例使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)框架开发构件或对话框UI以及硬编码的WKND冒险名称、折扣代码数据。

## 扩展点

此示例扩展到扩展点`rte`，在内容片段编辑器中将小组件添加到RTE。

| AEM UI已扩展 | 扩展点 |
| ------------------------ | --------------------- |
| [内容片段编辑器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [富文本编辑器小组件](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 扩展示例

以下示例创建&#x200B;_折扣代码列表_&#x200B;构件。 通过按RTE中的`{`特殊键，将打开上下文菜单，然后从上下文菜单中选择&#x200B;_折扣代码列表_&#x200B;选项将打开对话框UI。

WKND内容作者可以查找、选择和添加当前特定于冒险的折扣代码（如果可用）。

### 延期注册

`ExtensionRegistration.js`映射到index.html路由，是AEM扩展的入口点，并定义：

+ 具有`getWidgets()`特性的`id, label and url`函数中的构件定义。
+ `url`属性值，加载对话框UI的相对URL路径(`/index.html#/discountCodes`)。

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

### 在`discountCodes`中添加`App.js`路由{#add-widgets-route}

在主React组件`App.js`中，添加`discountCodes`路由以呈现上述相对URL路径的UI。

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

### 创建`DiscountCodes` React组件{#create-widget-react-component}

该构件或对话框UI是使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)框架创建的。 `DiscountCodes`组件代码如下所示，以下是主要亮点：

+ UI是使用React Spectrum组件呈现的，如[ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)、[ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)、[Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ `adventureDiscountCodes`数组具有冒险名称和折扣代码的硬编码映射。 在现实场景中，此数据可以从Adobe AppBuilder操作或PIM、OMS等外部系统或自主开发的或基于云提供商的API网关中检索。
+ 使用`guestConnection` `useEffect`React挂接[初始化](https://react.dev/reference/react/useEffect)，并将其作为组件状态进行管理。 用于与AEM主机通信。
+ `handleDiscountCodeChange`函数获取所选冒险名称的折扣代码并更新状态变量。
+ 使用`addDiscountCode`对象的`guestConnection`函数提供了要执行的RTE指令。 在本例中，`insertContent`指令和实际折扣代码的HTML代码段将插入到RTE中。

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
