---
title: 使用函数和代码编辑器
description: 使用函数和代码编辑器创作业务规则
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
duration: 460
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 使用自定义函数和代码编辑器 {#using-functions-and-code-editor}

在本部分中，我们将使用自定义函数和代码编辑器来创作业务规则。

您已安装 [具有自定义函数的ClientLib](assets/client-libs-and-logo.zip) 在本教程的前面部分。

通常，客户端库包含CSS和Javascript文件。 此客户端库包含javascript文件，该文件公开了用于填充下拉列表值的函数。


## 用于填充下拉列表的函数 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### 设置面板的摘要标题 {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### 验证面板 {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

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

您可以取消注释第1行以在浏览器窗口中调试代码。

第4行 — 获取当前面板

第5行 — 验证当前面板。

第9行 — 如果没有错误，则移至下一个面板

预览表单，并测试新启用的功能。
