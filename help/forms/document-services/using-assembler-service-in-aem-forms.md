---
title: 在AEM Forms中使用汇编程序服务
seo-title: 在AEM Forms中使用汇编程序服务
description: 使用AEM Forms中的汇编程序服务来汇编多个PDF文件
seo-description: 使用AEM Forms中的汇编程序服务来汇编多个PDF文件
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: 汇编程序
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 3%

---


# 在AEM Forms中使用汇编程序服务{#using-assembler-service-in-aem-forms}

本文为您提供了一些资产，用于演示将多个PDF文件拖放到浏览器中，并将汇编的PDF文件保存到您的文件系统的功能。 以下是Servlet的代码，该代码可组合使用浏览器上传的pdf文件。

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

使此功能在您的AEM Server上正常工作

* 将[AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip)下载到本地系统。
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)上载并安装包
* 下载[自定义文档服务包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下载[使用服务用户包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)部署和启动包
* 将您的浏览器指向[AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* 拖放几个PDF文件文件

>[!NOTE]
>
>确保AEM Forms安装完成。 所有包都需要处于活动状态。
>
>确保已添加 — Boot delegate RSA和BouncyCastle库，如本[安装AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)中所述
>
>**此演示的注意事项**
>
> * 代码不处理基于XFA的PDF文档
   >
   > 
* 确保仅拖放PDF文件
>
>







