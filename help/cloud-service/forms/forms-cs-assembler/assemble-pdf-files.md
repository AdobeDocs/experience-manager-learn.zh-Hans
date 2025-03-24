---
title: 使用调用DDX操作组合PDF文件
description: 发出POST请求以使用必要的参数调用DDX端点
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 693dac88-84f3-4051-8e46-3105093711a3
duration: 56
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 0%

---

# 进行POST调用


下一步是使用必要的参数对端点进行HTTP POST调用。 DDX和pdf文件作为资源文件提供。 端点具有基于令牌的身份验证，我们在请求标头中传递访问令牌。
使用Assembler服务时，请使用一种名为Document Description XML (DDX)的基于XML的语言来描述所需的输出。 DDX是一种声明性标记语言，其元素代表文档的构建块。以下DDX用于合并PDF源元素中标识的两个pdf文档。

```xml
<DDX xmlns="http://ns.adobe.com/DDX/1.0/">
<PDF result="doc3.pdf"> 
	<PDF source="CA-Drivers-Handbook.pdf"/>
 	<PDF source="CA-Parent-Teen-Handbook.pdf"/>
  </PDF>
</DDX>
```

以下代码用于组合pdf文件

```java
package com.aemformscs.documentservices;

import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.URL;

import org.apache.commons.io.IOUtils;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.HttpMultipartMode;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;

public class AssemblePDFFiles {
  public String SAVE_LOCATION = "c:\\assembledpdf";

  public void assemblePDF(String postURL) {

    HttpPost httpPost = new HttpPost(postURL);
    CredentialUtilites cu = new CredentialUtilites();
    String accessToken = cu.getAccessToken();
    httpPost.addHeader("Authorization", "Bearer " + accessToken);
    ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
    URL ddxFile = classLoader.getResource("ddxfiles/assemble2pdfs.ddx");
    MultipartEntityBuilder builder = MultipartEntityBuilder.create();

    builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);

    File ddx = new File(ddxFile.getPath());
    builder.addBinaryBody("ddx", ddx, ContentType.create("application/xml"), ddx.getName());
    URL url = classLoader.getResource("pdffiles");
    System.out.println(url.getPath());
    File files[] = new File(url.getPath()).listFiles();

    for (int i = 0; i < files.length; i++) {
      System.out.println("Added  " + files[i].getName());
      builder.addBinaryBody(files[i].getName(), files[i], ContentType.APPLICATION_OCTET_STREAM, files[i].getName());
    }
    try {

      HttpEntity entity = builder.build();
      httpPost.setEntity(entity);
      CloseableHttpClient httpclient = HttpClients.createDefault();
      CloseableHttpResponse response = httpclient.execute(httpPost);
      System.out.println("The success code is " + response.getStatusLine().getStatusCode());
      InputStream generatedPDF = response.getEntity().getContent();
      byte[] bytes = IOUtils.toByteArray(generatedPDF);
      File saveLocation = new File(SAVE_LOCATION);
      if (!saveLocation.exists()) {
        saveLocation.mkdirs();
      }
      File outputFile = new File(SAVE_LOCATION + File.separator + "assembledPDF.pdf");
      FileOutputStream outputStream = new FileOutputStream(outputFile);
      outputStream.write(bytes);
      outputStream.close();
      System.out.println("AssembledPDF.pdf saved to " + SAVE_LOCATION);

    } catch (Exception e) {
      System.out.println("The message is " + e.getMessage());
    }
  }

}
```
