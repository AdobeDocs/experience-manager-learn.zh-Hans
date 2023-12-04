---
title: 带有自适应Forms的条形码服务
description: 使用条形码服务对条形码进行解码，并根据提取的数据填充表单字段。
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 169
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# 带有自适应Forms的条形码服务{#barcode-service-with-adaptive-forms}

本文将演示使用条形码服务填充自适应表单。 用例如下所示：

1. 用户添加带有条形码的PDF作为自适应表单附件
1. 附件的路径将发送到servlet
1. 此servlet对条形码进行了解码，并以JSON格式返回数据
1. 然后，使用解码的数据填充自适应表单

以下代码对条形码进行解码，并使用解码的值填充JSON对象。 然后，servlet在响应调用应用程序时返回JSON对象。



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

以下是servlet代码。 当用户将附件添加到自适应表单时，将调用此servlet。 此servlet会将JSON对象返回给调用应用程序。 然后，调用应用程序会使用从JSON对象提取的值填充自适应表单。

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

以下代码是由自适应表单引用的客户端库的一部分。 当用户将附件添加到自适应表单时，将触发此代码。 该代码会对servlet进行GET调用，并在请求参数中传递附件的路径。 然后，使用从servlet调用接收的数据填充自适应表单。

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>此包中包含的自适应表单是使用AEM Forms 6.4构建的。如果您打算在AEM Forms 6.3环境中使用此包，请在AEM Form 6.3中创建自适应表单

第12行 — 用于获取服务解析程序的自定义代码。 此捆绑包作为此文章资源的一部分包含。

第23行 — 调用DocumentServices extractBarCode方法以使用解码的数据填充JSON对象

要在您的系统上运行此命令，请执行以下步骤：

1. [下载BarcodeService.zip](assets/barcodeservice.zip) 并使用包管理器导入AEM
1. [下载并安装自定义DocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [下载并安装DevelopingWithServiceUser捆绑包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下载示例PDF表单](assets/barcode.pdf)
1. 将浏览器指向 [自适应表单示例](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. 上传提供的示例PDF
1. 您应该会看到表单中填充了数据
