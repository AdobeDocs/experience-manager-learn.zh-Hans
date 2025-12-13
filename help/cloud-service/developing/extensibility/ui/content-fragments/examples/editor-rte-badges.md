---
title: 将徽章添加到富文本编辑器(RTE)
description: 了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加徽章
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 1%

---

# 将徽章添加到富文本编辑器(RTE)

了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加徽章。

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[富文本编辑器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)是使富文本编辑器(RTE)中的文本不可编辑的扩展。 这意味着声明为此类的徽章只能完全删除，不能部分编辑。 这些徽章还支持RTE中的特殊着色，明确向内容作者指示文本是徽章，因此不可编辑。 此外，它们提供有关徽章文本含义的视觉提示。

RTE徽章最常见的使用案例是与[RTE小组件](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/)一起使用。 这允许RTE构件插入到RTE中的内容不可编辑。

通常，与构件关联的徽章用于添加具有外部系统依赖关系的动态内容，但&#x200B;_内容作者无法修改_&#x200B;插入的动态内容以保持完整性。 它们只能作为整个项目删除。

使用&#x200B;**扩展点将**&#x200B;徽章&#x200B;**添加到内容片段编辑器中的** RTE`rte`。 使用`rte`扩展点的`getBadges()`方法添加了一个或多个徽章。

此示例说明如何添加名为&#x200B;_Large Group Bookings Customer Service_&#x200B;的小部件，以在RTE内容中查找、选择并添加特定于WKND冒险的客户服务详细信息，如&#x200B;**代表姓名**&#x200B;和&#x200B;**电话号码**。 使用徽章功能，**电话号码**&#x200B;被设为&#x200B;**不可编辑**，但WKND内容作者可以编辑代表姓名。

此外，**电话号码**&#x200B;的样式不同（蓝色），这是徽章功能的额外用例。

为了简单起见，此示例使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)框架开发构件或对话框UI以及硬编码的WKND客户服务电话号码。 为了控制内容的非编辑性和不同的样式方面，在徽章定义的`#`和`prefix`属性中使用`suffix`字符。

## 扩展点

此示例扩展到扩展点`rte`以在内容片段编辑器中向RTE添加徽章。

| AEM UI已扩展 | 扩展点 |
| ------------------------ | --------------------- |
| [内容片段编辑器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [富文本编辑器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)和[富文本编辑器小组件](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 扩展示例

以下示例创建一个&#x200B;_大型群预订客户服务_&#x200B;构件。 通过在RTE中按`{`键，打开RTE构件上下文菜单。 通过从上下文菜单中选择&#x200B;_Large Group Bookings Customer Service_&#x200B;选项，将打开自定义模式。

从模式中添加所需的客户服务编号后，徽章会使&#x200B;_电话号码不可编辑_&#x200B;并将其样式设置为蓝色。

### 延期注册

映射到`ExtensionRegistration.js`路由的`index.html`是AEM扩展的入口点并定义：

+ 在`getBadges()`中使用配置属性`id`、`prefix`、`suffix`、`backgroundColor`和`textColor`定义了徽章的定义。
+ 在此示例中，`#`字符用于定义此徽章的边界，这意味着RTE中由`#`包围的任何字符串都被视为此徽章的实例。

此外，请参阅RTE小部件的关键详细信息：

+ `getWidgets()`中的构件定义具有`id`、`label`和`url`属性。
+ `url`属性值，加载模态的相对URL路径(`/index.html#/largeBookingsCustomerService`)。


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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 在`largeBookingsCustomerService`中添加`App.js`路由{#add-widgets-route}

在主React组件`App.js`中，添加`largeBookingsCustomerService`路由以呈现上述相对URL路径的UI。

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### 创建`LargeBookingsCustomerService` React组件{#create-widget-react-component}

该构件或对话框UI是使用[Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html)框架创建的。

添加客户服务详细信息时，React组件代码在电话号码变量周围使用`#`已注册的徽章字符将其转换为徽章，如`#${phoneNumber}#`，从而使它不可编辑。

以下是`LargeBookingsCustomerService`代码的关键亮点：

+ UI是使用React Spectrum组件呈现的，如[ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)、[ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)、[Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ `largeGroupCustomerServiceList`数组具有代表性名称和电话号码的硬编码映射。 在现实场景中，此数据可以从Adobe AppBuilder操作或外部系统、自主开发或基于云提供商的API网关中检索。
+ 使用`guestConnection` `useEffect`React挂接[初始化](https://react.dev/reference/react/useEffect)，并将其作为组件状态进行管理。 用于与AEM主机通信。
+ `handleCustomerServiceChange`函数获取代表性姓名和电话号码，并更新组件状态变量。
+ 使用`addCustomerServiceDetails`对象的`guestConnection`函数提供了要执行的RTE指令。 在本例中，`insertContent`说明和HTML代码段。
+ 为了使用徽章使&#x200B;**电话号码不可编辑**，在`#`变量之前和之后添加`phoneNumber`特殊字符，如`...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`。

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
