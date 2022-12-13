---
title: AEM内容片段控制台扩展模式
description: 了解如何创建AEM内容片段控制台扩展模式。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# 扩展模式窗口

![AEM内容片段扩展模式](./assets/modal/modal.png){align="center"}

AEM内容片段扩展模式提供了一种将自定义UI附加到AEM内容片段扩展的方法，无论是 [操作栏](./action-bar.md) 或 [标题菜单](./header-menu.md) 按钮。

模型是React应用程序，基于 [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/)，并可以创建扩展所需的任何自定义UI，包括但不限于：

+ 确认对话框
+ [输入表单](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [进展指标](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [结果摘要](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ 错误消息
+ ...甚至是全面、多视图的React应用程序！

## 模式路由

模式体验由App Builder React应用程序在 `web-src` 文件夹。 与任何React应用程序一样，整个体验都是使用 [React路由](https://reactrouter.com/en/main/components/routes) 呈现 [React组件](https://reactjs.org/docs/components-and-props.html).

至少需要一条路由才能生成初始模式视图。 在 [扩展注册](#extension-registration)&#39;s `onClick(..)` 函数，如下所示。


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 扩展注册

要打开模式窗口，请调用 `guestConnection.host.modal.showUrl(..)` 由扩展的 `onClick(..)` 函数。 `showUrl(..)` 将传递一个具有键/值的JavaScript对象：

+ `title` 提供向用户显示的模式的标题名称
+ `url` 是调用 [React route](#modal-routes) 负责该模式的初始视图。

当务之急是 `url` 传递到 `guestConnection.host.modal.showUrl(..)` 解析为在扩展中路由，否则该模式窗口中不显示任何内容。

查看 [标题菜单](./header-menu.md#modal) 和 [操作栏](./action-bar.md#modal) 有关如何创建模式URL的文档。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## 模态组件

扩展的每条路径， [那不是 `index` 路由](./extension-registration.md#app-routes)，映射到可在扩展的模式中呈现的React组件。

模式可以由任意数量的React路由组成，从简单的单路由模式到复杂的多路由模式。

下面说明了一个简单的单路由模式，但此模式视图可能包含可调用其他路由或行为的React链接。

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

![AEM内容片段扩展模式关闭按钮](./assets/modal/close.png){align="center"}

模型必须提供自己的密切控制。 通过调用完成此操作 `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
