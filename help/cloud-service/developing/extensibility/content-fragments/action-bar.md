---
title: AEM内容片段控制台操作栏扩展
description: 了解如何创建AEM内容片段控制台操作栏扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 97d26a1f-f9a7-4e57-a5ef-8bb2f3611088
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 操作栏扩展

![操作栏扩展](./assets/action-bar/action-bar.png){align="center"}

扩展包含操作栏，在AEM内容片段控制台的操作中引入一个按钮，该控制台在以下情况下显示： __1个或更多__ 已选择内容片段。 由于操作栏扩展按钮仅在选择至少一个内容片段时显示，因此它们通常会对选定的内容片段执行操作。 示例包括：

+ 对选定的内容片段调用业务流程或工作流。
+ 更新或更改所选内容片段的数据。

## 延期注册

`ExtensionRegistration.js` 是AEM扩展的入口点，并定义：

1. 扩展类型；在此例中是操作栏按钮。
1. 扩展按钮的定义，在 `getButton()` 函数。
1. 按钮的点击处理程序，位于 `onClick()` 函数。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## 模态

![模态](./assets/modal/modal.png)

AEM内容片段控制台操作栏扩展可能需要：

+ 来自用户的附加输入，用于执行所需的操作。
+ 能够向用户提供有关操作结果的详细信息这一功能。

为了支持这些要求，AEM内容片段控制台扩展允许呈现为React应用程序的自定义模态。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">跳至创建模式窗口</p>
      <p class="has-text-blackest">了解如何创建在单击操作栏扩展按钮时显示的模式。</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="了解如何构建模态">了解如何构建模态</span>
        </a>
      </div>
    </div>
  </div>
</div>

## 无模态

有时，AEM内容片段控制台操作栏扩展不需要与用户进一步交互，例如：

+ 调用不需要用户输入的后端进程，例如导入或导出。

在这些情况下，AEM内容片段控制台扩展不需要 [模态](#modal)，并直接在操作栏按钮的 `onClick` 处理程序。

AEM内容片段控制台扩展允许进度指示器在执行工作时叠加AEM内容片段控制台，从而阻止用户执行进一步操作。 进度指示器的使用是可选的，但可用于将同步工作的进度告知用户。

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
