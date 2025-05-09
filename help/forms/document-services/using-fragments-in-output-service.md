---
title: 在输出服务中使用片段
description: 生成片段驻留在crx存储库中的pdf文档
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
exl-id: d7887e2e-c2d4-4f0c-b117-ba7c41ea539a
duration: 106
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# 使用片段生成pdf文档{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我们将使用输出服务来生成使用xdp片段的pdf文件。 主xdp和片段驻留在crx存储库中。 模拟AEM中的文件系统文件夹结构很重要。 例如，如果您在xdp的片段文件夹中使用片段，则必须在AEM中的基文件夹下创建一个名为&#x200B;**fragments**&#x200B;的文件夹。 基本文件夹将包含您的基本xdp模板。 例如，如果文件系统中具有以下结构
* c：\xdptemplates — 这将包含您的基本xdp模板
* c：\xdptemplates\fragments — 此文件夹将包含片段，主模板将引用如下所示的片段
  ![fragment-xdp](assets/survey-fragment.png)。
* 文件夹xdpdocuments将在&#x200B;**片段**&#x200B;文件夹中包含您的基本模板和片段

您可以使用[表单和文档UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)创建所需的结构

以下是示例使用2个片段的文件夹结构
![表单&amp;文档](assets/fragment-folder-structure-ui.png)


* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成拼合的pdf。 有关详细信息，请参阅输出服务的[javadoc](https://helpx.adobe.com/cn/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)。 在此示例中，我们使用驻留在crx存储库中的片段。


以下代码用于在PDF文件中包含片段

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**在系统上测试示例包**

* [下载示例xdp文件并将其导入AEM](assets/xdp-templates-fragments.zip)
* [使用AEM包管理器下载并安装包](assets/using-fragments-assets.zip)
* [可以从此处下载示例xdp和片段](assets/xdptemplates.zip)

**安装包后，必须在Adobe Granite CSRF筛选器中允许列表以下URL。**

1. 请按照以下所述步骤列入允许列表上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索Adobe Granite CSRF筛选器
1. 在排除的部分中添加以下路径并保存
1. /content/AemFormsSamples/usingfragments

可通过多种方法来测试示例代码。 最快、最轻松的是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在您的系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API

确保从下拉列表中选择“POST”
http://localhost:4502/content/AemFormsSamples/usingfragments.html
确保将“Authorization”指定为“Basic Auth”。 指定AEM服务器用户名和密码
导航到“正文”选项卡，并指定请求参数，如下图所示
![导出](assets/using-fragment-postman.png)
然后单击Send按钮

[您可以导入此Postman集合以测试API](assets/usingfragments.postman_collection.json)
