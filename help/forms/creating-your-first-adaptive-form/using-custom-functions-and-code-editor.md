---
title: 使用函数和代码编辑器
description: 使用函数和代码编辑器来创作业务规则
feature: 自适应表单
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 2%

---


# 使用自定义函数和代码编辑器 {#using-functions-and-code-editor}

在本部分中，我们将使用自定义函数和代码编辑器来创作业务规则。

在本教程的前面，您已经安装了具有自定义函数](assets/client-libs-and-logo.zip)的[ClientLib。

通常，客户端库由CSS和Javascript文件组成。 此客户端库包含可公开用于填充下拉列表值的函数的javascript文件。


## 用于填充下拉列表的函数 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### 设置面板的摘要标题 {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### 验证面板 {#validate-panels-using-rule-editor}

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

您可以在浏览器窗口中取消对第1行的注释以调试代码。

第4行 — 获取当前面板

第5行 — 验证当前面板。

第9行 — 如果没有错误，请移至下一个面板

预览表单，并测试新启用的功能。
