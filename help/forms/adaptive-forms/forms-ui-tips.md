---
title: 一些有用的UI提示和技巧
description: 文档以演示一些有用的用户界面提示
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# 切换密码字段可见性

一个常见用例是，允许表单填充程序切换到在密码字段中输入的文本的可见性。
为了完成此用例，我使用了 [Font Awesome库](https://fontawesome.com/). 为此演示创建的客户端库中包含所需的CSS和eye.svg。


## 示例代码

自适应表单的字段类型为PasswordBox，称为 **ssnField**.

加载表单时将执行以下代码

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

以下CSS用于定位 **眼睛** 密码字段内的图标

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## 部署切换密码示例

* 下载 [客户端库](assets/simple-ui-tips.zip)
* 下载 [示例表单](assets/simple-ui-tricks-form.zip)
* 使用 [包管理器UI](http://localhost:4502/crx/packmgr/index.jsp)
* 使用 [Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


