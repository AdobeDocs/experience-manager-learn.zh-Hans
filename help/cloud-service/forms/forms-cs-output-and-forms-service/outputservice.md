---
title: 使用输出服务生成PDF单据
description: 将数据与XDP模板合并以生成非交互式PDF
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
source-git-commit: a0de7eaa391749b6b0d90e7cf3e363c2d5a232b5
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 8%

---


# 使用输出服务生成PDF单据

[Output服务](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html)是属于AEM Document Services的OSGi服务。 它支持各种AEM Forms Designer输出格式和设计功能。 输出服务转换XFA模板和XML数据以生成不同格式的打印文档。

AEM Formsas a Cloud Service中的输出服务与AEM Forms 6.5中的输出服务非常相似，因此，如果您熟悉AEM Forms 6.5中的输出服务，则过渡到AEM Formsas a Cloud Service应该非常简单。

使用Output服务，您可以创建应用程序，以便：

+ 使用 XML 数据填充模板文件来生成最终表单文档。
+ 生成各种格式的输出表单，包括非交互式PDF、PostScript、PCL和ZPL打印流。
+ 从 XFA 表单 PDF 生成打印 PDF。
+ 通过将多组数据与提供的模板合并，批量生成PDF、PostScript、PCL和ZPL文档。

此服务设计为在AEM Formsas a Cloud Service实例的上下文中使用。 以下代码片段使用`OutputService`在servlet中生成PDF文档。

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
