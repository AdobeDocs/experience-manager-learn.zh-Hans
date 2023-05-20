---
title: 儲存最適化表單資料
description: 將最適化表單資料儲存至DataBase，作為AEM Workflow的一部分
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 2%

---

# 在資料庫中儲存最適化表單提交

有多種方式可將提交的表單資料儲存在您選擇的資料庫中。 JDBC資料來源可用來直接將資料儲存到資料庫中。 可以寫入自訂OSGI套件組合以將資料儲存至資料庫。 本文使用AEM工作流程中的自訂流程步驟來儲存資料。
使用案例是在提交最適化表單時觸發AEM工作流程，且工作流程中的步驟會將提交的資料儲存至資料庫。



## JDBC連線集區

* 前往 [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * 搜尋「JDBC連線集區」。 建立新的Day Commons JDBC連線集區。 指定資料庫的特定設定。

   * ![JDBC連線集區OSGi設定](assets/aemformstutorial-jdbc.png)

## 指定資料庫詳細資訊

* 搜尋&quot;**指定資料庫詳細資訊**&quot;
* 指定資料庫的特定屬性。
   * DataSourceName：您先前設定的資料來源名稱。
   * TableName — 您要儲存AF資料的表格名稱
   * FormName — 保留表單名稱的欄名稱
   * ColumnName — 儲存AF資料的欄名稱

   ![指定資料庫詳細資訊OSGi設定](assets/specify-database-details.png)



## OSGi設定的代碼

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## 讀取設定值

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## 實作處理步驟的程式碼

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## 部署範例資產

* 請確定您已設定JDBC連線集區
* 使用configMgr指定資料庫詳細資訊
* [下載Zip檔案並將檔案內容解壓縮至您的硬碟](assets/article-assets.zip)

   * 部署jar檔案，使用 [AEM網頁主控台](http://localhost:4502/system/console/bundles). 此jar檔案包含儲存資料庫中表單資料的程式碼。

   * 將兩個zip檔案匯入 [使用封裝管理程式的AEM](http://localhost:4502/crx/packmgr/index.jsp). 這樣您就可以 [範例工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html) 和 [最適化表單範例](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) 這會觸發表單提交的工作流程。 請注意工作流程步驟中的程式引數。 這些引數會指出將包含最適化表單資料的資料檔的表單名稱和名稱。 資料檔案儲存在crx存放庫的裝載資料夾下。 請留意 [最適化表單](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) 設定為在提交時觸發AEM工作流程，並設定資料檔案設定(data.xml)

   * 預覽並填寫表單並提交。 您應該會看到資料庫中建立的新列

