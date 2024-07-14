---
title: 显示内嵌记录文档
description: 将自适应表单数据与XDP模板合并，并使用Document Cloud嵌入PDF API显示内联PDF。
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 165
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# 显示DoR内联

一个常见用例是显示包含表单填写器输入的数据的pdf文档。

为完成此用例，我们使用了[Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)。

已执行以下步骤以完成集成

## 创建自定义组件以内嵌显示PDF

创建了自定义组件(embed-pdf)以嵌入由POST调用返回的pdf。

## 客户端库

在单击`viewPDF`复选框按钮时执行以下代码。 我们将自适应表单数据、模板名称传递到端点以生成PDF。 随后生成的pdf将使用embed pdf JavaScript库显示到表单填充器中。

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## 为XDP生成示例数据

* 在AEM Forms Designer中打开XDP。
* 单击“文件” | 表单属性 | 预览
* 单击生成预览数据
* 单击“生成”
* 提供有意义的文件名，如“form-data.xml”

## 从xml数据生成XSD

您可以使用任何免费在线工具从上一步中生成的xml数据中[生成XSD](https://www.freeformatter.com/xsd-generator.html)。

## 上传模板

确保使用“创建”按钮将xdp模板上传到[AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)


## 创建自适应表单

根据上一步的XSD创建自适应表单。
向自适应添加新的选项卡。 将复选框组件和embed-pdf组件添加到此选项卡
确保将复选框命名为viewPDF。
配置embed-pdf组件，如下面的屏幕快照所示
![embed-pdf](assets/embed-pdf-configuration.png)

**嵌入PDFAPI密钥** — 这是可用于嵌入pdf的密钥。 此密钥仅适用于localhost。 您可以创建[您自己的密钥](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html)并将其与其他域关联。

**返回pdf**&#x200B;的端点 — 这是自定义servlet，它将将数据与xdp模板合并并返回pdf。

**模板名称** — 这是xdp的路径。 通常，它存储在formsanddocuments文件夹下。

**PDF的文件名** — 这是将显示在嵌入pdf组件中的字符串。

## 创建自定义servlet

创建了一个自定义servlet以将数据与XDP模板合并并返回pdf。 下面列出了完成此任务的代码。 自定义servlet是[嵌入的pdf包](assets/embedpdf.core-1.0-SNAPSHOT.jar)的一部分

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
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

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## 在服务器上部署示例

要在本地服务器上对此进行测试，请执行以下步骤：

1. [下载并安装嵌入的pdf包](assets/embedpdf.core-1.0-SNAPSHOT.jar)。
这有了Servlet将数据与XDP模板合并，并使PDF流回。
1. 使用[AEM ConfigMgr](http://localhost:4502/system/console/configMgr)将/bin/getPDFToEmbed路径添加到AdobeGranite CSRF过滤器的排除路径部分中。 在生产环境中，建议使用[CSRF保护框架](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [导入客户端库和自定义组件](assets/embed-pdf.zip)
1. [导入自适应表单和模板](assets/embed-pdf-form-and-xdp.zip)
1. [预览自适应表单](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. 填写一些表单字段
1. 按Tab键转到“查看PDF”选项卡。 选中查看PDF复选框。 您应该会看到在填充了自适应表单数据的表单中显示的pdf
