---
title: 使用API通过AEM Forms生成记录文档
description: 以编程方式生成记录文档(DOR)
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 1%

---

# 在AEM Forms中使用API生成记录文档 {#using-api-to-generate-document-of-record-with-aem-forms}

以编程方式生成记录文档(DOR)

本文说明了 `com.adobe.aemds.guide.addon.dor.DoRService API` 生成 **记录文档** 以编程方式。 [记录文档](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) 是在自适应表单中捕获的数据的PDF版本。

1. 以下是代码片段。 第一行获得DOR服务。
1. 设置DoROptions。
1. 调用DoRService的渲染方法，并将DoROptions对象传递给渲染方法

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
```

要在本地系统上尝试此操作，请执行以下步骤

1. [使用包管理器下载并安装文章资源](assets/dor-with-api.zip)
1. 确保已安装并启动作为一部分提供的DevelopingWithServiceUser捆绑包 [创建服务用户文章](service-user-tutorial-develop.md)
1. [登录configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Apache Sling服务用户映射器服务
1. 确保输入以下内容 _DevelopingWithServiceUser.core：getformsresourceresolver=fd-service_ 在“服务映射”部分中
1. [打开窗体](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. 填写表单并单击“查看PDF”
1. 您应该会在浏览器的新选项卡中看到DOR


**疑难解答提示**

PDF不会显示在新的“浏览器”选项卡中：

1. 确保没有阻止浏览器中的弹出窗口
1. 确保您以管理员身份启动AEM服务器（至少在windows上）
1. 确保“DevelopingWithServiceUser”捆绑包位于 *活动状态*
1. [确保系统用户](http://localhost:4502/useradmin) “ fd-service”在以下节点上具有“读取”、“修改”和“创建”权限 `/content/usergenerated/content/aemformsenablement`
