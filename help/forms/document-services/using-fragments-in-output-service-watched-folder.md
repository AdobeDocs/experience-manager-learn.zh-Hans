---
title: 在具有watched資料夾的輸出服務中使用片段
description: 產生片段位於crx存放庫中的pdf檔案
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# 使用ECMA指令碼產生含片段的pdf檔案{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我們將使用輸出服務來產生使用xdp片段的pdf檔案。 主要xdp和片段位於crx存放庫中。 請務必在AEM中模擬檔案系統資料夾結構。 例如，如果您在xdp的片段資料夾中使用片段，您必須建立一個名為的資料夾 **片段** 在AEM的基本資料夾底下。 基底資料夾將包含您的基底xdp範本。 例如，如果您的檔案系統上有下列結構
* c：\xdptemplates — 這將包含您的基本xdp範本
* c：\xdptemplates\fragments — 此資料夾將包含片段，而主要範本將參考片段，如下所示
   ![fragment-xdp](assets/survey-fragment.png).
* 資料夾xdpdocuments將包含您的基礎範本和片段 **片段** 資料夾

您可以使用建立所需的結構 [表單和檔案ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

以下是使用2個片段的範例xdp的資料夾結構
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* 輸出服務 — 此服務通常用於合併xml資料與xdp範本或pdf，以產生平面化pdf。 如需詳細資訊，請參閱 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 用於Output服務 在此範例中，我們使用crx存放庫中的片段。


下列ECMA指令碼已用於產生PDF。 請注意程式碼中使用ResourceResolver和ResourceResolverHelper。 需要ResourceReolver，因為此程式碼在任何使用者內容之外執行。

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**在您的系統上測試範例套件的方式**
* [部署DevelopingWithServiceUSer套件](assets/DevelopingWithServiceUser.jar)
* 新增專案 **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service** 使用者對應程式服務修正中，如下方熒幕擷圖所示
   ![使用者對應程式修正](assets/user-mapper-service-amendment.png)
* [下載並匯入範例xdp檔案和ECMA指令碼](assets/watched-folder-fragments-ecma.zip).
這會在c：/fragmentsandoutputservice資料夾中建立watched資料夾結構

* [擷取範例資料檔案](assets/usingFragmentsSampleData.zip) 並將其放置在監視資料夾的安裝資料夾中(c：\fragmentsandoutputservice\install)

* 檢查watched資料夾設定的結果資料夾，找出產生的pdf檔案
