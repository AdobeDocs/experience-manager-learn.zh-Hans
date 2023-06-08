---
title: AEM UI扩展模式
description: 了解如何创建AEM UI扩展模式。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# 扩展模式

![AEM UI扩展模式](./assets/modal/modal.png){align="center"}

AEM UI扩展模式提供了一种将自定义UI附加到AEM UI扩展的方法。

模式是React应用程序，基于 [React频谱](https://react-spectrum.adobe.com/react-spectrum/)，和可以创建扩展所需的任何自定义UI，包括但不限于：

+ 确认对话框
+ [输入表单](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [进度指示器](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [结果摘要](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ 错误消息
+ ...甚至是一个全面的多视图React应用程序！

## 模式路由

模式体验由下定义的App Builder React扩展应用程序定义 `web-src` 文件夹。 与任何React应用程序一样，可以使用来编排完整体验 [React路由](https://reactrouter.com/en/main/components/routes) 该渲染 [React组件](https://reactjs.org/docs/components-and-props.html).

至少需要一条路由才能生成初始模态视图。 此初始路由将在 [分机注册](#extension-registration)的 `onClick(..)` 函数，如下所示。


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 延期注册

要打开模式窗口，请调用 `guestConnection.host.modal.showUrl(..)` 是从扩展的 `onClick(..)` 函数。 `showUrl(..)` 是通过键/值传递的JavaScript对象：

+ `title` 提供向用户显示的模式模式的标题名称
+ `url` 是调用 [React路由](#modal-routes) 负责模态的初始视图。

当务之急是 `url` 传递到 `guestConnection.host.modal.showUrl(..)` 解析以在扩展中路由，否则模式中不会显示任何内容。

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## 模式组件

每条延伸路线， [那不是 `index` 路由](./extension-registration.md#app-routes)，映射到可以在扩展的模式中渲染的React组件。

一个模式可以包含任意数量的React路由，从简单的一路由模式到复杂的多路由模式。

下面说明了一个简单的一路由模式，但此模式视图可能包含调用其他路由或行为的React链接。

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## 关闭模式窗口

![AEM UI扩展模式关闭按钮](./assets/modal/close.png){align="center"}

模范必须提供自己的严密控制。 这是通过调用完成的 `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
