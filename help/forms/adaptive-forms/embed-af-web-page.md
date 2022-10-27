---
title: 在网页中嵌入自适应Forms/HTML5表单
description: 将自适应Forms或HTML5表单嵌入到非AEM网页中所需的配置步骤。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# 在网页中嵌入自适应表单或HTML5表单

嵌入式自适应表单功能齐全，用户无需离开页面即可填写和提交表单。 它有助于用户停留在网页上其他元素的上下文中，并同时与表单进行交互。

以下视频介绍在网页中嵌入自适应或HTML5表单所需的步骤。
请参阅 [文档](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) 以了解最佳先决条件、最佳实践等。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

您可以下载视频中使用的示例文件 [从此处](assets/embedding-af-web-page.zip)

以下是用于获取自适应表单并将表单嵌入到由类名称标识的容器中的代码 **右**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
