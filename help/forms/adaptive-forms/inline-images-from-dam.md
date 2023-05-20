---
title: 在最適化Forms中顯示內嵌的DAM影像
description: 在最適化Forms中顯示內嵌的DAM影像
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 在最適化Forms中顯示DAM影像

常見的使用案例是以最適化表單內嵌顯示crx存放庫中的影像。

## 新增預留位置影像

第一個步驟是在面板元件前面加上預留位置div。 在下面的程式碼中，面板元件由其CSS類別名稱photo-upload識別。 JavaScript函式是與最適化表單相關聯的使用者端資料庫的一部分。 此函式是在檔案附件元件的初始化事件中呼叫。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 顯示內嵌影像

使用者選取影像後，隱藏欄位ImageName會填入選取的影像名稱。 然後，此影像名稱會傳遞至damURLToFile函式，該函式會呼叫createFile函式，將URL轉換為FileReader.readAsDataURL()的Blob。

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### 在您的伺服器上部署

* 下載並安裝 [使用者端資料庫和範例影像](assets/InlineDAMImage.zip) 使用AEM Package Manager的AEM執行個體上。
* 下載並安裝 [範例表單](assets/FieldInspectionForm.zip) 使用AEM套件管理員在您電腦上的AEM執行個體。
* 將瀏覽器指向 [FielInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 選取其中一個夾具
* 您應該會看到表單中顯示的影像
