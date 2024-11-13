---
title: 使用自适应表单数据生成交互式DoR
description: 将自适应表单数据与XDP模板合并以生成可下载的pdf
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
duration: 177
source-git-commit: 2625a9127c36ee191eb67128546864c9f6901663
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# 下载交互式DoR

一个常见用例是能够下载带有自适应表单数据的交互式DoR。 随后将使用Adobe Acrobat或Adobe Reader完成下载的DoR。

## 自适应表单不基于XSD架构

如果您的XDP和自适应表单不基于任何架构，请按照以下步骤生成交互式记录文档。

### 创建自适应表单

创建自适应表单并确保自适应表单字段名称的名称与xdp模板中的字段名称相同。
记下xdp模板的根元素名称。
![根元素](assets/xfa-root-element.png)

### 客户端库

以下代码在触发“下载PDF”按钮时触发

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
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

## 基于XSD架构的自适应表单

如果您的XDP不是基于XSD，请按照以下步骤创建您的自适应表单所基于的XSD（架构）

### 为XDP生成示例数据

* 在AEM Forms Designer中打开XDP。
* 单击“文件” | 表单属性 | 预览
* 单击生成预览数据
* 单击“生成”
* 提供有意义的文件名，如“form-data.xml”

### 从xml数据生成XSD

您可以使用任何免费在线工具从上一步中生成的xml数据中[生成XSD](https://www.freeformatter.com/xsd-generator.html)。

### 创建自适应表单

根据上一步中的XSD创建自适应表单。 关联表单以使用客户端库“irs”。 此客户端库具有向servlet进行POST调用的代码，该servlet会将PDF返回到调用应用程序。
单击_下载PDF_&#x200B;时触发以下代码

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
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



## 创建自定义servlet

创建自定义servlet以将数据与XDP模板合并并返回pdf。 下面列出了完成此任务的代码。 自定义servlet是[AEMFormsDocumentServices.core-1.0-SNAPSHOT包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)的一部分。

```java
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
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

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

在此示例代码中，xdp名称和其他参数是从请求对象中提取的。 如果表单不基于XSD，则会创建新的XML文档以与xdp合并。 但是，如果表单是基于XSD的，则直接从自适应表单提交的数据中提取相关节点，并生成XML文档以相应地与xdp模板合并。

## 在服务器上部署示例

要在本地服务器上对此进行测试，请执行以下步骤：

1. [下载并安装DevelopingWithServiceUser捆绑包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. 在Apache Sling服务用户映射器服务中添加以下条目
DevelopingWithServiceUser.core：getformsresourceresolver=fd-service
1. [下载并安装自定义DocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。 这有了servlet将数据与XDP模板合并，并流式传输PDF
1. [导入客户端库](assets/generate-interactive-dor-client-lib.zip)
1. [导入Assets文章（自适应表单、XDP模板和XSD）](assets/generate-interactive-dor-sample-assets.zip)
1. [预览自适应表单](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. 填写一些表单字段。
1. 单击下载PDF以获取PDF。 您可能需要等待几秒钟，才能下载PDF。

>[!NOTE]
>
>当您使用浏览器的pdf查看器打开下载的PDF时，您不会看到pdf中的数据。 使用Adobe Acrobat或Adobe Reader打开下载的PDF。


>[!NOTE]
>
>您可以对[不基于xsd的自适应表单](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled)尝试相同的用例。 确保将相应的参数传递到irs clientlib中的streampdf.js中的post端点。
