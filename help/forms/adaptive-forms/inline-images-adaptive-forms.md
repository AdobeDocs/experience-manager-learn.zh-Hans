---
title: 在自适应Forms中显示内联图像
description: 在自适应Forms中显示内联上传的图像
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 58
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# 自适应Forms中的内嵌图像

一个常见用例是在自适应表单中将上传的图像显示为内嵌图像。 默认情况下，上传的图像会显示为链接，可以通过在自适应表单中显示图像来增强此体验。 本文将指导您完成显示内联图像所涉及的步骤。

## 添加占位符图像

第一步是在文件附件组件前添加占位符div。 在下面的代码中，文件附件组件由其CSS类名photo-upload标识。 JavaScript函数是与自适应表单关联的客户端库的一部分。 在文件附件组件的初始化事件中调用此函数。

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

用户上传图像后，将在文件附件组件的提交事件中调用下面列出的函数。 函数会接收上传的文件对象作为参数。

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

### 在您的服务器上部署

* 使用AEM包管理器在AEM实例上下载并安装[客户端库](assets/inline-image-client-library.zip)。
* 使用AEM包管理器在AEM实例上下载并安装[示例表单](assets/inline-image-af.zip)。
* 将浏览器指向[添加内联图像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 单击“附加照片”按钮添加图像
