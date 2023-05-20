---
title: 從HTM5表單提交產生PDF
description: 從行動表單提交產生PDF
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---

# 從HTM5表單提交產生PDF {#generate-pdf-from-htm-form-submission}

本文將逐步引導您完成從HTML5 (亦稱為Mobile Forms)表單提交產生PDF的步驟。 此示範也將說明將影像新增到HTML5表單以及將影像合併到最終pdf所需的步驟。


若要將提交的資料合併至xdp範本，請執行下列動作

撰寫Servlet以處理HTML5表單提交

* 在此servlet內取得提交的資料
* 將此資料與xdp範本合併以產生pdf
* 將pdf串流回呼叫應用程式

以下是從請求中擷取已提交資料的servlet程式碼。 然後它會呼叫自訂documentServices .mobileFormToPDF方法來取得pdf。

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

若要新增影像至行動表單，並在PDF中顯示該影像，我們使用了下列專案

XDP範本 — 在xdp範本中，我們新增了影像欄位和名為btnAddImage的按鈕。 下列程式碼會處理自訂設定檔中btnAddImage的click事件。 如您所見，我們觸發file1點選事件。 xdp中不需要編碼即可完成此使用案例

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[自訂設定檔](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). 使用自訂設定檔可讓您更輕鬆地操控行動表單的HTMLDOM物件。 隱藏的檔案元素會新增至HTML.jsp。 當使用者點選「新增您的像片」時，我們會觸發檔案元素的點選事件。 這可讓使用者瀏覽並選取要附加的照片。 然後使用javascript FileReader物件來取得影像的base64編碼字串。 base64影像字串會儲存在表單的文字欄位中。 提交表單時，我們會擷取此值，並將其插入XML的img元素中。 然後使用此XML與xdp合併，以產生最終pdf。

用於本文的自訂設定檔已可供您使用作為本文資產的一部分。

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

當我們觸發檔案元素的點選事件時，就會執行上述程式碼。 第5行：我們將上傳檔案的內容擷取為base64字串，並儲存在文字欄位中。 然後在表單提交至我們的servlet時擷取此值。

接著，我們會在AEM中設定行動表單的下列屬性（進階）

* 提交URL - http://localhost:4502/bin/handlemobileformsubmission。 這是我們的servlet，會將提交的資料與xdp範本合併
* HTML演算設定檔 — 請務必選取「AddImageToMobileForm」。 這會觸發程式碼，將影像新增至表單。

若要在您自己的伺服器上測試此功能，請遵循下列步驟：

* [部署AemFormsDocumentServices套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [部署使用服務進行開發的使用者套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [下載並安裝與本文相關的套件。](assets/pdf-from-mobile-form-submission.zip)

* 檢視的屬性頁面，確定已正確設定提交URL和HTML轉譯器設定檔  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [以html預覽XDP](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 將影像新增至表單並提交。 您應該要取回包含影像的PDF。
