---
title: 利用产出和Forms服务在AEM Forms进行开发
description: 在AEM Forms中使用输出和Forms服务API
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# 利用产出和Forms服务在AEM Forms进行开发{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用输出和Forms服务API

在本文中，我们将了解以下内容

* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成扁平化的pdf。 有关更多详细信息，请参阅[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 的值。
* FormsService — 这是一项用途广泛的服务，允许您将数据从文件导出/导入PDF文件。 有关更多详细信息，请参阅 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html) 为Forms服务。


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
1. 选中“允许空”复选框。 （此设置应仅用于测试目的）有多种方法可测试示例代码。 最快、最简单的方法是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API

确保您从下拉列表http://localhost:4502/content/AemFormsSamples/exportdata.html中选择了“POST”。确保将“授权”指定为“基本身份验证”。 指定AEM Server用户名和密码导航到“Body”选项卡，并指定请求参数，如下图所示
![导出](assets/postexport.png)
然后，单击“发送”按钮

包含3个示例。 以下段落说明了何时使用输出服务或Forms服务、服务的url以及每个服务所期望的输入参数

## 合并数据和扁平化输出

* 使用输出服务将数据与xdp或pdf文档合并，以生成扁平化pdf
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **请求参数 —**

   * **xdp_or_pdf_file** :要将数据与
   * **xmlfile**:与xdp_or_pdf_file合并的xml数据文件
   * **saveLocation**:在文件系统上保存已渲染文档的位置。 例如c:\\documents\\sample.pdf

### 将数据导入PDF文件

* 使用FormsService将数据导入PDF文件
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **请求参数：**

   * **pdfile** :要将数据与
   * **xmlfile**:与pdf文件合并的xml数据文件
   * **saveLocation**:在文件系统上保存已渲染文档的位置。 例如 `c:\\outputsample.pdf`.

**从PDF文件导出数据**
* 使用FormsService从PDF文件导出数据
* **POSTUR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **请求参数：**

   * **pdfile** :要从中导出数据的pdf文件
   * **saveLocation**:在文件系统上保存导出数据的位置。 例如c:\\documents\\exported_data.xml

[您可以导入此邮递员集合以测试API](assets/document-services-postman-collection.json)
