---
title: 在自适应Forms中内联显示DAM图像
description: 在自适应Forms中内联显示DAM图像
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

# 在自适应Forms中显示DAM图像

一个常见用例是以自适应表单内联显示驻留在crx存储库中的图像。

## 添加占位符图像

第一步是在面板组件前附加占位符div。 在下面的代码中，面板组件由其CSS类名称photo-upload标识。 JavaScript函数是与自适应表单关联的客户端库的一部分。 在文件附件组件的初始化事件中调用此函数。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 显示内嵌图像

用户选择图像后，使用选定的图像名称填充隐藏字段ImageName。 然后，将此图像名称传递到damURLToFile函数，该函数调用createFile函数以将URL转换为FileReader.readAsDataURL()的Blob。

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

### 在您的服务器上部署

* 下载并安装 [客户端库和示例图像](assets/InlineDAMImage.zip) (在您的AEM实例上使用AEM包管理器)。
* 下载并安装 [示例表单](assets/FieldInspectionForm.zip) ，以使用AEM包管理器的AEM实例。
* 将浏览器指向 [字段检查表单](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 选取一个夹具
* 您应会看到表单中显示的图像
