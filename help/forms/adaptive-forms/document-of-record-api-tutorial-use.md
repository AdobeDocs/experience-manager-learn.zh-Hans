---
title: 使用API通过AEM Forms生成记录文档
description: 以编程方式生成记录文档(DOR)
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 1%

---

# 在AEM Forms中使用API生成记录文档 {#using-api-to-generate-document-of-record-with-aem-forms}

以编程方式生成记录文档(DOR)

本文说明了 `com.adobe.aemds.guide.addon.dor.DoRService API` 生成 **记录文档** 以编程方式。 [记录文档](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 是自适应表单中捕获的数据的PDF版本。

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
1. 确保已安装并启动作为 [创建服务用户文章](service-user-tutorial-develop.md)
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Apache Sling服务用户映射器服务
1. 确保输入以下条目 _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ 在“服务映射”部分
1. [打开表单](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填写表单并单击“ ViewPDF”
1. 您应会在浏览器的新选项卡中看到DOR


**疑难解答提示**

PDF未显示在新的浏览器选项卡中：

1. 确保未阻止浏览器中的弹出窗口
1. 使您已执行此中列出的步骤 [文章](service-user-tutorial-develop.md)
1. 确保“DevelopingWithServiceUser”包位于 *活动状态*
1. 确保系统用户“ data ”对以下节点具有读取、修改和创建权限 `/content/usergenerated/content/aemformsenablement`
