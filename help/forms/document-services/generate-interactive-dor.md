---
title: 使用最適化表單資料產生互動式DoR
description: 將最適化表單資料與XDP範本合併，以產生可下載的pdf
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# 下載互動式DoR

常見的使用案例是下載含有最適化表單資料的互動式DoR。 下載的DoR將使用Adobe Acrobat或Adobe Reader完成。

## 最適化表單並非以XSD結構描述為基礎

如果您的XDP和最適化表單並非以任何結構描述為基礎，則請依照下列步驟產生互動式記錄檔案。

### 建立最適化表單

建立最適化表單，並確保最適化表單欄位名稱的名稱與xdp範本中的欄位名稱相同。
記下xdp範本的根元素名稱。
![根元素](assets/xfa-root-element.png)

### 使用者端資料庫

下列程式碼會在下載PDF按鈕觸發時觸發

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

## 根據XSD結構描述的最適化表單

如果您的xdp不是以XSD為基礎，則請遵循以下步驟來建立您將作為最適化表單基礎的XSD(schema)

### 產生XDP的範例資料

* 在AEM Forms Designer中開啟XDP。
* 按一下檔案 |表單屬性 |預覽
* 按一下產生預覽資料
* 按一下「產生」
* 提供有意義的檔案名稱，例如&quot;form-data.xml&quot;

### 從xml資料產生XSD

您可以使用任何免費線上工具來 [產生XSD](https://www.freeformatter.com/xsd-generator.html) 來自上一步驟中產生的xml資料。

### 建立最適化表單

根據上一步的XSD建立最適化表單。 建立表單關聯以使用使用者端程式庫「irs」。 此使用者端程式庫具有向servlet進行POST呼叫的程式碼，該程式碼會將PDF傳回至呼叫的應用程式。以下程式碼會在 _下載PDF_ 已點按

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



## 建立自訂servlet

建立自訂servlet，將資料與XDP範本合併並傳回pdf。 完成此任務的程式碼如下。 自訂servlet是 [AEMFormsDocumentServices.core-1.0-SNAPSHOT套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar))。

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

在範常式式碼中，我們從請求物件中擷取xdp名稱和其他引數。 如果表單不是以XSD為基礎，則會建立要與xdp合併的xml檔案。如果表單是以XSD為基礎，我們只會從最適化表單提交的資料中擷取適當的節點，以產生xml檔案，以便與xdp範本合併。

## 在您的伺服器上部署範例

若要在本機伺服器上測試此專案，請遵循下列步驟：

1. [下載並安裝DevelopingWithServiceUser套裝](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. 在Apache Sling服務使用者對應程式服務DevelopingWithServiceUser.core：getformsresourceresolver=fd-service中新增以下專案
1. [下載並安裝自訂DocumentServices套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). 這有servlet可合併資料與XDP範本，並串流PDF回來
1. [匯入使用者端資源庫](assets/generate-interactive-dor-client-lib.zip)
1. [匯入文章資產（最適化表單、XDP範本和XSD）](assets/generate-interactive-dor-sample-assets.zip)
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. 填寫一些表單欄位。
1. 按一下「下載PDF」以取得PDF。 您可能需要等候幾秒鐘，才能下載PDF。

>[!NOTE]
>
>您可以嘗試相同的使用案例，透過 [非xsd型最適化表單](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). 請務必將適當的引數傳遞至irs clientlib中streampdf.js的貼文端點。
