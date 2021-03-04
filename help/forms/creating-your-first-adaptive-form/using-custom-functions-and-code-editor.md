---
title: 使用函数和代码编辑器
seo-title: 使用函数和代码编辑器
description: 使用函数和代码编辑器创作业务规则
seo-description: 使用函数和代码编辑器创作业务规则
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: 自适应表单
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---


# 使用自定义函数和代码编辑器{#using-functions-and-code-editor}

在这部分中，我们将使用自定义函数和代码编辑器来编写业务规则。

您已经在本教程的前面安装了自定义函数](assets/client-libs-and-logo.zip)的[ClientLib。

通常，客户端库由CSS和Javascript文件组成。 此客户端库包含显示用于填充下拉列表值的函数的javascript文件。


## 填充下拉列表列表{#function-to-populate-drop-down-list}的函数

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### 设置面板{#set-the-summary-title-of-panels}的摘要标题

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### 验证面板{#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

以下是用于验证面板字段的代码

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

您可以取消行1的注释，以在浏览器窗口中调试代码。

第4行 — 获取当前面板

第5行 — 验证当前面板。

第9行 — 如果没有错误，则移至下一个面板

预览表单，并测试新启用的功能。
