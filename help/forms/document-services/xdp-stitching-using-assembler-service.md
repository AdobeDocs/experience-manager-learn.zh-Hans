---
title: 使用組合器服務的XDP拼接
description: 在AEM Forms中使用組合器服務來拼接xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# 使用組合器服務的XDP拼接

本文提供示範使用組合器服務彙整xdp檔案能力的資產。
寫入下列jsp程式碼以插入名為的子表單 **地址** 將名為address.xdp的xdp檔案讀入名為 **地址** 在master.xdp檔案中。 產生的xdp會儲存在AEM安裝的根資料夾中。

組合器服務仰賴有效的DDX檔案來說明對PDF檔案的操作。 您可參閱 [DDX參考檔案在此](assets/ddxRef.pdf).第40頁包含有關xdp彙整的資訊。

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

以下列出要將片段插入另一個xdp的DDX檔案。 DDX會插入子表單  **地址** 從address.xdp插入插入點時呼叫 **地址** 在master.xdp中。 產生的檔案命名為 **stitched.xdp** 會儲存至檔案系統。

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

若要讓此功能在您的AEM伺服器上運作

* 下載 [XDP拼接封裝](assets/xdp-stitching.zip) 至您的本機系統。
* 使用上傳和安裝套件 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* [解壓縮此zip檔案的內容](assets/xdp-and-ddx.zip) 以取得範例xdp和DDX檔案

**安裝套件後，您必須在AdobeGranite CSRF篩選中允許列出下列URL。**

1. 請依照下列步驟將上述路徑加入允許清單。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增以下路徑並儲存 `/content/AemFormsSamples/assemblerservice`
1. 搜尋「Sling查閱者篩選器」
1. 勾選「允許空白」核取方塊。 （此設定僅供測試用途）測試範常式式碼的方法有很多種。 最快捷、最輕鬆的方式就是使用Postman應用程式。 Postman可讓您向伺服器發出POST請求。 在您的系統上安裝Postman app。
啟動應用程式並輸入以下URL以測試匯出資料API http://localhost:4502/content/AemFormsSamples/assemblerservice.html

提供熒幕擷取畫面中指定的下列輸入引數。 您可以使用先前下載的範例檔案，
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>請確定您的AEM Forms安裝已完成。 您的所有套件組合都必須處於作用中狀態。
