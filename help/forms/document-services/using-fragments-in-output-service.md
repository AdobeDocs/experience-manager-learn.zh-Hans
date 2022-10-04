---
title: 在输出服务中使用片段
description: 生成PDF文档，其中片段驻留在CRX存储库中
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
source-git-commit: 747d1823ce1bc6670d1e80abcf6483ac921c0a01
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 1%

---

# 使用片段生成PDF文档{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我们将使用输出服务来使用xdp片段生成pdf文件。 主xdp和片段位于crx存储库中。 在AEM中模拟文件系统文件夹结构非常重要。 例如，如果您在xdp的片段文件夹中使用片段，则必须创建一个名为 **片段** 在AEM的基文件夹下。 基本文件夹将包含您的基本xdp模板。 例如，如果文件系统上具有以下结构
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* 文件夹xdpdocuments将包含您的基本模板和 **片段** 文件夹

您可以使用 [表单和文档ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

以下是示例xdp的文件夹结构，该结构使用2个片段
![表单&amp;文档](assets/fragment-folder-structure-ui.png)


* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成扁平化的pdf。 有关更多详细信息，请参阅 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 的值。 在此示例中，我们使用的是驻留在crx存储库中的片段。


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

* [将示例xdp文件下载并导入AEM](assets/xdp-templates-fragments.zip)
* [使用AEM包管理器下载和安装包](assets/using-fragments-assets.zip)
* [可以从此处下载示例xdp和片段](assets/xdptemplates.zip)

**安装包后，必须在Granite CSRF过允许列表滤器中Adobe以下URL。**

1. 请按照下面提到的步骤来允许列表上述路径。
1. [登录到configMgr](http://localhost:4502/system/console/configMgr)
1. 搜索AdobeGranite CSRF过滤器
1. 在排除的部分中添加以下路径并保存
1. /content/AemFormsSamples/usingfragments

有多种方法可测试示例代码。 最快、最简单的方法是使用Postman应用程序。 Postman允许您向服务器发出POST请求。 在系统上安装Postman应用程序。
启动应用程序并输入以下URL以测试导出数据API

确保您从下拉列表http://localhost:4502/content/AemFormsSamples/usingfragments.html中选择了“POST”。确保将“授权”指定为“基本身份验证”。 指定AEM Server用户名和密码导航到“Body”选项卡，并指定请求参数，如下图所示
![导出](assets/using-fragment-postman.png)
然后，单击“发送”按钮

[您可以导入此邮递员集合以测试API](assets/usingfragments.postman_collection.json)
