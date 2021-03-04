---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在HTML5表单提交时触发AEM Workflow
description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
seo-description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
feature: 移动设备表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 1%

---


# 创建自定义用户档案

在此部分，我们将创建[自定义用户档案。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) 用户档案负责将XDP渲染为HTML。为将XDP渲染为HTML提供了现成的默认用户档案。 它表示Mobile Forms Rendition服务的自定义版本。 您可以使用Mobile Form Rendition服务自定义Mobile Forms的外观、行为和交互。 在我们的自定义用户档案中，我们将使用Guidebridge API捕获移动表单中填充的数据。 然后，此数据将发送到自定义servlet，该servlet随后将生成交互式PDF并将其流回调用的应用程序。

使用`formBridge` JavaScript API获取表单数据。 我们使用`getDataXML()`方法：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

在成功处理程序方法中，我们调用在AEM中运行的自定义servlet。 此Servlet将使用移动表单中的数据呈现和返回交互式pdf

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

以下是Servlet代码，它负责渲染交互式pdf并将pdf返回给调用应用程序。 Servlet调用自定义DocumentServices OSGi服务的`mobileFormToInteractivePdf`方法。

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

以下代码使用[Forms Service API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html)以移动表单中的数据呈现交互式PDF。

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

要视图从部分完成的移动表单下载交互式PDF的功能，[请单击此处](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content)。
下载PDF后，下一步是提交PDF以触发AEM工作流。 此工作流程将合并提交的PDF中的数据并生成非交互式PDF供审阅。

为此用例创建的自定义用户档案可作为本教程资源的一部分提供。
