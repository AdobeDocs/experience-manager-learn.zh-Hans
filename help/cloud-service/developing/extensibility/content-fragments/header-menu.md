---
title: AEM内容片段控制台标题菜单扩展
description: 了解如何创建AEM内容片段控制台标题菜单扩展。
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
source-wordcount: '366'
ht-degree: 0%

---


# 标题菜单扩展

![标题菜单扩展](./assets/header-menu/header-menu.png){align="center"}

包含标题菜单的扩展，在AEM内容片段控制台的标题中引入一个按钮，该按钮在 __否__ 已选择内容片段。 由于标题菜单扩展按钮仅在未选择内容片段时才显示，因此它们通常不会对现有内容片段采取任何操作。 标题菜单扩展通常：

+ 使用自定义逻辑创建新的内容片段，例如创建一组通过内容引用链接的内容片段。
+ 根据一组以编程方式选择的内容片段执行操作，例如导出上周创建的所有内容片段。

## 扩展注册

`ExtensionRegistration.js` 是AEM扩展的入口点，并定义：

1. 扩展类型；在此例中，显示标题菜单按钮。
1. 扩展按钮的定义，位于 `getButton()` 函数。
1. 按钮的点击处理程序(位于 `onClick()` 函数。

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

AEM内容片段控制台标题菜单扩展可能需要：

+ 用户执行所需操作的其他输入。
+ 能够提供用户有关操作结果的详细信息。

为了支持这些要求，AEM内容片段控制台扩展允许自定义模式以作为React应用程序呈现。

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳到创建模式窗口</p>
      <p class="has-text-blackest">了解如何创建在单击标题菜单扩展按钮时显示的模式。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="了解如何构建模式">了解如何构建模式</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 无模式

有时，AEM内容片段控制台标题菜单扩展不需要与用户进行进一步的交互，例如：

+ 调用不需要用户输入的后端进程，如导入或导出。
+ 打开新网页，例如查看有关内容准则的内部文档。

在这些情况下，AEM内容片段控制台扩展不需要 [模态](#modal)，并且可以直接在标题菜单按钮的 `onClick` 处理程序。

AEM内容片段控制台扩展允许使用进度指示器在执行工作时覆盖AEM内容片段控制台，从而阻止用户执行其他操作。 使用进度指示器是可选的，但对于将同步工作的进度告知用户非常有用。

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
