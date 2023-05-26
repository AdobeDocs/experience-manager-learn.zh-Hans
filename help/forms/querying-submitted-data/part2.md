---
title: 具有JSON架构和数据的AEM Forms[Part2]
seo-title: AEM Forms with JSON Schema and Data[Part2]
description: 多部分教程将指导您完成使用JSON模式创建自适应表单和查询提交的数据所涉及的步骤。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# 将提交的数据存储在数据库中


>[!NOTE]
>
>建议使用MySQL 8作为数据库，因为它支持JSON数据类型。 您还需要为MySQL数据库安装适当的驱动程序。 我已使用此位置https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12中提供的驱动程序

为了将提交的数据存储在数据库中，我们编写一个servlet来提取绑定数据和表单名称并存储。 下面给出了处理表单提交并将afBoundData存储在数据库中的完整代码。

我们创建了自定义提交来处理表单提交。 在此自定义提交的post.request.jsp中，我们将POST转发到我们的servlet。

要了解有关自定义提交请求的详细信息，请阅读此 [文章](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest，&quot;/bin/storeafsubmission&quot;，null，null)；

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![连接池](assets/connectionpooled.gif)

要使此功能在您的系统中正常工作，请执行以下步骤

* [下载并解压缩zip文件](assets/aemformswithjson.zip)
* 创建具有JSON架构的自适应表单。 您可以使用作为本文资产的一部分提供的JSON架构。 确保表单的提交操作已正确配置。 提交操作需要配置到“CustomSubmitHelpx”。
* 通过使用MySQL Workbench工具导入schema.sql文件，在MySQL实例中创建架构。 schema.sql文件也作为本教程资源的一部分提供给您。
* 从Felix Web控制台配置Apache Sling Connection Pooled DataSource
* 确保将数据源命名为“aemformswithjson”。 这是提供给您的示例OSGi捆绑包使用的名称
* 有关属性，请参阅上图。 这是假设您将使用MySQL作为数据库。
* 部署作为本文资源的一部分提供的OSGi捆绑包。
* 预览表单并提交。
* JSON数据存储在导入“schema.sql”文件时创建的数据库中。
