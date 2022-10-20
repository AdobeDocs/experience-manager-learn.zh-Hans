---
title: 在自适应Forms中显示内联图像
description: 在自适应Forms中显示内嵌的上传图像
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
kt: kt-11307
source-git-commit: 853c4fedd4b8db594aa0b53fd2d27d996811f14e
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 在自适应Forms中显示DAM图像

一个常见用例是在自适应表单中显示内联crx存储库中的图像。

## 添加占位符图像

第一步是在面板组件前面附加一个占位符div。 在下面的代码中，面板组件通过其照片上传的CSS类名称进行标识。 JavaScript函数是与自适应表单关联的客户端库的一部分。 在文件附件组件的初始化事件中调用此函数。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 显示内嵌图像

用户选择图像后，隐藏的字段ImageName将填充选定的图像名称。 然后，此图像名称将传递到damURLToFile函数，该函数调用createFile函数，以将URL转换为FileReader.readAsDataURL()的Blob。

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

### 在服务器上部署

* 下载并安装 [客户端库和示例图像](assets/InlineDAMImage.zip) 在AEM实例上使用AEM包管理器。
* 下载并安装 [示例表单](assets/FieldInspectionForm.zip) 使用AEM包管理器在您的AEM实例上。
* 将您的浏览器指向 [FileInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 选取一个夹具
* 您应会看到表单中显示的图像