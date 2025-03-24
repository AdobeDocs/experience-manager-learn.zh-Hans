---
title: 一些有用的UI提示和技巧
description: 文档演示一些有用的用户界面提示
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
last-substantial-update: 2019-06-09T00:00:00Z
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# 切换密码字段可见性

一个常见用例是允许表单填充器切换到显示在密码字段中输入的文本。
为完成此用例，我使用了[Font Awesome Library](https://fontawesome.com/)中的眼睛图标。 所需的CSS和eye.svg包含在为此演示创建的客户端库中。


## 示例代码

自适应表单具有名为&#x200B;**ssnField**&#x200B;的PasswordBox类型字段。

加载表单时执行以下代码

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

以下CSS用于在密码字段中定位&#x200B;**eye**&#x200B;图标

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

* 下载[客户端库](assets/simple-ui-tips.zip)
* 下载[示例表单](assets/simple-ui-tricks-form.zip)
* 使用[包管理器UI](http://localhost:4502/crx/packmgr/index.jsp)导入客户端库
* 使用[Forms和文档](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入示例表单
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


