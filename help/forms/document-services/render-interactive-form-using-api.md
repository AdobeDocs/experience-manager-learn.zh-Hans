---
title: 在AEM Forms中使用Forms服务渲染交互式PDF
description: 在AEM Forms中使用Forms服务API渲染交互式PDF
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 在AEM Forms中使用Forms服务渲染交互式PDF

在AEM Forms中使用Forms服务API渲染交互式PDF

在本文中，我们将了解以下服务

* FormsService — 这是一项用途广泛的服务，它允许您将数据从导出/导入PDF文件，还可通过将xml数据合并到xdp模板中来生成交互式pdf

列出了AEM Forms API的正式javadoc [此处](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

以下代码片段使用FormsService的renderPDFForm操作呈现交互式pdf。 申根.xdp是用于合并xml数据的模板。

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

第1行：包含xdp模板的文件夹的位置

第2-4行：创建PDFFormRenderOptions并设置其属性

第7行：使用FormsService的renderPDFForm服务操作生成交互式PDF

第11行：将生成的交互式pdf返回给调用应用程序

**在系统上测试示例包**
1. [使用Felix Web控制台下载并安装DocumentServices示例包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用AEM包管理器下载和安装包](assets/downloadinteractivepdffrommobileform.zip)



1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索AdobeGranite CSRF过滤器
1. 在排除的部分中添加以下路径并保存
1. /bin/generateinteractivepdf
1. [打开移动设备表单](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填写几个字段，然后单击 ***下载并填写…….*** 按钮
1. 应将交互式pdf下载到您的本地系统


示例包包含与Mobile表单关联的自定义配置文件。 请浏览 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 文件。 此jsp从移动表单中提取数据，并向装载在上的Servlet发出POST请求 ***/bin/generateinteractivepdf*** 路径。 Servlet将交互式PDF返回给调用应用程序。 customtoolbar.jsp中的代码，然后将该文件下载到您的本地系统
