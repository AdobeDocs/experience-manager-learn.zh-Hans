---
title: 在HTM5表單提交時觸發AEM工作流程 — 建立自訂設定檔
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 繼續以離線模式填寫行動表單並提交行動表單以觸發AEM工作流程
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# 建立自訂設定檔

在本部分中，我們將建立 [自訂設定檔。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) 設定檔負責將XDP轉譯為HTML。 提供立即可用的預設設定檔，以將XDP呈現為HTML。 它代表Mobile Forms轉譯服務的自訂版本。 您可以使用Mobile Form Rendition服務來自訂Mobile Forms的外觀、行為和互動。 在我們的自訂設定檔中，我們將使用Guidebridge API擷取填入行動表單的資料。 然後，此資料會傳送至自訂servlet，然後產生互動式PDF並將其串流回呼叫的應用程式。

使用取得表單資料 `formBridge` JavaScript API。 我們利用 `getDataXML()` 方法：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

在成功處理常式方法中，我們會呼叫在AEM中執行的自訂servlet。 此servlet將會轉譯並傳回互動式pdf，其中包含行動表單中的資料

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## 產生互動式PDF

以下是servlet程式碼，負責轉譯互動式pdf並將pdf傳回至呼叫的應用程式。 此servlet呼叫 `mobileFormToInteractivePdf` 自訂DocumentServices OSGi服務的方法。

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
          response.setContentType("application/pdf");
          response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
          response.setContentLength((int) fileInputStream.available());
          OutputStream responseOutputStream = response.getOutputStream();
          int bytes;
          while ((bytes = fileInputStream.read()) != -1) {
              responseOutputStream.write(bytes);
          }
          responseOutputStream.flush();
          responseOutputStream.close();
      } catch (IOException e) {
        // TODO Add proper error logging
      }
    }
}
```

### 演算互動式PDF

下列程式碼會使用 [Forms服務API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) 以使用行動表單中的資料呈現互動式PDF。

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

若要檢視從部分完成的行動表單下載互動式PDF的功能， [請按這裡](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
下載PDF後，下一步就是提交PDF以觸發AEM工作流程。 此工作流程會合併來自已提交PDF的資料，並產生非互動式PDF以供檢閱。

針對此使用案例建立的自訂設定檔可作為本教學課程資產的一部分。
