---
title: 使用usagerights API
description: 将使用权限应用于所提供的PDF的示例代码
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---

# 进行API调用

## 应用使用权限

获得访问令牌后，下一步是发出API请求以将使用权限应用于指定的PDF。 这涉及在请求标头中包含访问令牌以验证调用，确保文档的安全且授权处理。

以下函数应用使用权限

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## 功能划分：



* **设置API端点和负载**
   * 使用提供的`endPoint`和预定义的`BUCKET`构造API URL。
   * 定义一个指定要应用的权限的JSON字符串(`usageRights`)，例如：
      * 评论
      * 嵌入式文件
      * 表单填写
      * 表单数据导出

* **加载PDF文件**
   * 从`pdffiles`目录中检索`withoutusagerights.pdf`文件。
   * 记录错误，如果找不到该文件，则退出。

* **准备HTTP请求**
   * 将PDF文件读入字节数组。
   * 使用`MultipartEntityBuilder`创建包含下列内容的多部分请求：
      * 作为二进制主体的PDF文件。
      * `usageRights` JSON作为文本正文。
   * 设置具有标头的HTTP `POST`请求：
      * `Authorization: Bearer <accessToken>`进行身份验证。
      * `X-Adobe-Accept-Experimental: 1` （可能是API兼容性所必需的）。

* **发送请求并处理响应**
   * 使用`httpClient.execute(httpPost)`执行HTTP请求。
   * 读取响应(预期是应用了使用权限的更新PDF)。
   * 将接收的PDF内容写入位于`SAVE_LOCATION`的&#x200B;**&quot;ReaderExtended.pdf&quot;**。

* **错误处理和清理**
   * 捕获并记录任何`IOException`错误。
   * 确保`finally`块中的所有资源（流、HTTP客户端、响应）均已正确关闭。

以下是调用applyUsageRights函数的main.java代码

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

`main`方法通过从`AccessTokenService`调用`getAccessToken()`初始化，它应返回有效令牌。

* 然后，它从`DocumentGeneration`类中调用`applyUsageRights()`，并传入：
   * 已检索`accessToken`
   * 用于应用使用权限的API端点。


## 后续步骤

[部署示例项目](sample-project.md)
