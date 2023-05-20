---
title: 在HTM5表單提交時觸發AEM工作流程 — 處理PDF提交
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 繼續以離線模式填寫行動表單並提交行動表單以觸發AEM工作流程
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: eafeafe1-7a72-4023-b5bb-d83b056ba207
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# 處理PDF提交

在本部分中，我們將建立在AEM Publish上執行的簡單servlet，以處理來自Acrobat/Reader的PDF提交。 此servlet接著會向負責將提交的資料儲存為的AEM製作執行個體中執行的servlet發出HTTPPOST請求 `nt:file` AEM作者存放庫中的節點。

以下是處理PDF提交的servlet程式碼。 在此servlet中，我們會對掛載的servlet進行POST呼叫 **/bin/startworkflow** （在AEM作者執行個體中）。 此servlet會將表單資料儲存在AEM作者的存放庫中。


## AEM發佈servlet

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

## AEM Author servlet

下一步是將提交的資料儲存在AEM作者的存放庫中。 掛載的servlet `/bin/startworkflow` 儲存提交的資料。

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

AEM工作流程啟動器已設定為每次有新資源型別時觸發 `nt:file` 建立於 `/content/pdfsubmissions` 節點。 此工作流程會將提交的資料與xdp範本合併，以建立非互動式或靜態PDF。 產生的PDF會指派給使用者進行稽核和核准。

若要將提交的資料儲存在 `/content/pdfsubmissions` 節點，我們利用 `GetResolver` OSGi服務可讓我們使用儲存提交的資料 `fd-service` 系統使用者，可在每個AEM Forms安裝中使用。
