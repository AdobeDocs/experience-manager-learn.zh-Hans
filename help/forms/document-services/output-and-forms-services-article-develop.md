---
title: 以产出和Forms服务发展AEM Forms
seo-title: 以产出和Forms服务发展AEM Forms
description: 在AEM Forms使用输出和Forms服务API
seo-description: 在AEM Forms使用输出和Forms服务API
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---


# 以产出和Forms服务发展AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms使用输出和Forms服务API

在本文中，我们将看到以下内容：

* 输出服务——通常，此服务用于将xml数据与xdp模板或pdf合并，以生成拼合的pdf
* FormsService —— 这是一项功能多样的服务，允许您从PDF文件导出／导入数据

此处列出AEM FormsAPI的官方javadoc [。](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

以下代码片断从PDF文件导出数据

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行从请求中提取pdf文件

Line2从请求中提取saveLocation

第5行获得FormsService的持有权

第6行从PDF文件导出xmlData

**在系统上测试示例包**

[使用AEM包管理器下载并安装包](assets/outputandformsservice.zip)




**安装包后，您必须在允许列表AdobeGranite CSRF过滤器中以下URL。**

1. 请按照以下步骤允许列表上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. Adobe花岗石CSRF滤波器的研究
1. 在被排除的部分中添加以下3个路径并保存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜索“Sling推荐人过滤器”
1. 选中“允许空”复选框。 （此设置应仅用于测试目的）有多种方法可测试示例代码。 最快、最简单的方法是使用Postman应用程序。 邮递员允许您向服务器发出POST请求。 在您的系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API

确保您从下拉POST中选择了“列表”http://localhost:4502/content/AemFormsSamples/exportdata.html请确保将“授权”指定为“基本身份验证”。 指定AEM Server用户名和口令导航到“正文”选项卡并指定请求参数，如下面的导出图![像](assets/postexport.png)所示，然后单击“发送”按钮

软件包包含3个范例。 以下各段说明何时使用输出服务或Forms服务、服务的url、每个服务所需的输入参数

**合并数据和拼合输出：**

* 使用Output Service将数据与xdp或pdf文档合并，生成拼合的pdf
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **请求参数-**

   * xdp_or_pdf_file:要将数据与
   * xmlfile:将与xdp_or_pdf_file合并的xml数据文件
   * saveLocation:在文件系统上保存呈现文档的位置

**将数据导入PDF文件：**
* 使用FormsService将数据导入PDF文件
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **请求参数：**

   * pdf文件：要将数据与
   * xmlfile:将与pdf文件合并的xml数据文件
   * saveLocation:在文件系统上保存呈现文档的位置。 例如c:\\\outputsample.pdf。

**从PDF文件导出数据**
* 使用FormsService从PDF文件导出数据
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **请求参数：**

   * pdf文件：要从中导出数据的pdf文件
   * saveLocation:在文件系统上保存导出数据的位置

[您可以导入此邮递员集合以测试API](assets/document-services-postman-collection.json)

