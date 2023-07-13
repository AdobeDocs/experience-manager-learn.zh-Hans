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
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# 部署资源

以下资产/配置部署在AEM Forms发布服务器上。

* [Adobe Sign包装包](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [交互式通信模板示例](assets/waiver-interactive-communication.zip)
* [部署DevelopingWithServiceUser捆绑包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* 使用OSGi configMgr在Apache Sling服务用户映射器服务中添加以下条目
  **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service**

## 部署示例react应用程序

* [下载示例react应用程序](assets/mult-step-form1.zip)
* 将react应用程序的内容解压缩到一个新文件夹中
* 导航到文件夹并运行以下命令

```java
npm install
npm start
```

打开EmergencyContact.js文件，更改fetch方法中的URL以匹配您的环境。


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

要启用从REACT应用程序对AEM端点的POST调用，您需要在AdobeGranite跨源资源共享策略配置的允许的原始项字段中指定相应的条目。

![cors-setting](assets/cors-settings.png)
