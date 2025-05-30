---
title: 在AEM Forms中使用输出和Forms服务进行开发
description: 了解如何在AEM Forms中使用Output和Forms Service API进行开发。
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
duration: 122
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---

# 在AEM Forms中使用输出和Forms服务进行开发{#developing-with-output-and-forms-services-in-aem-forms}

了解如何在AEM Forms中使用Output和Forms Service API进行开发。

在本文中，我们将看一看以下内容

* [输出服务](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成拼合的pdf。
* [FormsService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) — 这是一项功能非常广泛的服务，允许您将xdp渲染为pdf，并将数据从PDF文件中导出/导入。


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

第6行从PDF文件导出xmlData

**在系统上测试示例包**

[使用AEM包管理器下载并安装包](assets/using-output-and-form-service-api.zip)




**安装包后，必须在Adobe Granite CSRF筛选器中允许列表以下URL。**

1. 请按照以下所述步骤列入允许列表上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Adobe Granite CSRF筛选器
1. 在排除的部分中添加以下3个路径并保存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. 搜索“Sling引用过滤器”
1. 选中“允许为空”复选框。 （此设置仅用于测试目的）

## 测试样本

可通过多种方法来测试示例代码。 最快、最轻松的是使用Postman应用程序。 Postman允许您向服务器发出POST请求。

* 在您的系统上安装Postman应用程序。
* 启动应用程序并输入相应的URL
* 确保从下拉列表中选择“POST”
* 确保将“Authorization”指定为“Basic Auth”。 指定AEM服务器用户名和密码
* 在body选项卡中指定请求参数
* 单击发送按钮

此软件包包含4个示例。 以下段落说明了何时使用输出服务或Forms服务、服务的URL以及每个服务期望的输入参数

## 使用OutputService将数据与xdp模板合并

* 使用输出服务将数据与xdp或pdf文档合并以生成拼合的pdf
* **发布URL**： http://localhost:4502/content/AemFormsSamples/outputservice.html
* **请求参数 —**

   * **xdp_or_pdf_file** ：要与数据合并的xdp或pdf文件
   * **xmlfile**：与xdp_or_pdf_file合并的xml数据文件
   * **saveLocation**：将渲染文档保存在文件系统中的位置。 例如c：\\documents\\sample.pdf

### 使用FormsService API

#### 导入数据

* 使用FormsService importData将数据导入PDF文件
* **发布URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html

* **请求参数：**

   * **pdf文件** ：要与它合并数据的pdf文件
   * **xmlfile**：与pdf文件合并的xml数据文件
   * **saveLocation**：将渲染文档保存在文件系统中的位置。 例如 `c:\\outputsample.pdf`。

#### 导出数据

* 使用FormsService exportData API从PDF文件导出数据
* **发布URL** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **请求参数：**

   * **pdf文件** ：要从中导出数据的pdf文件
   * **saveLocation**：在文件系统中保存导出数据的位置。 例如c：\\documents\\exported_data.xml

#### 渲染XDP

* 将XDP模板渲染为静态/动态pdf
* 使用FormsService renderPDFForm API将xdp模板渲染为PDF
* **发布URL** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* 请求参数：
   * xdpName：要呈现为pdf的xdp文件的名称

[您可以导入此Postman集合以测试API](assets/UsingDocumentServicesInAEMForms.postman_collection.json)

