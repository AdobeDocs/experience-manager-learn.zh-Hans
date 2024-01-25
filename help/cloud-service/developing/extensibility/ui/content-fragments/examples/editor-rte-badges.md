---
title: 将徽章添加到富文本编辑器(RTE)
description: 了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加徽章
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 614
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# 将徽章添加到富文本编辑器(RTE)

了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)添加徽章。

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[富文本编辑器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)  是使富文本编辑器(RTE)中的文本不可编辑的扩展。 这意味着声明为此类的徽章只能完全删除，不能部分编辑。 这些徽章还支持RTE中的特殊着色，明确向内容作者指示文本是徽章，因此不可编辑。 此外，它们提供有关徽章文本含义的视觉提示。

RTE徽章最常见的使用案例是将其与结合使用 [RTE小组件](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). 这允许RTE构件插入到RTE中的内容不可编辑。

通常，与小组件关联的徽章用于添加具有外部系统依赖关系，但 _内容作者无法修改_ 插入的动态内容以保持完整性。 它们只能作为整个项目删除。

此 **徽章** 都会添加到 **RTE** 在内容片段编辑器中使用 `rte` 扩展点。 使用 `rte` 扩展点的 `getBadges()` 方法添加一个或多个徽章。

此示例说明如何添加名为的小部件 _大型团体预订客户服务_ 查找、选择并添加特定于WKND冒险的客户服务详细信息，例如 **代表姓名** 和 **电话号码** 在RTE内容中。 使用徽章功能 **电话号码** 已制作 **不可编辑** 但WKND内容作者可以编辑代表名称。

此外， **电话号码** 样式不同（蓝色），这是徽章功能的额外用例。

为了保持简单，本示例使用 [AdobeReact频谱](https://react-spectrum.adobe.com/react-spectrum/index.html) 框架，用于开发小组件或对话框UI和硬编码的WKND客户服务电话号码。 要控制内容的非编辑和不同的样式方面，请 `#` 字符用于 `prefix` 和 `suffix` 徽章定义的属性。

## 扩展点

此示例将扩展到扩展点 `rte` 在内容片段编辑器中将徽章添加到RTE。

| AEM UI已扩展 | 扩展点 |
| ------------------------ | --------------------- | 
| [内容片段编辑器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [富文本编辑器徽章](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) 和 [富文本编辑器小组件](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 扩展示例

以下示例创建 _大型团体预订客户服务_ 构件。 通过按下 `{` 键，打开RTE构件上下文菜单。 通过选择 _大型团体预订客户服务_ 上下文菜单中的选项，自定义模式将打开。

从模式中添加所需的客户服务编号后，徽章会使 _电话号码不可编辑_ 并将其样式设置为蓝色。

### 延期注册

`ExtensionRegistration.js`，映射到 `index.html` route，是AEM扩展的入口点，并定义：

+ 在中定义徽章的定义 `getBadges()` 使用配置属性 `id`， `prefix`， `suffix`， `backgroundColor` 和 `textColor`.
+ 在此示例中， `#` 字符用于定义此徽章的边界，这意味着RTE中由以下对象包围的任何字符串 `#` 将被视为此徽章的实例。

此外，请参阅RTE小部件的关键详细信息：

+ 中的构件定义 `getWidgets()` 函数为 `id`， `label` 和 `url` 属性。
+ 此 `url` 属性值，相对URL路径(`/index.html#/largeBookingsCustomerService`)以加载该模式窗口。


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
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
              id: "phoneNumber",                    // Provide a unique ID for the badge
              prefix: "#",                          // Provide a Badge starting character
              suffix: "#",                          // Provide a Badge ending character
              backgroundColor: "",                  // Provide HEX or text CSS color code for the background
              textColor: "#071DF8"                  // Provide HEX or text CSS color code for the text
            }
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",       // Provide a unique ID for the widget
              label: "Large Group Bookings Customer Service",          // Provide a label for the widget
              url: "/index.html#/largeBookingsCustomerService",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/largeBookingsCustomerService`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### 添加 `largeBookingsCustomerService` 路由 `App.js`{#add-widgets-route}

在主React组件中 `App.js`，添加 `largeBookingsCustomerService` 用于呈现上述相对URL路径的UI的路由。

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

### 创建 `LargeBookingsCustomerService` React组件{#create-widget-react-component}

使用创建构件或对话框UI [AdobeReact频谱](https://react-spectrum.adobe.com/react-spectrum/index.html) 框架。

添加客户服务详细信息时，React组件代码将在电话号码变量周围加上 `#` 注册徽章字符，以将其转换为徽章，如 `#${phoneNumber}#`，因此使其不可编辑。

以下是的主要亮点 `LargeBookingsCustomerService` 代码：

+ UI使用React Spectrum组件渲染，例如 [组合框](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html)， [按钮组](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html)， [按钮](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ 此 `largeGroupCustomerServiceList` 数组具有代表性名称和电话号码的硬编码映射。 在现实场景中，此数据可以从AdobeAppBuilder操作或外部系统、自主开发或基于云提供商的API网关中检索。
+ 此 `guestConnection` 已使用 `useEffect` [React挂钩](https://react.dev/reference/react/useEffect) 并作为组件状态进行管理。 用于与AEM主机通信。
+ 此 `handleCustomerServiceChange` 函数获取代表性姓名和电话号码，并更新组件状态变量。
+ 此 `addCustomerServiceDetails` 函数使用 `guestConnection` 对象提供要执行的RTE指令。 在本例中 `insertContent` 指令和HTML代码段。
+ 创建 **电话号码不可编辑** 使用徽章， `#` 特殊字符添加在 `phoneNumber` 变量，如 `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

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
