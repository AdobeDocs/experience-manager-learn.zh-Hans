---
title: 在自适应Forms中显示内联图像
description: 在自适应Forms中显示内嵌的上传图像
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# 自适应Forms中的内联图像

在自适应表单中，常见的用例是将上传的图像显示为内嵌图像。 默认情况下，上传的图像会显示为链接，通过在自适应表单中显示图像，可以增强此体验。 本文将指导您完成显示内联图像时涉及的步骤。

## 添加占位符图像

第一步是在文件附件组件的前面添加占位符div。 在下面的代码中，文件附件组件通过其照片上传的CSS类名称进行标识。 JavaScript函数是与自适应表单关联的客户端库的一部分。 在文件附件组件的初始化事件中调用此函数。

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### 显示内嵌图像

用户上传图像后，将在文件附件组件的提交事件中调用下面列出的函数。 函数将上传的文件对象作为参数进行接收。

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

* 下载并安装 [客户端库](assets/inline-image-client-library.zip) 在AEM实例上使用AEM包管理器。
* 下载并安装 [示例表单](assets/inline-image-af.zip) 使用AEM包管理器在您的AEM实例上。
* 将您的浏览器指向 [添加内联图像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 单击“附加照片”按钮以添加图像
