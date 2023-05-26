---
title: 在HTM5表单提交时触发AEM工作流 — 创建自定义配置文件
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 继续以离线模式填写移动表单并提交移动表单以触发AEM Workflow
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

# 创建自定义配置文件

在本部分中，我们将创建 [自定义配置文件。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) 配置文件负责将XDP呈现为HTML。 现成提供默认配置文件，用于将XDP渲染为HTML。 它表示自定义版本的Mobile Forms呈现版本服务。 您可以使用Mobile Form Rendition服务自定义Mobile Forms的外观、行为和交互。 在我们的自定义配置文件中，我们将使用Guidebridge API捕获在移动表单中填充的数据。 然后，将此数据发送到自定义servlet，后者将生成交互式PDF并将其流式传输回调用应用程序。

使用获取表单数据 `formBridge` JavaScript API。 我们利用 `getDataXML()` 方法：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

在成功处理程序方法中，我们调用在AEM中运行的自定义servlet。 此servlet将渲染并返回带有移动表单数据的交互式pdf

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

## 生成交互式PDF

以下是servlet代码，该代码负责渲染交互式pdf并将pdf返回到调用应用程序。 servlet调用 `mobileFormToInteractivePdf` 自定义DocumentServices OSGi服务的方法。

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

### 渲染交互式PDF

以下代码利用 [Forms服务API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) 使用移动表单中的数据渲染交互式PDF。

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

要查看从部分完成的移动表单下载交互式PDF的功能， [请单击此处](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
下载PDF后，下一步是提交PDF以触发AEM工作流。 此工作流将合并来自已提交PDF的数据，并生成非交互式PDF以供审阅。

为此用例创建的自定义配置文件可在本教程资产中提供。
