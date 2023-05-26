---
title: 在服务器上部署示例资产
description: 获取在本地服务器上工作的用例
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

# 部署资源

以下资产/配置部署在AEM Forms发布服务器上。

* [Adobe Sign包装包](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [交互式通信模板示例](assets/waiver-interactive-communication.zip)
* [部署DevelopingWithServiceUser捆绑包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* 使用OSGi configMgr在Apache Sling服务用户映射器服务中添加以下条目
   **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service**
* [可以从此处下载示例React应用程序代码](assets/src.zip)



需要在本地环境中部署示例react应用程序

您必须更改端点URL以匹配您的环境。 打开EmergencyContact.js文件并在fetch方法中更改URL

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

要启用从REACT应用程序对AEM端点的POST调用，您需要在AdobeGranite跨源资源共享策略配置的允许的原始项字段中指定相应的条目

![cors-setting](assets/cors-settings.png)
