---
title: 在AEM Forms中使用Forms服务呈现交互式PDF
description: 在AEM Forms中使用Forms服务API呈现交互式PDF
feature: Forms Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# 在AEM Forms中使用Forms服务呈现交互式PDF

在AEM Forms中使用Forms服务API呈现交互式PDF

在本文中，我们将介绍以下服务

* FormsService — 这是一项功能非常广泛的服务，允许您从PDF文件中导出/导入数据，并通过将xml数据合并到xdp模板中来生成交互式pdf

此处列出了适用于AEM Forms API的官方[javadoc](https://helpx.adobe.com/cn/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

以下代码段使用FormsService的renderPDFForm操作渲染交互式pdf。 schengen.xdp是用于合并xml数据的模板。

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

Line2-4：创建PDFFormRenderOptions并设置其属性

第7行：使用FormsService的renderPDFForm服务操作生成交互式PDF

第11行：将生成的交互式pdf返回到调用应用程序

**在系统上测试示例包**
1. [下载并安装DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [使用Felix Web控制台下载并安装DocumentServices示例包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用AEM包管理器下载并安装包](assets/downloadinteractivepdffrommobileform.zip)

1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Adobe Granite CSRF筛选器
1. 在排除的部分中添加以下路径并保存
1. /bin/generateinteractivepdf
1. 搜索&#x200B;_Apache Sling服务用户映射器服务_，然后单击以打开属性
   1. 单击&#x200B;*+*&#x200B;图标（加号）以添加以下服务映射
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. 单击“保存”
1. [打开移动设备表单](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填写几个字段，然后单击&#x200B;***下载并填写....***&#x200B;按钮
1. 应将交互式pdf下载到您的本地系统


示例包包含与移动设备表单关联的自定义配置文件。 请浏览[customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp)文件。 此jsp从移动设备表单中提取数据，并对挂载在&#x200B;***/bin/generateinteractivepdf***&#x200B;路径上的servlet发出POST请求。 此servlet将交互式pdf返回到调用应用程序。 customtoolbar.jsp中的代码然后将文件下载到本地系统
