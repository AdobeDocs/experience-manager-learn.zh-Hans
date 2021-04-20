---
title: 使用API通过AEM Forms生成记录文档
seo-title: 使用API通过AEM Forms生成记录文档
description: 以编程方式生成记录文档(DOR)
seo-description: 使用API通过AEM Forms生成记录文档
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---


# 使用API在AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}中生成记录文档

以编程方式生成记录文档(DOR)

本文说明了使用`com.adobe.aemds.guide.addon.dor.DoRService API`以编程方式生成&#x200B;**记录**&#x200B;的文档。 [文档录](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 制在自适应表单中捕获的数据的PDF版本。

1. 以下是代码片断。 第一行获得DOR服务。
1. 设置DoROptions。
1. 调用DoRService的渲染方法并将DoROptions对象传递给渲染方法

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

要在本地系统上尝试，请执行以下步骤

1. [使用包管理器下载并安装文章资源](assets/dor-with-api.zip)
1. 确保已安装并启动作为[创建服务用户文章](service-user-tutorial-develop.md)的一部分提供的DevelopingWithServiceUser包
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Apache Sling Service用户映射器服务
1. 确保在“服务映射”部分中输入以下条目&#x200B;_DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_
1. [打开表单](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填写表单并单击“ 视图 PDF ”
1. 您应在浏览器的新选项卡中看到DOR


**疑难解答提示**

PDF不显示在新的浏览器选项卡中：

1. 确保您没有阻止浏览器中的弹出窗口
1. 让您遵循本[文章](service-user-tutorial-develop.md)中概述的步骤
1. 确保“DevelopingWithServiceUser”包处于&#x200B;*活动状态*
1. 确保系统用户“ data ”对以下节点`/content/usergenerated/content/aemformsenablement`具有“读取”、“修改”和“创建”权限

