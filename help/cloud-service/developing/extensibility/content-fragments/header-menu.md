---
title: AEM內容片段控制檯標題功能表擴充功能
description: 瞭解如何建立AEM內容片段主控台標題功能表擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 92d6e98e-24d0-4229-9d30-850f6b72ab43
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# 頁首功能表副檔名

![頁首功能表副檔名](./assets/header-menu/header-menu.png){align="center"}

包含頁首功能表的擴充功能會在以下情況下顯示的AEM內容片段控制檯頁首引入按鈕： __否__ 已選取內容片段。 由於頁首功能表擴充功能按鈕僅在未選取任何內容片段時顯示，因此它們通常不會對現有的內容片段採取任何行動。 反之，標題功能表副檔名通常會：

+ 使用自訂邏輯建立新的內容片段，例如透過內容參考連結建立一組內容片段。
+ 對以程式設計方式選取的一組內容片段執行動作，例如匯出上週建立的所有內容片段。

## 擴充功能註冊

`ExtensionRegistration.js` 是AEM擴充功能的進入點，並定義：

1. 擴充功能型別；如果是，則為標題功能表按鈕。
1. 擴充功能按鈕的定義，在 `getButton()` 函式。
1. 按鈕的點選處理常式，位於 `onClick()` 函式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## 模态

![模态](./assets/modal/modal.png)

AEM內容片段控制檯標題功能表擴充功能可能需要：

+ 來自使用者的其他輸入，以執行所需的動作。
+ 能夠向使用者提供有關動作結果的詳細資訊。

為了支援這些需求，AEM內容片段主控台擴充功能允許自訂強制回應視窗，以呈現為React應用程式。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至建立強制回應視窗</p>
      <p class="has-text-blackest">瞭解如何建立按一下標題功能表擴充功能按鈕時顯示的強制回應視窗。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="瞭解如何建立強制回應視窗">瞭解如何建立強制回應視窗</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 無強制回應視窗

有時，AEM內容片段控制檯標題功能表擴充功能不需要與使用者進一步互動，例如：

+ 叫用不需要使用者輸入的後端程式，例如匯入或匯出。
+ 開啟新的網頁，例如內容指南的內部檔案。

在這些情況下，AEM內容片段主控台擴充功能不需要 [強制回應](#modal)，並可直接在頁首功能表按鈕的 `onClick` 處理常式。

AEM內容片段主控台擴充功能允許進度指示器在工作執行時覆蓋AEM內容片段主控台，封鎖使用者執行進一步動作。 進度指示器的使用是選用的，但對於將同步工作的進度傳達給使用者很有用。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
