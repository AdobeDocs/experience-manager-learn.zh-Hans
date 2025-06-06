---
title: 在AEM Forms中使用Assembler服务
description: 在AEM Forms中使用Assembler服务来汇编多个PDF文件
feature: Assembler
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
duration: 76
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# 在AEM Forms中使用Assembler服务{#using-assembler-service-in-aem-forms}

本文提供了用于演示将多个PDF文件拖放到浏览器中，并将组合的pdf文件保存到文件系统中的功能的各种资源。 以下是servlet的代码，该代码可组合使用浏览器上传的pdf文件。

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

使此功能在您的AEM服务器上正常工作

* 将[AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip)下载到您的本地系统。
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)上载并安装包
* 下载[自定义文档服务包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下载[使用服务用户捆绑包进行开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)部署和启动包
* 将浏览器指向[AssemblePdf.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* 拖放多个PDF文件

>[!NOTE]
>
>确保您的AEM Forms安装已完成。 您的所有包都必须处于活动状态。
>
>确保您已添加 — 引导代理RSA和BouncyCastle库，如此[安装AEM Forms](https://helpx.adobe.com/cn/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)中所述
>
>此演示的&#x200B;**注意事项**
>
> * 该代码无法处理基于XFA的PDF文档
>
> * 确保只拖放PDF文件
>
>
