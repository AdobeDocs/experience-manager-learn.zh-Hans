---
title: 新增自訂欄
description: 新增自訂欄以顯示工作流程的其他資料
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# 新增自訂欄

若要在收件匣中顯示工作流程資料，我們必須在工作流程中定義和填入變數。 必須先設定變數的值，才能將任務指派給使用者。 為了讓您開一個好頭，我們提供了範例工作流程，可以部署在您的AEM伺服器上。

* [登入AEM](http://localhost:4502/crx/de/index.jsp)
* [匯入稽核工作流程](assets/review-workflow.zip)
* [檢閱工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

此工作流程有兩個已定義的變數（isMarried和income），其值是使用設定變數元件設定的。 這些變數可作為要新增至AEM收件匣的欄

## 建立服務

對於每個需要顯示在收件匣中的欄，我們需要編寫服務。 下列服務可讓我們新增欄以顯示isMarried變數的值

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>您必須在專案中加入AEM 6.5.5 Uber.jar，上述程式碼才能運作

![uber-jar](assets/uber-jar.PNG)

## 在您的伺服器上測試

* [登入AEM Web主控台](http://localhost:4502/system/console/bundles)
* [部署並啟動收件匣自訂套裝](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [開啟您的收件匣](http://localhost:4502/aem/inbox)
* 按一下以開啟Admin Control _清單檢視_ 圖示旁邊 _建立_ 按鈕
* 將「已婚」欄新增至「收件匣」並儲存變更
* [前往FormsAndDocuments UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [匯入範例表單](assets/snap-form.zip) 藉由選取 _檔案上傳_ 從 _建立_ 功能表
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* 選取 _婚姻狀況_ 並提交表單
   [檢視收件匣](http://localhost:4502/aem/inbox)

提交表單會觸發工作流程，且任務會指派給「管理員」使用者。 您應該會在「已婚」欄下看到值，如本熒幕擷取畫面所示

![已婚欄位](assets/married-column.PNG)

## 后续步骤

[顯示已婚欄](./use-sightly-template.md)
