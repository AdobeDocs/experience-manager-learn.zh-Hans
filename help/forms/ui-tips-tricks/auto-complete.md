---
title: AEM Forms中的自动完成功能
description: 使用户能够利用搜索和筛选功能，在键入值时快速查找并从中进行选择。
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
duration: 57
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# 实施自动完成

使用jquery的自动完成功能，在AEM表单中实施自动完成功能。
本文包含的示例使用各种数据源（静态数组，通过REST API响应填充的动态数组）在用户开始键入文本字段时填充建议。

用于完成自动完成功能的代码与字段的初始化事件相关联。

## 为地址提供建议

![country-suggestions](assets/auto-complete2.png)



下面是用来提供街道地址建议的代码

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```





## 使用表情符号的建议

![country-suggestions](assets/auto-complete3.png)

以下代码用于显示建议列表中的表情符号

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

此 [可以下载示例表单](assets/auto-complete-form.zip) 从这里。 请确保使用代码的代码编辑器提供您自己的用户名/API密钥，以成功进行REST调用。

>[!NOTE]
>
> 要使自动完成正常工作，请确保您的表单使用以下客户端库 **cq.jquery.ui**. 此客户端库随AEM提供。
