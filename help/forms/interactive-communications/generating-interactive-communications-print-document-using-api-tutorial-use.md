---
title: 使用监视文件夹机制生成用于打印渠道的交互式通信文档
seo-title: 使用监视文件夹机制生成用于打印渠道的交互式通信文档
description: 使用监视文件夹生成打印渠道文档
seo-description: 使用监视文件夹生成打印渠道文档
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 1%

---


# 使用监视文件夹机制生成用于打印渠道的交互式通信文档

设计并测试了打印渠道文档后，您通常需要通过发出REST调用来生成文档，或使用监视文件夹机制生成打印文档。

本文说明了使用监视文件夹机制生成打印渠道文档的用例。

将文件放入监视文件夹时，将执行与监视文件夹关联的脚本。 此脚本在以下文章中进行说明。

放入监视文件夹的文件具有以下结构。 代码将为XML文档中列出的所有帐号生成语句。

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

以下代码清单如下所示：

第1行 — InteractiveCommunicationsDocument的路径

第15-20行：从XML文档获取帐号的列表，并将其放入监视的文件夹

第24-25行：获取与文档关联的PrintChannelService和打印渠道。

第30行：将帐号作为关键元素传递到表单数据模型。

第32-36行：为要生成的文档设置数据选项。

第38行：渲染文档。

第39-40行 — 将生成的文档保存到文件系统。

表单数据模型的REST端点需要一个id作为输入参数。 此id将映射到名为accountnumber的请求属性，如以下屏幕截图所示。

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


**要在本地系统上测试此功能，请按照以下说明操作：**

* 按照本[文章中的说明设置Tomcat。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat具有生成样本数据的war文件。
* 按本[文章](/help/forms/adaptive-forms/service-user-tutorial-develop.md)中所述设置服务aka系统用户。
确保此系统用户对以下节点具有读取权限。 要授予用户登录[用户admin](https://localhost:4502/useradmin)的权限，并搜索系统用户“data”，并按下面节点的Tab键时授予读取权限，请执行以下操作
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* 使用包管理器将以下包导入AEM。 此包包含以下内容：


* [交互通信示例文档](assets/retirementstatementprint.zip)
* [监视文件夹脚本](assets/printchanneldocumentusingwatchedfolder.zip)
* [数据源配置](assets/datasource.zip)

* 打开/etc/fd/watchfolder/scripts/PrintPDF.ecma文件。 确保第1行中的interactiveCommunicationsDocument的路径指向要打印的正确文档

* 根据第2行中您的首选项修改saveLocation

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


* 将accountnumbers.xml放入C:\RenderPrintChannel\input folder目录中。

* 生成的PDF文件将按照ecma脚本中的指定写入saveLocation。

>[!NOTE]
>
>如果您计划在非Windows操作系统上使用它，请导航到
>
>/etc/fd/watchfolder /config/PrintChannelDocument并根据您的首选项更改folderPath

