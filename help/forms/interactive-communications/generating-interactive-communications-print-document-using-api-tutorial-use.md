---
title: 使用监视文件夹机制为打印渠道生成交互式通信文档
description: 使用watched文件夹生成打印渠道文档
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# 使用监视文件夹机制为打印渠道生成交互式通信文档

设计和测试打印渠道文档后，通常需要通过进行REST调用来生成文档，或使用监视文件夹机制生成打印文档。

本文介绍了使用watched文件夹机制生成打印渠道文档的用例。

将文件放入watched文件夹时，会执行与watched文件夹关联的脚本。 下面的文章中对此脚本进行了说明。

放入watched文件夹中的文件具有以下结构。 该代码将为XML文档中列出的所有帐号生成语句。

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

下面列出的代码执行以下操作：

第1行 — InteractiveCommunicationsDocument的路径

第15-20行：从拖放到watched文件夹中的XML文档获取帐号列表

行24 - 25：获取与文档关联的PrintChannelService和Print Channel。

第30行：将accountnumber作为关键元素传递给表单数据模型。

第32-36行：为要生成的文档设置数据选项。

第38行：渲染文档。

第39-40行 — 将生成的文档保存到文件系统。

表单数据模型的REST端点需要将ID作为输入参数。 此id映射到名为accountnumber的请求属性，如下面的屏幕快照所示。

![请求属性](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**要在本地系统上对此进行测试，请按照以下说明操作：**

* 按照本说明设置Tomcat [文章。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat具有生成样本数据的war文件。
* 设置服务（即系统用户），如中所述 [文章](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
确保此系统用户具有下列节点的读取权限。 授予权限登录至 [用户管理员](https://localhost:4502/useradmin) 和搜索系统用户“data”，并通过tab键转到“权限”选项卡，为以下节点授予读取权限
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* 使用包管理器将以下包导入AEM。 此包包含以下内容：


* [交互式通信文档示例](assets/retirementstatementprint.zip)
* [观察文件夹脚本](assets/printchanneldocumentusingwatchedfolder.zip)
* [数据源配置](assets/datasource.zip)

* 打开/etc/fd/watchfolder/scripts/PrintPDF.ecma文件。 确保第1行中interactiveCommunicationsDocument的路径指向要打印的正确文档

* 根据您的首选项第2行修改saveLocation

* 创建包含以下内容的accountnumbers.xml文件

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* 将accountnumbers.xml拖放到C:\RenderPrintChannel\input文件夹中。

* 生成的PDF文件将写入ecma脚本中指定的saveLocation。

>[!NOTE]
>
>如果您计划在非Windows操作系统上使用此功能，请导航至
>
>/etc/fd/watchfolder /config/PrintChannelDocument ，并根据您的首选项更改folderPath
