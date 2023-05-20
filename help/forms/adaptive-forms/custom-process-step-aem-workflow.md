---
title: 實作自訂流程步驟
description: 使用自訂程式步驟將最適化表單附件寫入檔案系統
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---

# 自訂流程步驟

本教學課程適用於需要實作自訂流程步驟的AEM Forms客戶。 處理步驟可以執行ECMA指令碼或呼叫自訂Java程式碼來執行作業。 本教學課程將說明實作由程式步驟執行的WorkflowProcess所需的步驟。

實作自訂流程步驟的主要原因是為了擴充AEM Workflow。 例如，如果您在工作流程模型中使用AEM Forms元件，您可能會想要執行下列操作

* 將最適化表單附件儲存至檔案系統
* 處理提交的資料

若要完成上述使用案例，您通常會撰寫由處理步驟執行的OSGi服務。

## 建立Maven專案

第一步是使用適當的AdobeMaven原型建立Maven專案。 詳細步驟列於此 [文章](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). 將您的maven專案匯入到eclipse中後，您就可以開始編寫可在流程步驟中使用的第一個OSGi元件了。


### 建立實作WorkflowProcess的類別

在eclipse IDE中開啟maven專案。 展開 **projectname** > **核心** 資料夾。 展開src/main/java資料夾。 您應該會看到結尾為「core」的套件。 建立在此封裝中實作WorkflowProcess的Java類別。 您需要覆寫執行方法。 執行方法的簽章如下public void execute(WorkItem workItem， WorkflowSession workflowSession， MetaDataMap processArguments)throws WorkflowException執行方法可存取下列3個變數

**工作專案**：workItem變數會授予與工作流程相關資料的存取權。 公開API檔案已可供參考 [此處。](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**工作流程工作階段**：此workflowSession變數可讓您控制工作流程。 公開API檔案已可供參考 [此處](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**中繼資料對應**：與工作流程相關聯的所有中繼資料。 傳遞至程式步驟的任何程式引數都可使用MetaDataMap物件。[API檔案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

在本教學課程中，我們會將新增至最適化表單的附件寫入檔案系統，作為AEM Workflow的一部分。

為了完成此使用案例，已撰寫下列java類別

讓我們來看看此程式碼

```java
package com.learningaemforms.adobe.core;

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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
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
                log.debug("Error saving file " + e.getMessage());
            }
```

第1行 — 定義元件的屬性。 將OSGi元件與程式步驟建立關聯時，您會看到process.label屬性，如下面的其中一個熒幕擷取畫面所示。

行13-15 — 傳遞至此OSGi元件的程式引數會使用「，」分隔符號進行分割。 接著，系統會從字串陣列中擷取attachmentPath和saveToLocation的值。

* attachmentPath — 這是您在調適型表單中指定的相同位置，當您設定調適型表單的提交動作以叫用AEM Workflow時。 這是您希望附件相對於工作流程裝載儲存在AEM中的資料夾名稱。

* saveToLocation — 這是您希望附件儲存在AEM伺服器檔案系統中的位置。

這兩個值會以程式引數傳遞，如下面的熒幕擷圖所示。

![ProcessStep](assets/implement-process-step.gif)

QueryBuilder服務用於查詢attachmentsPath資料夾下nt：file型別的節點。 其餘程式碼會逐一瀏覽搜尋結果，以建立Document物件並將其儲存至檔案系統


>[!NOTE]
>
>由於我們使用的是AEM Forms專屬的Document物件，因此您需要在maven專案中包含aemfd-client-sdk相依性。 群組ID是com.adobe.aemfd ，而成品ID是aemfd-client-sdk。

#### 建置和部署

[依照此處的說明建置套件組合](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[請確定該套件組合已部署且處於作用中狀態](http://localhost:4502/system/console/bundles)

建立工作流程模型。 在工作流程模型中拖放流程步驟。 將程式步驟與「將最適化表單附件儲存至檔案系統」相關聯。

提供必要的程式引數，以逗號分隔。 例如Attachments，c：\\scrappp\\。 第一個引數是相較於工作流程裝載儲存最適化表單附件時的資料夾。 此值必須與您在設定最適化表單的提交動作時指定的值相同。 第二個引數是儲存附件的位置。

创建自适应表单. 將「檔案附件」元件拖放到表單上。 設定表單的提交動作，以叫用在前面步驟建立的工作流程。 提供適當的附件路徑。

儲存設定。

預覽表單。 新增一些附件並提交表單。 附件應儲存在您在工作流程中指定的位置中的檔案系統中。
