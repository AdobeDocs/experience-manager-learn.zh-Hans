---
title: 从HTM5表单提交生成PDF
description: 从移动设备表单提交生成PDF
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---

# 从HTM5表单提交生成PDF {#generate-pdf-from-htm-form-submission}

本文将指导您完成从HTML5(即移动设备Forms)表单提交生成PDF时涉及的步骤。 此演示还将说明向HTML5表单中添加图像以及将图像合并到最终pdf中所需的步骤。


要将提交的数据合并到xdp模板中，我们会执行以下操作

编写servlet以处理HTML5表单提交

* 在此Servlet内获取已提交数据的保存
* 将此数据与xdp模板合并以生成PDF
* 将PDF流传输回呼叫应用程序

以下是Servlet代码，用于从请求中提取提交的数据。 然后，调用自定义documentServices .mobileFormToPDF方法以获取PDF。

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

要向移动设备表单中添加图像，并在PDF中显示该图像，我们使用了以下内容

XDP模板 — 在xdp模板中，我们添加了一个名为btnAddImage的图像字段和按钮。 以下代码处理自定义配置文件中btnAddImage的click事件。 如您所见，我们会触发文件1click事件。 xdp中无需编码即可完成此用例

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[自定义用户档案](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). 使用自定义配置文件可更轻松地处理移动表单的HTMLDOM对象。 隐藏的文件元素将添加到HTML.jsp中。 当用户单击“添加照片”时，我们会触发文件元素的点击事件。 这允许用户浏览并选择要附加的照片。 然后，使用javascript FileReader对象获取图像的base64编码字符串。 base64图像字符串以表单形式存储在文本字段中。 提交表单后，我们会提取此值，并将其插入XML的img元素中。 然后，此XML将用于与xdp合并，以生成最终的pdf。

本文使用的自定义用户档案已作为本文资产的一部分提供给您。

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

当我们触发文件元素的点击事件时，将执行上述代码。 第5行，我们将上传文件的内容提取为base64字符串，并存储在文本字段中。 然后，在将表单提交到我们的Servlet时，会提取此值。

然后，我们在AEM中配置移动表单的以下属性（高级）

* 提交URL - http://localhost:4502/bin/handlemobileformsubmission。 这是我们的Servlet，它将提交的数据与xdp模板合并
* HTML呈现配置文件 — 确保选择“AddImageToMobileForm”。 这将触发用于向表单添加图像的代码。

要在您自己的服务器上测试此功能，请执行以下步骤：

* [部署AemFormsDocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [部署使用服务用户包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下载并安装与本文关联的包。](assets/pdf-from-mobile-form-submission.zip)

* 通过查看的属性页面，确保正确设置提交URL和HTML渲染配置文件  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [以html形式预览XDP](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 将图像添加到表单并提交。 您应该将PDF重新包含在其中。
