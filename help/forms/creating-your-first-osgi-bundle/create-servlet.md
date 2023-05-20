---
title: 在AEM Forms中建立您的第一個servlet
description: 建立您的第一個Sling servlet以合併資料與表單範本。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# Sling Servlet

Servlet是一種類別，用來擴充裝載應用程式的伺服器的功能，這些應用程式可透過要求 — 回應程式設計模型來存取。 對於這類應用程式，Servlet技術會定義HTTP特定的servlet類別。
所有servlet都必須實作Servlet介面，該介面定義生命週期方法。


AEM中的servlet可註冊為OSGi服務：您可以將SlingSafeMethodsServlet擴充為唯讀實作或SlingAllMethodsServlet，以實作所有RESTful作業。

## Servlet程式碼

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import java.io.File;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## 建置和部署

若要建置您的專案，請遵循下列步驟：

* 開啟 **命令提示視窗**
* 导航至 `c:\aemformsbundles\mysite\core`
* 執行命令 `mvn clean install -PautoInstallBundle`
* 上述命令會自動建置套件組合，並將其部署至在localhost：4502上執行的AEM執行個體

該組合也可在以下位置使用 `C:\AEMFormsBundles\mysite\core\target`. 也可以使用將套件組合部署至AEM [Felix網頁主控台。](http://localhost:4502/system/console/bundles)


## 測試Servlet解析程式

將瀏覽器指向 [servlet解析器URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). 這會告訴您針對指定路徑叫用的servlet，如下方熒幕擷取所示
![servlet-resolver](assets/servlet-resolver.JPG)

## 使用Postman測試servlet

![使用Postman測試servlet](assets/test-servlet-postman.JPG)

## 后续步骤

[包含協力廠商jar的](./include-third-party-jars.md)

