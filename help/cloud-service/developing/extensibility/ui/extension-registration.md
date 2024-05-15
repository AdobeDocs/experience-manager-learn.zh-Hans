---
title: AEM UI扩展注册
description: 了解如何注册AEM UI扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# 延期注册

AEM UI扩展是专门的App Builder应用程序，基于React并使用 [React频谱](https://react-spectrum.adobe.com/react-spectrum/) 用户界面框架。

要定义AEM UI扩展的显示位置和方式，扩展的App Builder应用程序中需要两个配置：应用程序路由和扩展注册。

## 应用程序路由{#app-routes}

扩展的 `App.js` 声明 [React路由器](https://reactrouter.com/en/main) 该路由包括在AEM UI中注册扩展的索引路由。

最初加载AEM UI时会调用索引路由，此路由的目标会定义扩展在控制台中的公开方式。

+ `./src/aem-ui-extension/web-src/src/components/App.js`

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

## 延期注册

`ExtensionRegistration.js` 必须立即通过扩展的索引路径加载，并充当扩展的注册点。

基于以下情况下选择的AEM UI扩展模板 [初始化App Builder应用程序扩展](./app-initialization.md)中，支持不同的扩展点。

+ [内容片段UI扩展点](./content-fragments/overview.md#extension-points)

## 有条件地包含扩展

AEM UI扩展可以执行自定义逻辑以限制该扩展所显示的AEM环境。 此检查在 `register` 在中调用 `ExtensionRegistration` 组件，如果不应显示扩展，则会立即返回。

此检查的可用上下文有限：

+ 加载扩展的AEM主机。
+ 当前用户的AEM访问令牌。

加载扩展时最常见的检查包括：

+ 使用AEM主机(`new URLSearchParams(window.location.search).get('repo')`)以确定是否应加载扩展。
   + 仅在属于特定项目的AEM环境中显示扩展（如下例所示）。
   + 仅显示特定AEM环境(AEM主机)上的扩展。
+ 使用 [Adobe I/O Runtime操作](./runtime-action.md) 对AEM进行HTTP调用，以确定当前用户是否应看到该扩展。

以下示例说明如何将扩展限制为程序中的所有环境 `p12345`.

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
