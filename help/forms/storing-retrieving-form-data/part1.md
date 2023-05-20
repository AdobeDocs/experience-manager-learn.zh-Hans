---
title: 儲存和擷取MySQL資料庫的表單資料 — 設定資料來源
description: 多部分教學課程，逐步引導您完成儲存和擷取表單資料的相關步驟
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 4%

---

# 設定資料來源

AEM有許多方式可用來與外部資料庫整合。 資料庫整合最常見和標準的做法之一，就是透過以下方式使用Apache Sling Connection Pooled DataSource設定屬性： [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下載並部署適當的 [MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 在AEM中。
建立Apache Sling Connection Pooled DataSource並提供以下熒幕擷取畫面中指定的屬性。 資料庫結構描述會作為本教學課程資產的一部分提供給您。

![data-source](assets/save-continue.PNG)

資料庫有一個名為formdata的表格，包含3欄，如下面的熒幕擷取畫面所示。

![資料庫](assets/data-base-tables.PNG)

建立結構描述的sql檔案可以是 [已從此處下載](assets/form-data-db.sql). 您必須使用MySql Workbench匯入此檔案，才能建立結構描述和表格。

>[!NOTE]
>請務必為您的資料來源命名 **SaveAndContent**. 範常式式碼會使用名稱來連線至資料庫。

| 属性名称 | 价值 |
| ------------------------|---------------------------------------|
| 資料來源名稱 | SaveAndContent |
| JDBC驅動程式類別 | com.mysql.cj.jdbc.Driver |
| JDBC連線URI | jdbc:mysql://localhost：3306/aemformstutorial |
