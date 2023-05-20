---
title: 使用對話方塊實作自訂流程步驟
description: 使用自訂程式步驟將最適化表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# 自訂流程步驟

本教學課程適用於需要實作自訂工作流程元件的AEM Forms客戶。建立工作流程元件的第一步是撰寫將與工作流程元件關聯的Java程式碼。 在本教學課程中，我們將編寫簡單的Java類別，以將最適化表單附件儲存至檔案系統。此Java程式碼將讀取工作流程元件中指定的引數。

寫入Java類別並將類別部署為OSGi套件組合時，需要執行下列步驟

## 建立Maven專案

第一步是使用適當的AdobeMaven原型建立Maven專案。 詳細步驟列於此 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). 將您的maven專案匯入到eclipse中後，您就可以開始編寫可在流程步驟中使用的第一個OSGi元件了。


### 建立實作WorkflowProcess的類別

在eclipse IDE中開啟maven專案。 展開 **projectname** > **核心** 資料夾。 展開src/main/java資料夾。 您應該會看到結尾為「core」的套件。 建立在此封裝中實作WorkflowProcess的Java類別。 您需要覆寫執行方法。 執行方法的簽章如下public void execute(WorkItem workItem， WorkflowSession workflowSession， MetaDataMap processArguments)throws WorkflowException

在本教學課程中，我們會將新增至最適化表單的附件寫入檔案系統，作為AEM Workflow的一部分。

為了完成此使用案例，已撰寫下列java類別

讓我們來看看此程式碼

```java
package com.mysite.core;
import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import javax.jcr.Node;
import javax.jcr.Session;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);

    log.debug("Got attachments path: " + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
    log.debug("Got save location: " + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* attachmentsPath — 這是您在調適型表單中指定的位置(當您設定調適型表單的提交動作以叫用AEM Workflow時)。 這是您希望附件相對於工作流程裝載儲存在AEM中的資料夾名稱。

* saveToLocation — 這是您希望附件儲存在AEM伺服器檔案系統中的位置。

這兩個值會使用工作流程元件的對話方塊作為程式引數傳遞

![ProcessStep](assets/custom-workflow-component.png)

QueryBuilder服務用於查詢attachmentsPath資料夾下nt：file型別的節點。 其餘程式碼會逐一瀏覽搜尋結果，以建立Document物件並將其儲存至檔案系統


>[!NOTE]
>
>由於我們使用的是AEM Forms專屬的Document物件，因此您需要在maven專案中包含aemfd-client-sdk相依性。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### 建置和部署

[依照此處的說明建置套件組合](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[請確定該套件組合已部署且處於作用中狀態](http://localhost:4502/system/console/bundles)

## 后续步骤

建立您的 [自訂工作流程元件](./custom-workflow-component.md)

