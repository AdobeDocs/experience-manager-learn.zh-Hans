---
title: 使用使用权限将XDP渲染为PDF
seo-title: 使用使用权限将XDP渲染为PDF
description: 对pdf应用使用权限
seo-description: 对pdf应用使用权限
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: Forms Service
discoiquuid: aefb4124-91a0-4548-94a3-86785ea04549
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---


# 将XDP渲染为具有使用权限{#rendering-xdp-into-pdf-with-usage-rights}的PDF

一个常见用例是将xdp渲染到PDF中，并将Reader扩展应用到渲染的PDF。

例如，在AEM Forms的表单门户中，当用户单击XDP时，我们可以将XDP渲染为PDF，并且Reader会扩展PDF。

要测试此功能，可尝试使用此[链接](https://forms.enablementadobe.com/content/samples/samples.html?query=0)。 示例名称为“Render XDP with RE”

要完成此用例，我们需要执行以下操作。

* 向“fd-service”用户添加Reader扩展证书。 添加Reader扩展凭据的步骤列在[此处](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)

* 创建将呈现和应用使用权限的自定义OSGi服务。 完成此操作的代码如下

## 渲染XDP并应用使用权限{#render-xdp-and-apply-usage-rights}

* 第7行：使用FormsService的renderPDForm，我们从XDP生成PDF。

* 第8-14行：设置了相应的使用权限。 从OSGi配置设置中获取这些使用权限。

* 第20行：使用与服务用户fd-service关联的资源解析器

* 第24行：DocumentAssuranceService的secureDocument方法用于应用使用权限

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

以下屏幕截图显示了公开的配置属性。 大多数常用使用权限通过此配置公开。

![](assets/configurationproperties.gif)

以下代码显示用于构建OSGi配置设置的代码

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## 创建Servlet以流式传输PDF {#create-servlet-to-stream-the-pdf}

下一步是使用GET方法创建一个Servlet，将Reader扩展PDF返回给用户。 在这种情况下，将要求用户将PDF保存到其文件系统。 这是因为PDF以动态PDF形式呈现，而浏览器附带的pdf查看器不处理动态PDF。

以下是servlet的代码。 我们将CRX存储库中的XDP路径传递到此servlet。

然后，我们调用com.aemformssamples.documentservices.core.DocumentServices的renderAndExtendXdp方法。

然后，将Reader扩展PDF流式传输到调用应用程序

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

要在本地服务器上测试此功能，请执行以下步骤
1. [下载并安装DevelopingWithServiceUser捆绑包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下载并安装AEMFormsDocumentServices捆绑](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用包管理器将与本文相关的资产下载并导入AEM](assets/renderandextendxdp.zip)
   * 此包包含示例门户和xdp文件
1. 向“fd-service”用户添加Reader扩展证书
1. 将浏览器指向[门户网页](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. 单击pdf图标以渲染xdp并获取Reader Extended



