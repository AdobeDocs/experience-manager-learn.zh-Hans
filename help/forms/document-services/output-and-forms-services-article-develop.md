---
title: 在AEM Forms中使用输出和Forms服务进行开发
seo-title: 在AEM Forms中使用输出和Forms服务进行开发
description: 在AEM Forms中使用输出和Forms Service API
seo-description: 在AEM Forms中使用输出和Forms Service API
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: 输出服务
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---


# 使用AEM Forms中的输出和Forms服务进行开发{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用输出和Forms Service API

在本文中，我们将看到以下内容

* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成拼合的pdf
* FormsService — 这是一项功能多样的服务，允许您从PDF文件导出/导入数据

此处](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)列出了AEM Forms API的正式javadoc[

以下代码段从PDF文件导出数据

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行从请求中提取pdfile

Line2从请求中提取saveLocation

第5行持有FormsService

第6行从PDF文件导出xmlData

**在系统上测试示例包**

[使用AEM包管理器下载并安装包](assets/outputandformsservice.zip)




**安装包后，您必须在Adobe 允许列表 Granite CSRF滤镜中以下URL。**

1. 请按照以下步骤操允许列表作上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. Adobe花岗岩CSRF滤波器的研究
1. 在被排除的部分中添加以下3个路径并保存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜索“Sling推荐人过滤器”
1. 选中“允许空”复选框。 （此设置应仅用于测试目的）
有多种方法可测试示例代码。 最方便快捷的方法是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在您的系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API

确保已从下拉POST中选择“列表”
http://localhost:4502/content/AemFormsSamples/exportdata.html
确保将“授权”指定为“基本身份验证”。 指定AEM Server用户名和密码
导航到“Body”选项卡并指定请求参数，如下图所示
![export](assets/postexport.png)
然后单击“发送”按钮

该包包含3个样本。 以下各段说明何时使用输出服务或Forms服务、服务的url以及每个服务所需的输入参数

**合并数据和拼合输出：**

* 使用Output Service将数据与xdp或pdf文档合并，生成拼合的pdf
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **请求参数 —**

   * xdp_or_pdf_file :要与
   * xmlfile:将与xdp_or_pdf_file合并的xml数据文件
   * saveLocation:将渲染的文档保存到文件系统的位置

**将数据导入PDF文件：**
* 使用FormsService将数据导入PDF文件
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **请求参数：**

   * pdf文件：要与
   * xmlfile:将与pdf文件合并的xml数据文件
   * saveLocation:在文件系统上保存渲染的文档的位置。 例如c:\\\outputsample.pdf。

**从PDF文件导出数据**
* 使用FormsService从PDF文件导出数据
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **请求参数：**

   * pdf文件：要从中导出数据的pdf文件
   * saveLocation:在文件系统上保存导出数据的位置

[您可以导入此邮递员集合以测试API](assets/document-services-postman-collection.json)

