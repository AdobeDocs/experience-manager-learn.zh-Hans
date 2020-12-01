---
title: 在AEM Forms使用汇编服务
seo-title: 在AEM Forms使用汇编服务
description: 使用AEM Forms的Assembler Service组合多个pdf文件
seo-description: 使用AEM Forms的Assembler Service组合多个pdf文件
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---


# 使用AEM Forms{#using-assembler-service-in-aem-forms}中的汇编器服务

本文为您提供了一些资源，用于演示将多个PDF文件拖放到浏览器中以及将组合的pdf文件保存到文件系统的能力。 以下是servlet的代码，它汇编使用浏览器上传的pdf文件。

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

使此功能在AEM Server上工作

* 将[AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip)下载到本地系统。
* 使用[包管理器](http://localhost:4502/crx/packmgr/index.jsp)上载和安装包
* 下载[自定义文档服务捆绑](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下载[使用服务用户捆绑开发](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)部署和开始捆绑套件
* 将您的浏览器指向[AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* 拖放几个PDF文件

>[!NOTE]
>
>确保您的AEM Forms安装完成。 您的所有捆绑包都必须处于活动状态。
>
>确保已添加——如本[安装AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)中所述的引导委托RSA和BouncyCastle库
>
>**此演示的注意事项**
>
> * 代码不处理基于XFA的PDF文档
   >
   > 
* 确保仅拖放PDF文件
>
>







