---
title: 通过HTM5表单提交生成PDF
seo-title: 通过HTML5表单提交生成PDF
description: 通过提交移动表单生成PDF
seo-description: 通过提交移动表单生成PDF
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---


# 从HTM5表单提交{#generate-pdf-from-htm-form-submission}生成PDF

本文将引导您完成从HTML5(又称Mobile Forms)表单提交生成pdf所涉及的步骤。 此演示还将说明向HTML5表单添加图像以及将图像合并到最终pdf中所需的步骤。

要查看此功能的实时演示，请访问[示例服务器](https://forms.enablementadobe.com/content/samples/samples.html?query=0)并搜索“移动表单到PDF”。

要将提交的数据合并到xdp模板中，我们将执行以下操作

编写一个servlet来处理HTML5表单提交

* 在此servlet中，获取已提交数据
* 将此数据与xdp模板合并以生成pdf
* 将pdf流回调用应用程序

以下是从请求中提取已提交数据的servlet代码。 然后，它调用自定义documentServices .mobileFormToPDF方法以获取pdf。

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

要向移动表单添加图像并在pdf中显示该图像，我们使用了以下功能

XDP模板 — 在xdp模板中，我们添加了一个名为btnAddImage的图像字段和按钮。 以下代码在我们的自定义用户档案中处理btnAddImage的单击事件。 正如您所看到的，我们触发了file1单击事件。 xdp中不需要编码即可完成此用例

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[自定义用户档案](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)。使用自定义用户档案，可更轻松地处理移动表单的HTML DOM对象。 隐藏文件元素将添加到HTML.jsp。 当用户单击“添加您的照片”时，我们会触发文件元素的单击事件。 这允许用户浏览并选择要附加的照片。 然后，使用javascript FileReader对象获取图像的base64编码字符串。 base64图像字符串存储在表单的文本字段中。 提交表单后，我们会提取此值并将其插入XML的img元素中。 然后，此XML用于与xdp合并以生成最终的pdf。

您已将用于此文章的自定义用户档案作为本文资源的一部分提供给您。

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

在触发文件元素的单击事件时，将执行上述代码。 第5行，我们将上传文件的内容提取为base64字符串，并存储在文本字段中。 然后，将表单提交到我们的servlet时提取此值。

然后，我们在AEM中配置移动表单的以下属性（高级）

* 提交URL - http://localhost:4502/bin/handlemobileformsubmission。 这是我们的servlet，它将提交的数据与xdp模板合并
* HTML渲染用户档案 — 确保选择“AddImageToMobileForm”。 这将触发向表单添加图像的代码。

要在您自己的服务器上测试此功能，请执行以下步骤：

* [部署AemFormsDocumentServices捆绑包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [使用服务用户捆绑部署开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并安装与此文章关联的包。](assets/pdf-from-mobile-form-submission.zip)

* 通过查看[xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)的属性页，确保正确设置提交URL和HTML渲染用户档案

* [将XDP预览为html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 向表单中添加图像并提交。 您应将图像放回PDF。

