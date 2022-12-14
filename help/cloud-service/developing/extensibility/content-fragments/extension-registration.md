---
title: AEM内容片段控制台扩展注册
description: 了解如何注册内容片段控制台扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# 扩展注册

AEM内容片段控制台扩展是基于React的专用App Builder应用程序，使用 [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) UI框架。

要定义扩展在何处以及如何显示AEM内容片段控制台，该扩展的App Builder应用程序中需要两个特定配置：应用程序路由和扩展注册。

## 应用程序路由{#app-routes}

扩展的 `App.js` 声明 [React路由器](https://reactrouter.com/en/main) 该路径包括在AEM内容片段控制台中注册扩展的索引路由。

当AEM内容片段控制台最初加载时，将调用索引路由，此路由的目标定义扩展在控制台中的显示方式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 扩展注册

`ExtensionRegistration.js` 必须立即通过扩展的索引路由加载，并执行扩展的注册点操作，定义：

1. 扩展类型；a [标题菜单](./header-menu.md) 或 [操作栏](./action-bar.md) 按钮。
   + [标题菜单](./header-menu.md#extension-registration) 扩展由 `headerMenu` 资产 `methods`.
   + [操作栏](./action-bar.md#extension-registration) 扩展由 `actionBar` 资产 `methods`.
1. 扩展按钮的定义，位于 `getButton()` 函数。 此函数返回一个包含字段的对象：
   + `id` 是按钮的唯一ID
   + `label` 是AEM内容片段控制台中扩展按钮的标签
   + `icon` 是AEM内容片段控制台中扩展按钮的图标。 图标是 [React Spectrum](https://spectrum.adobe.com/page/icons/) 图标名称，并删除空格。
1. 在 `onClick()` 函数。
   + [标题菜单](./header-menu.md#extension-registration) 扩展不会将参数传递到点击处理程序。
   + [操作栏](./action-bar.md#extension-registration) 扩展提供了 `selections` 参数。

### 标题菜单扩展

![标题菜单扩展](./assets/extension-registration/header-menu.png)

未选择内容片段时，将显示“标题菜单”扩展按钮。 由于标题菜单扩展不会对内容片段选择产生任何作用，因此不会向其提供内容片段 `onClick()` 处理程序。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至构建标题菜单扩展</p>
      <p class="has-text-blackest">了解如何在AEM内容片段控制台中注册和定义标题菜单扩展。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="了解如何构建标题菜单扩展">了解如何构建标题菜单扩展</span>
        </a>
      </div>
    </div>
  </div>
</div>

### 操作栏扩展

![操作栏扩展](./assets/extension-registration/action-bar.png)

选择一个或多个内容片段时，将显示操作栏扩展按钮。 所选内容片段的路径可通过 `selections` 参数，在按钮的 `onClick(..)` 处理程序。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至构建操作栏扩展</p>
      <p class="has-text-blackest">了解如何在AEM内容片段控制台中注册和定义操作栏扩展。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="了解如何构建操作栏扩展">了解如何构建操作栏扩展</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 有条件地包含扩展

AEM内容片段控制台扩展可以执行自定义逻辑以限制该扩展在AEM内容片段控制台中显示的时间。 此检查在 `register` 在中调用 `ExtensionRegistration` 组件中，如果不显示扩展，则会立即返回。

此检查的上下文有限：

+ 加载扩展的AEM主机。
+ 当前用户的AEM访问令牌。

加载扩展的最常见检查包括：

+ 使用AEM主机(`new URLSearchParams(window.location.search).get('repo')`)来确定是否应加载扩展。
   + 仅在AEM环境中属于特定程序一部分的扩展（如以下示例所示）。
   + 仅在特定AEM环境(即AEM主机)上显示扩展。
+ 使用 [Adobe I/O Runtime行动](./runtime-action.md) 对AEM进行HTTP调用，以确定当前用户是否应看到扩展。

以下示例说明了将扩展限制在项目中的所有环境 `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
