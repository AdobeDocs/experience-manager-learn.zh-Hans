---
title: 具有JSON結構描述和資料的AEM Forms[Part2]
seo-title: AEM Forms with JSON Schema and Data[Part2]
description: 多部分教學課程將逐步引導您完成使用JSON結構描述建立調適型表單和查詢已提交資料的相關步驟。
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

# 將提交的資料儲存在資料庫中


>[!NOTE]
>
>建議您使用MySQL 8作為資料庫，因為它支援JSON資料型別。 您還需要為MySQL DB安裝適當的驅動程式。 我已使用此位置https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12提供的驅動程式

為了將提交的資料儲存在資料庫中，我們將撰寫servlet來擷取繫結的資料以及表單名稱並儲存。 以下提供處理表單提交及將afBoundData儲存在資料庫中的完整程式碼。

我們已建立自訂提交來處理表單提交。 在此自訂提交的post.request.jsp中，我們將POST轉寄給我們的servlet。

若要進一步瞭解自訂提交，請閱讀此 [文章](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

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

![connectionpool](assets/connectionpooled.gif)

若要讓系統運作正常，請遵循下列步驟

* [下載並解壓縮zip檔案](assets/aemformswithjson.zip)
* 使用JSON結構描述建立最適化表單。 您可以使用本文資產中提供的JSON結構描述。 請確認表單的提交動作已適當設定。 提交動作需要設定為「CustomSubmitHelpx」。
* 使用MySQL Workbench工具匯入schema.sql檔案，在MySQL執行個體中建立結構描述。 schema.sql檔案也隨附在本教學課程資產中。
* 從Felix Web主控台設定Apache Sling Connection Pooled DataSource
* 請務必將資料來源命名為「aemformswithjson」。 這是提供給您的範例OSGi套件組合所使用的名稱
* 如需屬性，請參閱上圖。 這是假設您將使用MySQL作為資料庫。
* 部署作為本文資產一部分提供的OSGi套件組合。
* 預覽表單並提交。
* JSON資料會儲存在您匯入「schema.sql」檔案時建立的資料庫中。
