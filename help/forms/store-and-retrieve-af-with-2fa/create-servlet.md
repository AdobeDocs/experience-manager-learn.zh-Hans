---
title: 创建Servlet
description: 创建Servlet以处理保存表单数据的POST请求
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6539
thumbnail: 6539.pg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 4%

---

# 创建Servlet

下一步是创建一个Servlet，它将调用我们自定义OSGi服务的相应方法。 Servlet可以访问自适应表单数据、文件附件信息。 Servlet将返回一个唯一的应用程序ID，它可用于检索部分完成的自适应表单。

当用户单击自适应表单上的“保存并退出”按钮时，将调用此servlet

```java
package com.techmarketing.core.servlets;

import java.io.PrintWriter;
import javax.servlet.Servlet;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import store.and.fetch.core.*;

@Component(service = {
    Servlet.class
}, property = {
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
    private static final long serialVersionUID = 1 L;
    @Reference(target = "(&(datasource.name=aemformstutorial))")
    private DataSource dataSource;
    @Reference
    AemFormsAndDB aemFormsAndDB;
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("#### Inside dopost of StoreDataInDBWithAttachmentsInfo ####");
        String afData = request.getParameter("data");
        String tel = request.getParameter("mobileNumber");
        log.debug("$$$The telephone number is  " + tel);
        log.debug("The request parameter " + afData);
        try {
            JSONObject fileMap = new JSONObject(request.getParameter("fileMap").toString());
            String newFileMap = aemFormsAndDB.storeAFAttachments(fileMap, request);
            String application_id = aemFormsAndDB.storeFormData(afData, newFileMap.toString(), tel);
            JsonObject jsonObject = new JsonObject();
            jsonObject.addProperty("path", application_id);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
        } catch (Exception ex) {
            log.debug(ex.getMessage());
        }
    }
}
```
