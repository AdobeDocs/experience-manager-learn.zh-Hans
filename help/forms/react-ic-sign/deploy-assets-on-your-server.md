---
title: 在您的伺服器上部署範例資產
description: 取得在本機伺服器上運作的使用案例
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# 部署資產

下列資產/設定部署在AEM Forms發佈伺服器上。

* [Adobe Sign包裝函式套件](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [互動式通訊範本範例](assets/waiver-interactive-communication.zip)
* [部署DevelopingWithServiceUser套裝](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* 使用OSGi configMgr在Apache Sling服務使用者對應程式服務中新增以下專案
   **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service**
* [可從這裡下載React應用程式程式碼範例](assets/src.zip)



範例react應用程式需要部署在本機環境中

您必須變更端點URL以符合您的環境。 開啟EmergencyContact.js檔案，並變更擷取方法中的URL

```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

若要啟用從REACT應用程式對AEM端點進行POST呼叫，您需要在AdobeGranite跨原始資源共用原則設定的「允許的原始項」欄位中指定適當的專案

![cors — 設定](assets/cors-settings.png)
