---
title: 在AEM Forms中使用組合器服務
description: 在AEM Forms中使用組合器服務來組合多個pdf檔案
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# 在AEM Forms中使用組合器服務{#using-assembler-service-in-aem-forms}

本文提供的資產，可讓您示範將多個PDF檔案拖放至瀏覽器，並將組合的pdf檔案儲存至檔案系統的功能。 以下是servlet的程式碼，它會組合使用瀏覽器上傳的pdf檔案。

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

若要讓此功能在您的AEM伺服器上運作

* 下載 [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) 至您的本機系統。
* 使用上傳和安裝套件 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 下載[自訂檔案服務套裝](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下載 [使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用部署及啟動套件組合 [felix web主控台](http://localhost:4502/system/console/bundles)
* 將瀏覽器指向 [AssemblePdf.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* 拖放多個PDF檔案檔案

>[!NOTE]
>
>請確定您的AEM Forms安裝已完成。 您的所有套件組合都必須處於作用中狀態。
>
>請確定您已新增 — 開機委派RSA和BouncyCastle程式庫（如本中所述） [安裝AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**此示範的注意事項**
>
> * 程式碼不會處理XFA型PDF檔案
>
> * 請確定您只拖放PDF檔案
>
>

