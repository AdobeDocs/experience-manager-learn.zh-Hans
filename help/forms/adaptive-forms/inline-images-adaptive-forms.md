---
title: 在自适应Forms中显示内联图像
seo-title: 在自适应Forms中显示内联图像
description: 在自适应Forms中显示内嵌的上传图像
seo-description: 在自适应Forms中显示内嵌的上传图像
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# 自适应Forms中的内联图像

一个常见用例是在自适应表单中将上传的图像显示为内嵌图像。 默认情况下，上传的图像会显示为链接，通过在自适应表单中显示图像可以增强此体验。 本文将引导您逐步了解显示内联图像时涉及的步骤。

## 添加占位符图像

第一步是在文件附件组件前附加一个占位符div。 在下面的代码中，文件附件组件由照片上传的CSS类名称标识。 JavaScript函数是与自适应表单关联的客户端库的一部分。 在文件附件组件的初始化事件中调用此函数。

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### 显示内联图像

用户上传图像后，将在文件附件组件的提交事件中调用下面列出的函数。 该函数接收上传的文件对象作为参数。

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

### 在服务器上部署

* 使用AEM包管理器在AEM实例上下载并安装[客户端库](assets/inline-image-client-library.zip)。
* 使用AEM包管理器在AEM实例上下载并安装[示例表单](assets/inline-image-af.zip)。
* 将浏览器指向[添加内联图像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 单击“附加照片”按钮以添加图像