---
title: 具有自适应Forms的条形码服务
seo-title: 具有自适应Forms的条形码服务
description: 使用条形码服务解码条形码并从提取的数据填充表单字段
seo-description: 使用条形码服务解码条形码并从提取的数据填充表单字段
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---


# 具有自适应Forms的条形码服务{#barcode-service-with-adaptive-forms}

本文将演示如何使用条形码服务填充自适应表单。 用例如下：

1. 用户添加带有条形码作为自适应表单附件的PDF
1. 附件的路径将发送到servlet
1. Servlet解码条形码并返回JSON格式的数据
1. 然后，使用解码的数据填充自适应表单

以下代码解码条形码并使用解码值填充JSON对象。 然后，servlet在响应调用应用程序时返回JSON对象。

您可以实时查看此功能，请访问[示例门户](https://forms.enablementadobe.com/content/samples/samples.html?query=0)并搜索条形码服务演示

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

以下是servlet代码。 当用户向自适应表单添加附件时，将调用此servlet。 servlet将JSON对象返回给调用应用程序。 调用应用程序随后将使用从JSON对象提取的值填充自适应表单。

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

以下代码是自适应表单引用的客户端库的一部分。 当用户将附件添加到自适应表单时，将触发此代码。 该代码对servlet进行GET调用，其路径为请求参数中传递的附件。 然后使用从servlet调用接收的数据填充自适应表单。

```
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
>此包中包含的自适应表单是使用AEM Forms6.4构建的。如果您打算在AEM Forms6.3环境中使用此包，请在AEM表单6.3中创建自适应表单

第12行——获取服务解析程序的自定义代码。 此捆绑包作为本文章资源的一部分提供。

第23行——调用DocumentServices extractBarCode方法，以获取用解码数据填充的JSON对象

要在您的系统上运行此程序，请按照以下步骤操作

1. [下载BarcodeService.](assets/barcodeservice.zip) zip并使用包管理器导入AEM
1. [下载并安装自定义DocumentServices捆绑](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [下载并安装DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下载范例PDF表单](assets/barcode.pdf)
1. 将浏览器指向[示例自适应表单](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. 上传提供的范例PDF
1. 您应当看到填充了数据的表单

