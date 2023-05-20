---
title: 使用監看資料夾機製為列印頻道產生互動式通訊檔案
seo-title: Generating Interactive Communications Document for print channel using watch folder mechanism
description: 使用watched資料夾產生列印管道檔案
seo-description: Use watched folder to generate print channel documents
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 0%

---

# 使用監看資料夾機製為列印頻道產生互動式通訊檔案

設計和測試列印管道檔案後，您通常需要透過進行REST呼叫來產生檔案，或使用監看資料夾機制產生列印檔案。

本文說明使用Watched資料夾機制產生列印管道檔案的使用案例。

將檔案拖放到watched資料夾中時，會執行與watched資料夾相關聯的指令碼。 下方的文章將說明此指令碼。

放入watched資料夾中的檔案具有下列結構。 程式碼會為XML檔案中列出的所有帳號產生陳述式。

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

以下列出的程式碼會執行下列動作：

第1行 — InteractiveCommunicationsDocument的路徑

第15到20行：從XML檔案取得accountnumbers清單，並放入watched資料夾

第24-25行：取得與檔案相關的PrintChannelService和Print Channel。

第30行：將accountnumber作為關鍵元素傳遞至表單資料模型。

第32-36行：為要產生的檔案設定資料選項。

第38行：轉譯檔案。

第39-40行 — 將產生的檔案儲存到檔案系統。

表單資料模型的REST端點預期ID為輸入引數。 此id對應至名為accountnumber的請求屬性，如下方熒幕擷圖所示。

![requestattribute](assets/requestattributeprintchannel.gif)

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


**若要在本機系統上測試此功能，請遵循下列指示：**

* 依照以下說明設定Tomcat [文章。](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat有產生範例資料的war檔案。
* 設定服務（亦稱為系統使用者），如以下所述 [文章](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
請確定此系統使用者擁有下列節點的讀取許可權。 將許可權登入到的方式 [使用者管理員](https://localhost:4502/useradmin) 和搜尋系統使用者「資料」，並將Tab鍵移至「許可權」索引標籤，以授與下列節點的讀取許可權
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* 使用封裝管理員將下列封裝匯入AEM。 此套件包含下列專案：


* [互動式通訊檔案範例](assets/retirementstatementprint.zip)
* [Watched資料夾指令碼](assets/printchanneldocumentusingwatchedfolder.zip)
* [数据源配置](assets/datasource.zip)

* 開啟/etc/fd/watchfolder/scripts/PrintPDF.ecma檔案。 確定第1行中interactiveCommunicationsDocument的路徑指向您要列印的正確檔案

* 根據您的偏好設定，在第2行修改saveLocation

* 建立包含以下內容的accountnumbers.xml檔案

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


* 將accountnumbers.xml拖放至C:\RenderPrintChannel\input資料夾。

* 產生的PDF檔案會寫入到ecma指令碼中指定的saveLocation。

>[!NOTE]
>
>如果您打算在非視窗作業系統上使用此功能，請瀏覽至
>
>/etc/fd/watchfolder /config/PrintChannelDocument並根據您的偏好變更folderPath
