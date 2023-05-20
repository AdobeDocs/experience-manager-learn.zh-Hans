---
title: AEM內容片段主控台擴充功能註冊
description: 瞭解如何註冊內容片段主控台擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# 擴充功能註冊

AEM內容片段主控台擴充功能是專門的應用程式產生器應用程式，以React為基礎，並使用 [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) UI框架。

若要定義擴充功能的AEM內容片段主控台出現位置及方式，擴充功能的App Builder應用程式中需要兩個特定設定：應用程式路由和擴充功能註冊。

## 應用程式路由{#app-routes}

擴充功能的 `App.js` 宣告 [React路由器](https://reactrouter.com/en/main) 其中包括在AEM內容片段主控台中註冊擴充功能的索引路由。

索引路由會在最初載入AEM內容片段主控台時叫用，而且此路由的目標會定義擴充功能在主控台中的公開方式。

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

## 擴充功能註冊

`ExtensionRegistration.js` 必須透過擴充功能的索引路徑立即載入，並做為擴充功能的註冊點，定義：

1. 擴充功能型別； a [頁首功能表](./header-menu.md) 或 [動作列](./action-bar.md) 按鈕。
   + [頁首功能表](./header-menu.md#extension-registration) 擴充功能由 `headerMenu` 下的屬性 `methods`.
   + [動作列](./action-bar.md#extension-registration) 擴充功能由 `actionBar` 下的屬性 `methods`.
1. 擴充功能按鈕的定義，在 `getButton()` 函式。 此函式傳回包含欄位的物件：
   + `id` 是按鈕的唯一ID
   + `label` 是AEM內容片段主控台中的擴充功能按鈕標籤
   + `icon` 是AEM內容片段主控台中的擴充功能按鈕圖示。 圖示為 [React Spectrum](https://spectrum.adobe.com/page/icons/) 圖示名稱，移除空格。
1. 按鈕的點選處理常式，在 `onClick()` 函式。
   + [頁首功能表](./header-menu.md#extension-registration) 擴充功能不會將引數傳遞至點選處理常式。
   + [動作列](./action-bar.md#extension-registration) 擴充功能提供 `selections` 引數。

### 頁首功能表擴充功能

![頁首功能表擴充功能](./assets/extension-registration/header-menu.png)

未選取任何內容片段時，會顯示標題功能表擴充功能按鈕。 由於頁首功能表擴充功能不會作用於內容片段選擇，因此不會向其提供任何內容片段 `onClick()` 處理常式。

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至建立頁首功能表擴充功能</p>
      <p class="has-text-blackest">瞭解如何在AEM內容片段主控台中註冊和定義標題功能表擴充功能。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="瞭解如何建立頁首功能表擴充功能">瞭解如何建立頁首功能表擴充功能</span>
        </a>
      </div>
    </div>
  </div>
</div>

### 動作列擴充功能

![動作列擴充功能](./assets/extension-registration/action-bar.png)

選取一或多個內容片段時，就會顯示動作列擴充功能按鈕。 所選內容片段的路徑可透過以下路徑提供給擴充功能使用： `selections` 引數，在按鈕的 `onClick(..)` 處理常式。

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至建立動作列擴充功能</p>
      <p class="has-text-blackest">瞭解如何在AEM內容片段主控台中註冊和定義動作列擴充功能。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="瞭解如何建立動作列擴充功能">瞭解如何建立動作列擴充功能</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 有條件地包含擴充功能

AEM內容片段主控台擴充功能可執行自訂邏輯，以限制擴充功能出現在AEM內容片段主控台中的時間。 此檢查是在 `register` 呼叫中的 `ExtensionRegistration` 元件，如果不應該顯示擴充功能，則會立即傳回。

此檢查可用的內容有限：

+ 正在載入擴充功能的AEM主機。
+ 目前使用者的AEM存取權杖。

載入擴充功能的最常見檢查為：

+ 使用AEM主機(`new URLSearchParams(window.location.search).get('repo')`)以決定是否應載入擴充功能。
   + 只在屬於特定計劃一部分的AEM環境中顯示擴充功能（如下例所示）。
   + 僅顯示特定AEM環境(即AEM主機)上的擴充功能。
+ 使用 [Adobe I/O Runtime動作](./runtime-action.md) 對AEM進行HTTP呼叫，以判斷目前使用者是否應該看到擴充功能。

以下範例說明如何將擴充功能限制在程式中的所有環境 `p12345`.

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
