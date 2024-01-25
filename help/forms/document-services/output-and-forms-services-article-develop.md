---
title: 在AEM Forms中使用输出和Forms服务进行开发
description: 在AEM Forms中使用Output和Forms服务API
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
duration: 138
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---

# 在AEM Forms中使用输出和Forms服务进行开发{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用Output和Forms服务API

在本文中，我们将看一看以下内容

* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成拼合的pdf。 有关更多详细信息，请参阅此 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 用于Output服务
* FormsService — 这是一项非常通用的服务，允许您从PDF文件中导出/导入数据。 有关更多详细信息，请参阅此 [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) 用于Forms服务。


以下代码片段从PDF文件中导出数据

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行从请求中提取PDF文件

Line2从请求中提取saveLocation

第5行获取FormsService

第6行从PDF文件中导出xmlData

**在系统上测试示例包**

[使用AEM包管理器下载并安装包](assets/outputandformsservice.zip)




**安装包后，必须在AdobeGranite CSRF过滤器中允许列表以下URL。**

1. 请按照以下所述步骤列入允许列表上述路径。
1. [登录configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索AdobeGranite CSRF筛选器
1. 在排除的部分中添加以下3个路径并保存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜索“Sling引用过滤器”
1. 选中“允许为空”复选框。 （此设置仅用于测试目的）有多种方法来测试示例代码。 最快、最轻松的是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在您的系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API

确保从下拉列表中选择“POST” http://localhost:4502/content/AemFormsSamples/exportdata.html确保将“授权”指定为“基本身份验证”。 指定AEM Server用户名和密码。导航到“正文”选项卡，然后指定请求参数，如下图所示
![导出](assets/postexport.png)
然后单击Send按钮

此软件包包含3个示例。 以下段落说明了何时使用输出服务或Forms服务、服务的URL以及每个服务期望的输入参数

## 合并数据并拼合输出

* 使用输出服务将数据与xdp或pdf文档合并以生成拼合的pdf
* **POSTURL**： http://localhost:4502/content/AemFormsSamples/outputservice.html
* **请求参数 —**

   * **xdp_or_pdf_file** ：要与数据合并的xdp或pdf文件
   * **xmlfile**：与xdp_or_pdf_file合并的xml数据文件
   * **saveLocation**：将渲染文档保存在文件系统中的位置。 例如c：\\documents\\sample.pdf

### 将数据导入PDF文件

* 使用FormsService将数据导入PDF文件
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **请求参数：**

   * **pdf** ：要与数据合并的pdf文件
   * **xmlfile**：与pdf文件合并的xml数据文件
   * **saveLocation**：将渲染文档保存在文件系统中的位置。 例如 `c:\\outputsample.pdf`。

**从PDF文件导出数据**
* 使用FormsService从PDF文件中导出数据
* **POSTUR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **请求参数：**

   * **pdf** ：要从中导出数据的pdf文件
   * **saveLocation**：将导出数据保存在文件系统中的位置。 例如c：\\documents\\exported_data.xml

[您可以导入此Postman集合以测试API](assets/document-services-postman-collection.json)
