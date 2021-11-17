---
title: 从一个数据文件生成多个PDF
description: OutputService提供了多种方法来使用表单设计和数据创建文档，以便与表单设计合并。 了解如何从一个包含多个单个记录的大型xml中生成多个pdf。
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 0%

---

# 从一个xml数据文件生成一组PDF文档

OutputService提供了多种方法来使用表单设计和数据创建文档，以便与表单设计合并。 以下文章介绍了用例，该用例用于从一个包含多个单个记录的大型xml中生成多个pdf。
以下是包含多个记录的xml文件的屏幕截图。

![多记录xml](assets/multi-record-xml.PNG)

数据xml有2条记录。 每个记录由form1元素表示。 此xml将传递到OutputService [generatePDFOutputBatch方法](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) 我们获得pdf文档列表（每条记录一个）， generatePDFOutputBatch方法的签名采用以下参数

* 模板 — 包含模板的映射，由键标识
* data — 包含xml数据文档的映射，由键标识
* pdfOutputOptions — 用于配置pdf生成的选项
* batchOptions — 用于配置批处理的选项



## 用例详细信息{#use-case-details}

在此用例中，我们将提供一个简单的Web界面来上传模板和data(xml)文件。 文件上传完成并将POST请求发送到AEM Servlet后。 此Servlet提取文档并调用OutputService的generatePDFOutputBatch方法。 生成的PDF文件会压缩为zip文件，以供最终用户从Web浏览器下载。

## Servlet代码{#servlet-code}

以下是Servlet中的代码段。 代码从请求中提取模板(xdp)和数据文件(xml)。 模板文件将保存到文件系统。 创建了两个映射 — templateMap和dataFileMap ，它们分别包含模板和xml(data)文件。 然后，调用以生成DocumentServices服务的multipleRecords方法。

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

### 界面实施代码{#Interface-Implementation-Code}

以下代码使用OutputService的generatePDFOutputBatch生成多个PDF，并将包含PDF文件的zip文件返回给调用Servlet

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

### 在服务器上部署{#Deploy-on-your-server}

要在您的服务器上测试此功能，请按照以下说明操作：

* [将zip文件内容下载并解压缩到您的文件系统](assets/mult-records-template-and-xml-file.zip).此zip文件包含模板和xml数据文件。
* [将您的浏览器指向Felix Web控制台](http://localhost:4502/system/console/bundles)
* [部署DevelopingWithServiceUser包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [部署自定义AEMFormsDocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).自定义包，该包使用OutputService API生成PDF
* [将浏览器指向包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [导入和安装包](assets/generate-multiple-pdf-from-xml.zip). 此包中包含html页，用于放置模板和数据文件。
* [将您的浏览器指向MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* 将模板和xml数据文件拖放到一起
* 下载创建的zip文件。 此zip文件包含由输出服务生成的pdf文件。

>[!NOTE]
>有多种方法可触发此功能。 在本例中，我们使用Web界面拖放模板和数据文件以演示该功能。
