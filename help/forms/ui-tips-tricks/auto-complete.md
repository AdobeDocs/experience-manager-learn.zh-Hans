---
title: AEM Forms中的自动完成功能
description: 利用搜索和筛选，用户可在键入值时从预填充的值列表中快速查找和选择值。
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# 实施自动完成

使用jquery的自动完成功能在AEM表单中实施自动完成功能。
本文中包含的示例使用各种数据源（静态数组、从REST API响应填充的动态数组）在用户开始在文本字段中键入内容时填充建议。

用于完成自动完成功能的代码与字段的初始化事件相关联。


## 为国家/地区名称提供建议

![国家建议](assets/auto-complete1.png)

## 提供地址建议

![国家建议](assets/auto-complete2.png)

以下代码用于提供街道地址建议

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

## 关于表情符号的建议

![国家建议](assets/auto-complete3.png)

以下代码用于在建议列表中显示表情符号

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

的 [示例表单可下载](assets/auto-complete-form.zip) 从这里。 请确保使用代码编辑器提供您自己的用户名/API密钥，以便成功进行REST调用。

>[!NOTE]
>
> 为了自动完成工作，请确保您的表单使用以下客户端库 **cq.jquery.ui**. 此客户端库随AEM一起提供。
