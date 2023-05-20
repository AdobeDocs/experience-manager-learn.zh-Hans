---
title: 建立OSGi服務
description: 建立OSGi服務以儲存要簽署的表單
feature: Workflow
version: 6.4,6.5
thumbnail: 6886.jpg
kt: 6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 1%

---

# 建立OSGi服務

下列程式碼是用來儲存需要簽署的表單。 每個要簽署的表單都與唯一的GUID和客戶ID相關聯。 因此，一或多個表單可以與相同的客戶ID建立關聯，但會將唯一的GUID指派給表單。

## 接口

以下是使用的介面宣告

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## 插入資料

insert data方法會在資料來源所識別的資料庫中插入列。 資料庫中的每一列都與表單相對應，並由GUID和客戶ID唯一識別。 表單資料和表單URL也儲存在此列中。 狀態列用於指示表單是否已填寫和簽署。 0值表示表單尚未簽署。

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## 取得表單資料

下列程式碼用於擷取與特定GUID關聯的最適化表單資料。 然後，表單資料會用於預先填入最適化表單。

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## 更新簽章狀態

成功完成簽署儀式會觸發與表單相關聯的AEM工作流程。 工作流程的第一步是處理步驟，它會更新資料庫中由guid和客戶id所識別之列的狀態。 我們也會將formdata中帶正負號的元素的值設為Y，表示表單已填寫並簽署。 最適化表單會填入此資料，而xml資料中已簽署資料元素的值會用來顯示適當的訊息。 會從自訂程式步驟叫用updateSignatureStatus程式碼。


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## 取得下一個要簽署的表單

下列程式碼已用於取得狀態為0之指定customerID的下一個簽署表單。 如果sql查詢未傳回任何列，我們會傳回字串 **&quot;AllDone&quot;** 這表示指定的客戶id已沒有任何可供簽署的表單。

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Assets

包含上述服務的OSGi套件組合可以是 [已從此處下載](assets/sign-multiple-forms.jar)

## 后续步骤

[建立主要工作流程以處理初始表單提交](./create-main-workflow.md)
