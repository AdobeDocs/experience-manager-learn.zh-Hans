---
title: 在网页中嵌入自适应Forms/HTML5表单
description: 在非AEM网页中嵌入自适应Forms或HTML5表单所需的配置步骤。
feature: 自适应表单
feature-set: Adaptive Forms
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: 管理
role: Admin
level: Beginner
kt: 8390
source-git-commit: 2fc4f748fd3b8f820d1451d08c5fe01d11892029
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---


# 在网页中嵌入自适应表单或HTML5表单

嵌入式自适应表单功能齐全，用户无需离开页面即可填写和提交表单。 它有助于用户停留在网页上其他元素的上下文中，并同时与表单进行交互。

以下视频介绍在网页中嵌入自适应或HTML5表单所需的步骤。
请参阅[文档](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) ，了解最佳先决条件、最佳实践等。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

您可以从此处](assets/embedding-af-web-page.zip)下载视频[中使用的示例文件

以下是用于获取自适应表单并将表单嵌入到由类名称&#x200B;**right**&#x200B;标识的容器中的代码

```javascript
$(document).ready(function(){
  
	var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
	console.log(formName);
	$(".right").append(data);
      
    });
  
});
```














