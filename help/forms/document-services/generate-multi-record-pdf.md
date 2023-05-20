---
title: 從單一資料檔案產生多個pdf
description: OutputService提供許多使用表單設計建立檔案的方法，以及要與表單設計合併的資料。 瞭解如何從包含多個個別記錄的一個大型xml產生多個pdf。
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
last-substantial-update: 2020-01-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 0%

---

# 從一個xml資料檔案產生一組PDF檔案

OutputService提供許多使用表單設計建立檔案的方法，以及要與表單設計合併的資料。 以下文章將說明使用案例，以從包含多個個別記錄的一個大型xml產生多個pdf。
以下是包含多個記錄的xml檔案的熒幕擷取畫面。

![multi-record-xml](assets/multi-record-xml.PNG)

資料xml有2筆記錄。 每個記錄由form1元素表示。 此xml傳遞至OutputService [generatePDFOutputBatch方法](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) 我們會取得pdf檔案清單（每個記錄一個） generatePDFOutputBatch方法的簽章會採用下列引數

* 範本 — 包含範本的對應，以索引鍵識別
* 資料 — 包含xml資料檔案的對應，以索引鍵識別
* pdfOutputOptions — 設定pdf產生程式的選項
* batchoptions — 設定批次的選項



## 使用案例詳細資訊{#use-case-details}

在此使用案例中，我們將提供簡單的網頁介面來上傳範本和資料(xml)檔案。 一旦檔案上傳完成，系統就會將POST要求傳送至AEM servlet。 此servlet會擷取檔案並呼叫OutputService的generatePDFOutputBatch方法。 產生的pdf會壓縮成zip檔案，以供一般使用者從網頁瀏覽器下載。

## Servlet程式碼{#servlet-code}

以下是servlet的程式碼片段。 程式碼會從要求中擷取範本(xdp)和資料檔案(xml)。 範本檔案會儲存至檔案系統。 已建立兩個對映 — 分別包含範本和xml（資料）檔案的templateMap和dataFileMap。 然後呼叫DocumentServices服務的generateMultipleRecords方法。

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### 介面實作程式碼{#Interface-Implementation-Code}

下列程式碼會使用OutputService的generatePDFOutputBatch產生多個pdf，並將包含pdf檔案的zip檔案傳回至呼叫的servlet

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### 在您的伺服器上部署{#Deploy-on-your-server}

若要在您的伺服器上測試此功能，請遵循下列指示：

* [下載並解壓縮zip檔案內容至您的檔案系統](assets/mult-records-template-and-xml-file.zip).此zip檔案包含範本和xml資料檔案。
* [將瀏覽器指向Felix網頁主控台](http://localhost:4502/system/console/bundles)
* [部署DevelopingWithServiceUser套裝](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [部署自訂AEMFormsDocumentServices套裝](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).使用OutputService API產生PDF的自訂套件
* [將瀏覽器指向封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [匯入並安裝套件](assets/generate-multiple-pdf-from-xml.zip). 此套件包含html頁面，可讓您放置範本和資料檔案。
* [將瀏覽器指向MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html？)
* 將範本和xml資料檔案拖放在一起
* 下載已建立的zip檔案。 此zip檔案包含輸出服務產生的pdf檔案。

>[!NOTE]
>有多種方式可觸發此功能。 在此範例中，我們使用網頁介面放入範本和資料檔案來示範此功能。
