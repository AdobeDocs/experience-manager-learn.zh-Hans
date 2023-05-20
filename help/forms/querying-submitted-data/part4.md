---
title: 具有JSON結構描述和資料的AEM Forms[Part4]
seo-title: AEM Forms with JSON Schema and Data[Part4]
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
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 查詢提交的資料


下一步是查詢提交的資料並以表格方式顯示結果。 我們使用以下軟體來完成這項作業：

[Querybuilder](https://querybuilder.js.org/)  — 建立查詢的UI元件

[資料表格](https://datatables.net/) — 以表格方式顯示查詢結果。

已建置下列UI以啟用查詢提交的資料。 只有在JSON結構描述中標籤為必要的元素才可供查詢。 在下方熒幕擷圖中，我們正在查詢deliverypref為SMS的所有提交內容。

用於查詢提交資料的範例UI未使用QueryBuilder中所有可用的進階功能。 建議您自行嘗試。

![查询生成器](assets/querybuilderui.gif)

>[!NOTE]
>
>本教學課程的目前版本不支援查詢多個欄。

當您選取要執行查詢的表單時，會呼叫GET **/bin/getdatakeysfromschema**. 此GET呼叫會傳回與表單結構描述相關聯的必填欄位。 必要欄位會填入QueryBuilder的下拉式清單中，供您建立查詢。

下列程式碼片段會呼叫JSONSchemaOperations服務的getRequiredColumnsFromSchema方法。 我們會將結構描述的屬性和必要元素傳遞至此方法呼叫。 然後，會使用此函式呼叫傳回的陣列來填入查詢產生器下拉式清單

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

按一下GetResult按鈕時，會進行Get呼叫 **&quot;/bin/querydata&quot;**. 我們透過查詢引數，將QueryBuilder UI建立的查詢傳遞給servlet。 然後，Servlet會將此查詢訊息化為可用來查詢資料庫的SQL查詢。 例如，如果您要搜尋以擷取所有名為「Mouse」的產品，則查詢產生器查詢字串為 `$.productname = 'Mouse'`. 然後，此查詢將轉換為以下內容

選取 &#42; 來自aemformswithjson 。  formsubmissions，其中JSON_EXTRACT( formsubmissions .formdata，&quot;$.productName &quot;)= &#39;Mouse&#39;

然後會傳回此查詢的結果，以填入UI中的表格。

若要在本機系統上執行此範例，請執行下列步驟

1. [請確定您已依照這裡所述的所有步驟進行](part2.md)
1. [使用AEM封裝管理員匯入Dashboardv2.zip。](assets/dashboardv2.zip) 此套件包含查詢資料所需的所有套件組合、組態設定、自訂提交和範例頁面。
1. 使用範例json結構描述建立最適化表單
1. 設定最適化表單以提交至「customsubmithelpx」自訂提交動作
1. 填寫表單並提交
1. 將瀏覽器指向 [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. 選取表單並執行簡單查詢
