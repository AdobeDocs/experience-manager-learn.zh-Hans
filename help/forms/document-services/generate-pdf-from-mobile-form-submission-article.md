---
title: 从HTM5表单提交生成PDF
description: 通过移动设备表单提交生成PDF
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
duration: 132
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# 从HTM5表单提交生成PDF {#generate-pdf-from-htm-form-submission}

本文将引导您完成从HTML5(又称Mobile Forms)表单提交生成PDF时涉及的步骤。 此演示还将说明将图像添加到HTML5表单并将图像合并到最终PDF中所需的步骤。


要将提交的数据合并到xdp模板中，请执行以下操作

编写一个servlet来处理HTML5表单提交

* 在此servlet内获取已提交的数据
* 将此数据与xdp模板合并以生成pdf
* 将pdf流回调用应用程序

以下是从请求中提取已提交数据的servlet代码。 然后，它会调用自定义documentServices .mobileFormToPDF方法以获取pdf。

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

要将图像添加到移动设备表单并在PDF中显示该图像，我们使用了以下内容

XDP模板 — 在xdp模板中，我们添加了一个名为btnAddImage的图像字段和按钮。 以下代码处理自定义配置文件中btnAddImage的单击事件。 如您所见，我们将触发file1点击事件。 xdp中无需编码即可完成此用例

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[自定义配置文件](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)。 使用自定义配置文件，可以更轻松地处理移动设备表单的HTMLDOM对象。 隐藏的文件元素将添加到HTML.jsp中。 当用户点击“添加照片”时，我们将触发文件元素的点击事件。 这允许用户浏览并选择要附加的照片。 然后使用javascript FileReader对象获取图像的base64编码字符串。 base64图像字符串存储在表单的文本字段中。 在提交表单时，我们将提取此值并将其插入XML的img元素中。 然后，使用此XML与xdp合并以生成最终pdf。

用于本文的自定义配置文件已作为本文资源的一部分提供给您。

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

当我们触发文件元素的单击事件时，将执行上述代码。 在第5行，我们将上传文件的内容提取为base64字符串并存储在文本字段中。 然后，在表单提交到我们的servlet时，将提取此值。

然后，我们在AEM中配置移动表单的以下属性（高级）

* 提交URL - http://localhost:4502/bin/handlemobileformsubmission。 这是我们的servlet，它将提交的数据与xdp模板合并
* HTML渲染配置文件 — 确保选择“AddImageToMobileForm”。 这将触发代码以将图像添加到表单。

要在您自己的服务器上测试此功能，请执行以下步骤：

* [部署AemFormsDocumentServices捆绑包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [部署Developing with Service用户包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并安装与本文关联的包。](assets/pdf-from-mobile-form-submission.zip)

* 通过查看[xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)的HTML页，确保已正确设置提交URL和属性渲染配置文件

* [以html预览XDP](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 将图像添加到表单并提交。 您应该重新获取PDF，并在其中插入图像。
