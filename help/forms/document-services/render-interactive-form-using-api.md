---
title: 在AEM Forms中使用Forms服務呈現互動式PDF
description: 在AEM Forms中使用Forms Service API來呈現互動式PDF
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# 在AEM Forms中使用Forms服務呈現互動式PDF

在AEM Forms中使用Forms Service API來呈現互動式PDF

在本文中，我們將瞭解以下服務

* FormsService — 這是一項功能非常廣泛的服務，可讓您從PDF檔案匯出/匯入資料，也可以將xml資料合併到xdp範本中來產生互動式pdf

官方 [此處列出適用於AEM Forms API的javadoc](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

下列程式碼片段會使用FormsService的renderPDFForm作業來轉譯互動式pdf。 schengen.xdp是用於合併xml資料的範本。

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

第1行：包含xdp範本的資料夾位置

Line2-4：建立PDFFormRenderOptions並設定其屬性

第7行：使用FormsService的renderPDFForm服務操作產生互動式PDF

第11行：將產生的互動式pdf傳回呼叫應用程式

**在您的系統上測試範例套件的方式**
1. [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [使用Felix Web Console下載並安裝DocumentServices範例套件](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用AEM封裝管理員下載並安裝封裝](assets/downloadinteractivepdffrommobileform.zip)

1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增以下路徑並儲存
1. /bin/generateinteractivepdf
1. 搜尋 _Apache Sling服務使用者對應程式服務_ 並按一下以開啟屬性
   1. 按一下 *+* 圖示（加號）以新增以下服務對應
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. 按一下「儲存」
1. [開啟行動表單](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填寫一些欄位，然後按一下 ***下載並填寫....*** 按钮
1. 互動式pdf應下載至您的本機系統


範例套件包含與行動表單相關聯的自訂設定檔。 請探索 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 檔案。 此jsp會從行動表單中擷取資料，並向Servlet掛載提出POST請求 ***/bin/generateinteractivepdf*** 路徑。 此servlet會將互動式pdf傳回至呼叫的應用程式。 customtoolbar.jsp中的程式碼然後會將檔案下載到您的本機系統
