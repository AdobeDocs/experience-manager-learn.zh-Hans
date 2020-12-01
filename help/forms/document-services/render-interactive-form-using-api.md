---
title: 以产出和Forms服务发展AEM Forms
seo-title: 以产出和Forms服务发展AEM Forms
description: 在AEM Forms使用输出和Forms服务API
seo-description: 在AEM Forms使用输出和Forms服务API
feature: forms-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 1%

---


# 使用AEM Forms的Forms服务渲染交互式PDF

使用AEM Forms的Forms服务API渲染交互式PDF

在本文中，我们将看到以下服务

* FormsService —— 这是一项功能非常广泛的服务，它允许您从PDF文件导出／导入数据，还可以通过将xml数据合并到xdp模板中来生成交互式pdf

此处[列出了AEM FormsAPI的正式javadoc](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

以下代码片断使用FormsService的renderPDFForm操作来渲染交互式pdf。 申根。xdp是用于合并xml数据的模板。

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
1. Adobe花岗石CSRF滤波器的研究
1. 在被排除的部分中添加以下路径并保存
1. /bin/generateinteractivepdf
1. [打开移动表单](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填写几个字段，然后单击&#x200B;***下载并填写…….*** 按钮
1. 应将交互式pdf下载到您的本地系统


示例包包含与移动表单关联的自定义用户档案。 请浏览[customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp)文件。 此jsp从移动表单中提取数据，并向装载在&#x200B;***/bin/generateinteractivepdf***&#x200B;路径上的servlet发出POST请求。 Servlet将交互式pdf返回给调用应用程序。 customtoolbar.jsp中的代码随后会将文件下载到本地系统


