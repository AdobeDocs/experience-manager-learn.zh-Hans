---
title: 在网页中嵌入自适应Forms/HTML5表单
description: 将自适应Forms或HTML5表单嵌入非AEM网页所需的配置步骤。
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 398
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 29%

---

# 在网页中嵌入自适应表单或HTML5表单

嵌入的自适应表单功能齐全，用户无需离开页面即可填写并提交表单。它帮助用户留在网页上其他元素的上下文中，并同时与该表单交互。

以下视频介绍在网页中嵌入Adaptive或HTML5表单所需的步骤。
请参阅[文档](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html)以了解最佳先决条件、最佳实践等。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

您可以从此处](assets/embedding-af-web-page.zip)下载视频[中使用的示例文件

以下代码用于获取自适应表单并将表单嵌入到由类名称&#x200B;**right**&#x200B;标识的容器中

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
