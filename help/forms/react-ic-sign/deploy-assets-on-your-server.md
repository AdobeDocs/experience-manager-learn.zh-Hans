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
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# 部署资产

在AEM Forms发布服务器上部署了以下资产/配置。

* [Adobe Sign包装包](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [交互式通信模板示例](assets/waiver-interactive-communication.zip)
* [部署DevelopingWithServiceUser包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* 使用OSGi configMgr在Apache Sling服务用户映射器服务中添加以下条目
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [可以从此处下载示例React应用程序代码](assets/src.zip)



需要在本地环境中部署示例react应用程序

您必须更改端点URL才能与环境匹配。 打开EmergencyContact.js文件，并在获取方法中更改URL

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

要启用从REACT应用程序对AEM端点进行POST调用，您需要在AdobeGranite跨源资源共享策略配置的允许的源字段中指定相应的实体

![cors设置](assets/cors-settings.png)



