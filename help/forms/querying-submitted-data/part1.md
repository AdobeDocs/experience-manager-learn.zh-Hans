---
title: 具有JSON結構描述和資料的AEM Forms[第1部分]
seo-title: AEM Forms with JSON Schema and Data[Part1]
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
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# 根據JSON結構描述建立最適化表單


AEM Forms 6.3版中引進了根據JSON結構描述建立最適化Forms的功能。 有關使用JSON結構描述建立最適化Forms的詳細資訊，請參閱本節 [文章](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

一旦您根據JSON結構描述建立了調適型表單，下一步就是將提交的資料儲存在資料庫中。 為此，我們將使用各種資料庫廠商推出的新JSON資料型別。 就本文而言，我們將使用MySql 8資料庫來儲存提交的資料。

本文使用了MySql 8資料庫。 MySQL匯入了名為的新資料型別 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). 這可讓您更輕鬆地儲存和查詢JSON物件。 我們會將提交的資料儲存在資料庫中型別為JSON的欄中。

以下熒幕擷圖顯示以JSON資料型別儲存的已提交表單資料。 「formdata」欄的型別為JSON。 我們還將與資料相關聯的表單名稱儲存在欄表單名稱中

>[!NOTE]
>
>請確定您的json結構描述檔案已適當命名。 例如，它需要以下列格式命名 &lt;name>schema.json. 因此您的結構描述檔案可以是mortgage.schema.json或credit.schema.json。


![資料儲存](assets/datastored.gif)


[可用來建立最適化Forms的JSON結構描述範例。](assets/samplejsonschemas.zip). 下載並解壓縮zip檔案以取得JSON結構描述
