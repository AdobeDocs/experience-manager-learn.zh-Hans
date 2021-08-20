---
title: 使用API通过AEM Forms生成记录文档
description: 以编程方式生成记录文档(DOR)
feature: 自适应表单
version: 6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 3%

---


# 在AEM Forms中使用API生成记录文档 {#using-api-to-generate-document-of-record-with-aem-forms}

以编程方式生成记录文档(DOR)

本文说明了如何使用`com.adobe.aemds.guide.addon.dor.DoRService API`以编程方式生成&#x200B;**记录文档**。 [文档记](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 录在自适应表单中捕获的PDF版本数据。

1. 以下是代码片段。 第一线获得DOR服务。
1. 设置DoROptions。
1. 调用DoRService的呈现方法并将DoROptions对象传递到呈现方法

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

要在本地系统上尝试此操作，请执行以下步骤

1. [使用包管理器下载并安装文章资产](assets/dor-with-api.zip)
1. 确保已安装并启动作为[创建服务用户文章](service-user-tutorial-develop.md)一部分提供的DevelopingWithServiceUser包
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Apache Sling服务用户映射器服务
1. 确保在“服务映射”部分中输入以下条目&#x200B;_DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_
1. [打开表单](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填写表单并单击“查看PDF ”
1. 您应会在浏览器的新选项卡中看到DOR


**疑难解答提示**

PDF未显示在新的浏览器选项卡中：

1. 确保未阻止浏览器中的弹出窗口
1. 使您已执行本[文章](service-user-tutorial-develop.md)中所述的步骤
1. 确保“DevelopingWithServiceUser”包处于&#x200B;*active状态*
1. 确保系统用户“ data ”对以下节点`/content/usergenerated/content/aemformsenablement`具有读取、修改和创建权限

