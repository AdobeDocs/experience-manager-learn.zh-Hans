---
title: 使用許可權將XDP轉譯為PDF
description: 套用使用許可權至pdf
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 2%

---

# 使用許可權將XDP轉譯為PDF{#rendering-xdp-into-pdf-with-usage-rights}

一個常見的使用案例是將xdp轉譯為PDF並將Reader擴充功能套用至轉譯的PDF。

例如，在AEM Forms的表單入口網站中，當使用者按一下XDP時，我們可以將XDP轉譯為PDF，讓讀者擴展PDF。


若要完成此使用案例，我們需要執行下列動作。

* 將Reader擴充功能憑證新增至「fd-service」使用者。 列出新增Reader延伸模組認證的步驟 [此處](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=zh-Hans)


* 您也可以參閱以下影片： [設定Reader延伸模組認證](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* 建立可轉譯和套用使用許可權的自訂OSGi服務。 完成此工作的程式碼如下

## 轉譯XDP並套用使用許可權 {#render-xdp-and-apply-usage-rights}

* 第7行：我們使用FormsService的renderPDFForm從XDP產生PDF。

* 第8-14行：設定適當的使用許可權。 這些使用許可權是從OSGi組態設定中擷取。

* 第20行：使用與服務使用者fd-service相關聯的resourceresolver

* 第24行：DocumentAssuranceService的secureDocument方法用於套用使用許可權

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

下列熒幕擷圖顯示公開的設定屬性。 大部分的常見使用許可權會透過此設定公開。

![設定屬性](assets/configurationproperties.gif)

下列程式碼會顯示用於建置OSGi組態設定的程式碼

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

## 建立Servlet以串流PDF {#create-servlet-to-stream-the-pdf}

下一步是使用GET方法建立servlet，以將讀取器擴展PDF傳回給使用者。 在這種情況下，系統會要求使用者將PDF儲存至其檔案系統。 這是因為PDF會呈現為動態PDF，而且瀏覽器隨附的pdf檢視器不會處理動態pdf。

以下是servlet的程式碼。 我們會將CRX存放庫中的XDP路徑傳遞給此servlet。

然後呼叫com.aemformssamples.documentservices.core.DocumentServices的renderAndExtendXdp方法。

然後，讀取器延伸PDF會串流至呼叫應用程式

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

若要在本機伺服器上測試此專案，請遵循下列步驟
1. [下載並安裝DevelopingWithServiceUser套裝](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載並安裝AEMFormsDocumentServices套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [下載自訂入口網站範本html](assets/render-and-extend-template.zip)
1. [使用封裝管理程式下載與本文相關的資產並將其匯入AEM](assets/renderandextendxdp.zip)
   * 此套件具有範例入口網站和xdp檔案
1. 將Reader延伸模組憑證新增至「fd-service」使用者
1. 將瀏覽器指向 [入口網站網頁](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. 按一下pdf圖示將xdp轉譯為已套用使用許可權的pdf檔案。
