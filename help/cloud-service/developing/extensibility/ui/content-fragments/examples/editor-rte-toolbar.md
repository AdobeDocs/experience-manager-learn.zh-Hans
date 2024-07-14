---
title: 将自定义按钮添加到富文本编辑器(RTE)工具栏
description: 了解如何在AEM内容片段编辑器中向富文本编辑器(RTE)工具栏添加自定义按钮
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 将自定义按钮添加到富文本编辑器(RTE)工具栏

了解如何在AEM内容片段编辑器的富文本编辑器(RTE)工具栏中添加自定义按钮。

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

可以使用`rte`扩展点将自定义按钮添加到内容片段编辑器中的&#x200B;**RTE工具栏**。 此示例说明如何向RTE工具栏添加名为&#x200B;_添加提示_&#x200B;的自定义按钮并修改RTE中的内容。

使用`rte`扩展点的`getCustomButtons()`方法，可以将一个或多个自定义按钮添加到&#x200B;**RTE工具栏**。 还可以分别使用`getCoreButtons()`和`removeButtons)`方法添加或删除标准RTE按钮，如&#x200B;_复制、粘贴、粗体和斜体_。

此示例说明如何使用自定义&#x200B;_添加提示_&#x200B;工具栏按钮插入高亮显示的注释或提示。 高亮显示的注释或提示内容具有通过HTML元素和关联的CSS类应用的特殊格式。 使用`getCustomButtons()`的`onClick()`回调方法插入占位符内容和HTML代码。

## 扩展点

此示例扩展到扩展点`rte`以在内容片段编辑器中将自定义按钮添加到RTE工具栏。

| AEM UI已扩展 | 扩展点 |
| ------------------------ | --------------------- | 
| [内容片段编辑器](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [富文本编辑器工具栏](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## 扩展示例

以下示例在RTE工具栏中创建一个&#x200B;_添加提示_&#x200B;自定义按钮。 单击操作将占位符文本插入到RTE中的当前插入符号位置。

该代码显示了如何添加带有图标的自定义按钮并注册点击处理程序函数。

### 延期注册

`ExtensionRegistration.js`映射到index.html路由，是AEM扩展的入口点，并定义：

+ RTE工具栏按钮在`getCustomButtons()`函数中的定义具有`id, tooltip and icon`属性。
+ `onClick()`函数中按钮的点击处理程序。
+ 点击处理程序函数接收`state`对象作为参数，以获取HTML或文本格式的RTE内容。 但在本例中，并未使用它。
+ click handler函数返回指令数组。 此数组有一个具有`type`和`value`属性的对象。 要插入内容，`value`属性HTML代码段，`type`属性使用`insertContent`。 如果有用例替换内容，则用例`replaceContent`指令类型。

`insertContent`值是HTML字符串`<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`。 用于显示值的CSS类`cmp-contentfragment__element-tip`未在构件中定义，而是在显示此内容片段字段的Web体验上实施的。


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
          // RTE Toolbar custom button
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
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
