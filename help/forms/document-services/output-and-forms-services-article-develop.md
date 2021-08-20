---
title: 利用产出和Forms服务在AEM Forms进行开发
description: 在AEM Forms中使用输出和Forms服务API
feature: 输出服务
version: 6.4,6.5
topic: 开发
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# 利用产出和Forms服务在AEM Forms进行开发{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用输出和Forms服务API

在本文中，我们将了解以下内容

* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成扁平化pdf。 有关更多详细信息，请参阅输出服务的[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)。
* FormsService — 这是一项用途广泛的服务，允许您将数据从PDF文件导出/导入PDF文件。 有关详细信息，请参阅Forms服务的[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html)。


以下代码片段从PDF文件导出数据

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行从请求中提取文件

Line2从请求中提取saveLocation

第5行获得FormsService的保留

第6行从PDF文件导出xmlData

**在系统上测试示例包**

[使用AEM包管理器下载和安装包](assets/outputandformsservice.zip)




**安装包后，必须在Granite CSRF过允许列表滤器中Adobe以下URL。**

1. 请按照下面提到的步骤来允许列表上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索AdobeGranite CSRF过滤器
1. 在排除的部分中添加以下3个路径并保存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜索“Sling反向链接过滤器”
1. 选中“允许空”复选框。 （此设置应仅用于测试目的）
有多种方法可测试示例代码。 最快、最简单的方法是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API

确保已从下拉列表中选择“POST”
http://localhost:4502/content/AemFormsSamples/exportdata.html
确保将“授权”指定为“基本身份验证”。 指定AEM Server用户名和密码
导航到“Body”选项卡，并指定请求参数，如下图所示
![export](assets/postexport.png)
然后，单击“发送”按钮

包含3个示例。 以下各段说明了何时使用输出服务或Forms服务、服务的url以及每个服务所期望的输入参数

**合并数据和扁平化输出：**

* 使用输出服务将数据与xdp或pdf文档合并，以生成扁平化pdf
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **请求参数 —**

   * xdp_or_pdf_file :要将数据与
   * xmlfile:将与xdp_or_pdf_file合并的xml数据文件
   * saveLocation:在文件系统上保存已渲染文档的位置

**将数据导入PDF文件：**
* 使用FormsService将数据导入PDF文件
* **POSTURL**  - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **请求参数：**

   * 配置文件：要将数据与
   * xmlfile:将与pdf文件合并的xml数据文件
   * saveLocation:在文件系统上保存已渲染文档的位置。 例如c:\\\outputsample.pdf。

**从PDF文件导出数据**
* 使用FormsService从PDF文件导出数据
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **请求参数：**

   * 配置文件：要从中导出数据的pdf文件
   * saveLocation:在文件系统上保存导出数据的位置

[您可以导入此邮递员集合以测试API](assets/document-services-postman-collection.json)

