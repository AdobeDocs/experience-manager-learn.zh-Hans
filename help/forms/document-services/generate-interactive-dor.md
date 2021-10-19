---
title: 使用自适应表单数据生成交互式DoR
description: 将自适应表单数据与XDP模板合并，以生成可下载的PDF
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
source-git-commit: 72a9edb3edc73cf14f13bb53355a37e707ed4c79
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---


# 下载交互式DoR

一个常见用例是能够下载包含自适应表单数据的交互式DoR。 随后，将使用Adobe Acrobat或Adobe Reader完成下载的DoR。

要完成此用例，我们需要执行以下操作

## 为xdp生成示例数据

在AEM Forms设计器中打开XDP。
单击文件 |表单属性 |预览单击生成预览数据单击生成提供有意义的文件名，如“form-data.xml”

## 从xml数据生成XSD

您可以使用任何免费的在线工具 [生成xsd](https://www.freeformatter.com/xsd-generator.html) 从上一步中生成的xml数据。

## 创建自适应

根据上一步中的xsd创建自适应表单。 关联表单以使用客户端库“irs”。 此客户端库具有用于对Servlet进行POST调用的代码，该代码会将PDF返回到调用应用程序。以下代码将在 _下载PDF_ 点击

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## 创建自定义Servlet

创建一个自定义Servlet，以将数据与xdp模板合并并返回pdf。 完成此操作的代码如下所示。 自定义Servlet是 [AEMFormsDocumentServices.core-1.0-SNAPSHOT包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar))。

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

在示例代码中，模板名称(f8918-r14e_redo-barcode_3 2.xdp)进行了硬编码。 您可以轻松地将模板名称传递到Servlet，以使此代码通用，可用于所有模板。


要在本地服务器上测试此功能，请执行以下步骤：
1. [下载并安装DevelopingWithServiceUser包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. 在Apache Sling Service用户映射器服务DevelopingWithServiceUser.core:getformsresourceresolver=fd-service中添加以下条目
1. [下载并安装自定义DocumentServices包](/hep/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). 这有一个Servlet，用于将数据与XDP模板合并并并流回PDF
1. [导入客户端库](assets/irs.zip)
1. [导入自适应表单](assets/f8918complete.zip)
! [导入XDP模板和架构](assets/xdp-template-and-xsd.zip)
1. [预览自适应表单](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. 填写几个表单字段
1. 单击下载PDF以获取PDF
