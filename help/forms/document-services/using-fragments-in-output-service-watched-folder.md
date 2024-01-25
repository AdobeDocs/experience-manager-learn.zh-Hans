---
title: 在带有watched文件夹的输出服务中使用片段
description: 生成片段驻留在crx存储库中的pdf文档
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
duration: 100
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# 使用ECMA脚本生成带片段的pdf文档{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我们将使用输出服务来生成使用xdp片段的pdf文件。 主xdp和片段驻留在crx存储库中。 模拟AEM中的文件系统文件夹结构很重要。 例如，如果您在xdp的片段文件夹中使用片段，则必须创建一个名为 **片段** 在AEM中的基础文件夹下。 基本文件夹将包含您的基本xdp模板。 例如，如果文件系统中具有以下结构
* c：\xdptemplates — 这将包含您的基本xdp模板
* c：\xdptemplates\fragments — 此文件夹将包含片段，主模板将引用如下所示的片段
  ![片段 — xdp](assets/survey-fragment.png).
* xdpdocuments文件夹将包含您的基本模板和片段 **片段** 文件夹

您可以使用创建所需的结构 [表单和文档ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

以下是示例使用2个片段的文件夹结构
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* 输出服务 — 通常，此服务用于将xml数据与xdp模板或pdf合并，以生成拼合的pdf。 欲知更多详情，请参阅 [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 用于Output服务 在此示例中，我们使用驻留在crx存储库中的片段。


以下ECMA脚本已用于生成PDF。 请注意代码中使用了ResourceResolver和ResourceResolverHelper。 需要ResourceReolver，因为此代码在任何用户上下文之外运行。

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
* [部署DevelopingWithServiceUSer捆绑包](assets/DevelopingWithServiceUser.jar)
* 添加条目 **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service** 在用户映射器服务修正中，如下面的屏幕快照中所示
  ![用户映射器修正](assets/user-mapper-service-amendment.png)
* [下载并导入示例xdp文件和ECMA脚本](assets/watched-folder-fragments-ecma.zip).
这将在c：/fragmentsandoutputservice文件夹中创建观察文件夹结构

* [提取示例数据文件](assets/usingFragmentsSampleData.zip) 并将其放入观察文件夹的install文件夹中(c：\fragmentsandoutputservice\install)

* 检查观察文件夹配置的结果文件夹中是否包含生成的pdf文件
