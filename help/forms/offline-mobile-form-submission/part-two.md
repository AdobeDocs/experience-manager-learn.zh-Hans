---
title: 在HTM5表单提交时触发AEM工作流 — 处理PDF提交
description: 在离线模式下继续填写移动表单并提交移动表单以触发AEM工作流
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: eafeafe1-7a72-4023-b5bb-d83b056ba207
duration: 116
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# 处理PDF提交

在此部分中，我们将创建一个在AEM Publish上运行的简单servlet，以处理从Acrobat/Reader提交的PDF。 此servlet随后将向AEM创作实例中运行的servlet发出HTTPPOST请求，该servlet负责将提交的数据另存为AEM创作存储库中的`nt:file`节点。

以下是处理PDF提交的servlet的代码。 在此servlet中，我们对AEM Author实例中&#x200B;**/bin/startworkflow**&#x200B;上装载的servlet进行POST调用。 此servlet将表单数据保存在AEM作者的存储库中。


## AEM Publish servlet

```java
package com.aemforms.handlepdfsubmission.core.servlets;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import javax.servlet.ServletInputStream;
import javax.servlet.ServletOutputStream;

import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
  service={Servlet.class}, 
  property={
    "sling.servlet.methods=post", 
    "sling.servlet.paths=/bin/handlepdfsubmit"
  }
)
public class HandlePDFSubmission extends SlingAllMethodsServlet {

  private static Logger logger = LoggerFactory.getLogger(HandlePDFSubmission.class);
  
  protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
    ByteArrayOutputStream result = new ByteArrayOutputStream();
    try {
       ServletInputStream is = request.getInputStream();
       byte[] buffer = new byte[1024];
       int length;
       while ((length = is.read(buffer)) != -1) {
         result.write(buffer, 0, length);
       }
       logger.debug(result.toString(StandardCharsets.UTF_8.name()));
     } catch (IOException e1) {
        logger.error("An error occurred", e1);
     }

     HttpPost postReq = new HttpPost("http://localhost:4502/bin/startworkflow");
     // This is the base64 encoding of the admin credetnials. This call should be made over HTTPS in production scenarios to avoid leaking credentials.
     postReq.addHeader("Authorization", "Basic YWRtaW46YWRtaW4=");
     
     CloseableHttpClient httpClient = HttpClients.createDefault();
     List<NameValuePair> urlParameters = new ArrayList<NameValuePair>();
     
     logger.debug("added url parameters");
     
     try {
        urlParameters.add(new BasicNameValuePair("xmlData", result.toString(StandardCharsets.UTF_8.name())));
        postReq.setEntity(new UrlEncodedFormEntity(urlParameters));
        httpClient.execute(postReq);
        logger.debug("Sent request to author instance");
        ServletOutputStream sout = response.getOutputStream();
        sout.print("Your form was successfully submitted");
     } catch (UnsupportedEncodingException | ClientProtocolException | IOException e) {
        logger.error("An error occurred", e)
     }
}
```

## AEM创作servlet

下一步是将提交的数据存储在AEM Author的存储库中。 装载在`/bin/startworkflow`上的servlet保存提交的数据。

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.UUID;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

@Component(
    service = {Servlet.class},
    property = {
            "sling.servlet.methods=get",
            "sling.servlet.paths=/bin/startworkflow"
    }
)
public class StartWorkflow extends SlingAllMethodsServlet {
        private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

        @Reference
        private GetResolver getResolver;

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
            String xmlData = null;
            System.out.println("in start workflow");
            response.setContentType("text/html;charset=UTF-8");

            if (request.getParameter("xmlData") != null) {
                logger.debug("The form was submitted from Acrobat/Reader");
                xmlData = request.getParameter("xmlData");
                System.out.println("in start workflow" + xmlData);
            } else {
                logger.debug("Mobile Form submission");
                StringBuffer stringBuffer = new StringBuffer();
                String line = null;

                try {
                    InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
                    BufferedReader reader = new BufferedReader(isReader);
                    while ((line = reader.readLine()) != null)
                        stringBuffer.append(line);
                } catch (Exception e) {
                    logger.debug("Error" + e.getMessage());
                }

                xmlData = new String(stringBuffer);
            }

            Resource r = getResolver.getFormsServiceResolver().getResource("/content/pdfsubmissions");
            Session session = r.getResourceResolver().adaptTo(Session.class);
            logger.debug("Got reosurce pdfsubmissions" + r.getPath());
            UUID uidName = UUID.randomUUID();
            Node xmlDataFilesNode = r.adaptTo(Node.class);
            InputStream is = new ByteArrayInputStream(xmlData.getBytes());
            Binary binary;

            try {
                Node xmlFileNode = xmlDataFilesNode.addNode(uidName.toString(), "nt:file");
                logger.debug("Added nt file node");
                Node jcrContent = xmlFileNode.addNode("jcr:content", "nt:resource");
                logger.debug("Added jcr content");
                binary = session.getValueFactory().createBinary(is);
                jcrContent.setProperty("jcr:data", binary);
                session.save();
            } catch (RepositoryException e) {
                logger.error("Unable to store data to JCR", e);
            }

            response.setContentType("text/plain;charset=UTF-8");

            try {
                ServletOutputStream sout = response.getOutputStream();
                sout.print("Your form was successfully submitted");
            } catch (IOException e) {
                logger.error("Unable write response", e);
            }
        }
}
```

AEM工作流启动器配置为每次在`/content/pdfsubmissions`节点下创建`nt:file`类型的新资源时都会触发。 此工作流将通过将提交的数据与xdpPDF合并来创建非交互式或静态模板。 然后将生成的PDF分配给用户进行审阅和批准。

为了在`/content/pdfsubmissions`节点下存储提交的数据，我们使用`GetResolver`个OSGi服务，以便能够使用`fd-service`系统用户(可在每个AEM Forms安装中使用)保存提交的数据。
