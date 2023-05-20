---
title: 一些實用的UI提示和技巧
description: 檔案以示範一些實用的使用者介面提示
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

# 切換密碼欄位可見性

常見的使用案例是允許表單填寫者切換至在密碼欄位中輸入文字的可見度。
為了完成此使用案例，我使用了以下檔案中的眼睛圖示： [Font Awesome Library](https://fontawesome.com/). 必要的CSS和eye.svg包含在為此示範建立的使用者端資料庫中。


## 程式碼範例

最適化表單有一個欄位為PasswordBox型別，稱為 **ssnField**.

載入表單時執行以下程式碼

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

下列CSS是用來定位 **眼睛** 圖示在密碼欄位內

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

## 部署切換密碼範例

* 下載 [使用者端資源庫](assets/simple-ui-tips.zip)
* 下載 [範例表單](assets/simple-ui-tricks-form.zip)
* 使用匯入使用者端資料庫 [封裝管理員UI](http://localhost:4502/crx/packmgr/index.jsp)
* 使用匯入範例表單 [Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


