---
title: 在最適化Forms中顯示內嵌影像
description: 在最適化Forms中顯示內嵌的已上傳影像
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# Adaptive Forms中的內嵌影像

常見的使用案例是在最適化表單中將上傳的影像顯示為內嵌影像。 依預設，上傳的影像會顯示為連結，而以最適化表單顯示影像可增強此體驗。 本文將逐步引導您完成顯示內嵌影像的步驟。

## 新增預留位置影像

第一個步驟是在檔案附件元件前加上預留位置div。 在下面的程式碼中，檔案附件元件由其CSS類別名稱photo-upload識別。 JavaScript函式是與最適化表單相關聯的使用者端資料庫的一部分。 此函式是在檔案附件元件的初始化事件中呼叫。

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### 顯示內嵌影像

使用者上傳影像後，會在檔案附件元件的認可事件中叫用下列函式。 函式會接收上傳的檔案物件作為引數。

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### 在您的伺服器上部署

* 下載並安裝 [使用者端資源庫](assets/inline-image-client-library.zip) 使用AEM封裝管理員的AEM執行個體上。
* 下載並安裝 [範例表單](assets/inline-image-af.zip) 使用AEM套件管理員在您電腦上的AEM執行個體。
* 將瀏覽器指向 [新增內嵌影像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 按一下「附加像片」按鈕以新增影像
