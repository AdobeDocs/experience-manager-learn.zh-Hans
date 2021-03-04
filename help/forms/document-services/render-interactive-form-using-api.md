---
title: 在AEM Forms中使用输出和Forms服务进行开发
seo-title: 在AEM Forms中使用输出和Forms服务进行开发
description: 在AEM Forms中使用输出和Forms Service API
seo-description: 在AEM Forms中使用输出和Forms Service API
feature: 表单服务
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 开发
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 2%

---


# 使用AEM Forms中的Forms服务渲染交互式PDF

使用AEM Forms中的Forms Service API渲染交互式PDF

在本文中，我们将看到以下服务

* FormsService — 这是一项用途非常广的服务，它允许您从PDF文件中导出/导入数据，还可以通过将xml数据合并到xdp模板中来生成交互式pdf

此处](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)列出了AEM Forms API的正式javadoc[

以下代码段使用FormsService的renderPDFForm操作来渲染交互式pdf。 senga.xdp是用于合并xml数据的模板。

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
1. [使用AEM包管理器下载并安装包](assets/downloadinteractivepdffrommobileform.zip)



1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. Adobe花岗岩CSRF滤波器的研究
1. 在被排除的部分中添加以下路径并保存
1. /bin/generateinteractivepdf
1. [打开移动表单](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填写几个字段，然后单击&#x200B;***下载并填写…….*** 按钮
1. 应将交互式pdf下载到您的本地系统


示例包中包含与移动表单关联的自定义用户档案。 请浏览[customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp)文件。 此jsp从移动表单中提取POST，并向装载在&#x200B;***/bin/generateinteractivepdf***&#x200B;路径上的servlet发出数据请求。 Servlet将交互式pdf返回给调用应用程序。 然后，customtoolbar.jsp中的代码将文件下载到您的本地系统


