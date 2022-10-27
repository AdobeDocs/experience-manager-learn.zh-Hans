---
title: 在输出服务中使用片段
description: 生成PDF文档，其中片段驻留在CRX存储库中
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 使用ECMA脚本生成带有片段的PDF文档{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我们将使用输出服务来使用xdp片段生成pdf文件。 主xdp和片段位于crx存储库中。 在AEM中模拟文件系统文件夹结构非常重要。 例如，如果您在xdp的片段文件夹中使用片段，则必须创建一个名为 **片段** 在AEM的基文件夹下。 基本文件夹将包含您的基本xdp模板。 例如，如果文件系统上具有以下结构
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* 文件夹xdpdocuments将包含您的基本模板和 **片段** 文件夹

您可以使用 [表单和文档ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

以下是示例xdp的文件夹结构，该结构使用2个片段
![表单&amp;文档](assets/fragment-folder-structure-ui.png)


* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成扁平化的pdf。 有关更多详细信息，请参阅 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 的值。 在此示例中，我们使用的是驻留在crx存储库中的片段。


以下ECMA脚本已使用生成PDF。 请注意在代码中使用ResourceResolver和ResourceResolverHelper。 由于此代码在任何用户上下文之外运行，因此需要ResourceRelover。

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**在系统上测试示例包**
* [部署DevelopingWithServiceUSer包](assets/DevelopingWithServiceUser.jar)
* 添加条目 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 在用户映射器服务修正中，如以下屏幕截图所示
   ![用户映射器修正](assets/user-mapper-service-amendment.png)
* [下载并导入示例xdp文件和ECMA脚本](assets/watched-folder-fragments-ecma.zip).
这将在c:/fragmentsandoutputservice文件夹中创建一个监视的文件夹结构

* [提取示例数据文件](assets/usingFragmentsSampleData.zip) 并将其放在监视文件夹的安装文件夹中(c:\fragmentsandoutputservice\install)

* 检查已监视文件夹配置的结果文件夹，以查找生成的pdf文件
