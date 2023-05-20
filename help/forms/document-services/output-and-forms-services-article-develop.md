---
title: 在AEM Forms中使用輸出和Forms服務進行開發
description: 在AEM Forms中使用輸出和Forms服務API
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 46df7b13401ee3497c871eac3b8158148c2e6a04
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# 在AEM Forms中使用輸出和Forms服務進行開發{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用輸出和Forms服務API

在本文中，我們將瞭解以下內容

* 輸出服務 — 此服務通常用於合併xml資料與xdp範本或pdf，以產生平面化pdf。 如需詳細資訊，請參閱此 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 用於Output服務
* FormsService — 這是功能非常廣泛的服務，可讓您從PDF檔案匯出/匯入資料。 如需詳細資訊，請參閱此 [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) 用於Forms服務。


下列程式碼片段會從PDF檔案匯出資料

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行會從請求中擷取pdfile

Line2會從請求中擷取saveLocation

第5行保留FormsService

第6行會從PDF檔案匯出xmlData

**在您的系統上測試範例套件的方式**

[使用AEM封裝管理員下載並安裝封裝](assets/outputandformsservice.zip)




**安裝套件後，您必須在AdobeGranite CSRF篩選中允許列出下列URL。**

1. 請依照下列步驟將上述路徑加入允許清單。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增下列3個路徑並儲存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜尋「Sling查閱者篩選器」
1. 勾選「允許空白」核取方塊。 （此設定僅供測試用途）測試範常式式碼的方法有很多種。 最快捷、最輕鬆的方式就是使用Postman應用程式。 Postman可讓您向伺服器發出POST請求。 在您的系統上安裝Postman app。
啟動應用程式並輸入以下URL以測試匯出資料API

確定您已從下拉式清單中選取「POST」 http://localhost:4502/content/AemFormsSamples/exportdata.html請務必將「授權」指定為「基本驗證」。 指定AEM伺服器使用者名稱和密碼瀏覽至「內文」標籤，然後指定要求引數，如下圖所示
![匯出](assets/postexport.png)
然後按一下傳送按鈕

此套件包含3個範例。 以下段落說明何時使用輸出服務或Forms服務、服務的URL、每個服務預期的輸入引數

## 合併資料並平面化輸出

* 使用輸出服務將資料與xdp或pdf檔案合併，以產生平面化pdf
* **POSTURL**： http://localhost:4502/content/AemFormsSamples/outputservice.html
* **請求引數 —**

   * **xdp_or_pdf_file** ：您要合併資料的xdp或pdf檔案
   * **xmlfile**：與xdp_or_pdf_file合併的xml資料檔案
   * **saveLocation**：將演算後的檔案儲存在檔案系統上的位置。 例如c：\\documents\\sample.pdf

### 將資料匯入PDF檔案

* 使用FormsService將資料匯入PDF檔案
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **要求引數：**

   * **個人檔案** ：您要合併資料的pdf檔案
   * **xmlfile**：與pdf檔案合併的xml資料檔案
   * **saveLocation**：將演算後的檔案儲存在檔案系統上的位置。 例如 `c:\\outputsample.pdf`。

**從PDF檔案匯出資料**
* 使用FormsService從PDF檔案匯出資料
* **POSTUR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **要求引數：**

   * **個人檔案** ：您要匯出資料的pdf檔案
   * **saveLocation**：將匯出的資料儲存至檔案系統的位置。 例如c：\\documents\\exported_data.xml

[您可以匯入此Postman集合來測試API](assets/document-services-postman-collection.json)
