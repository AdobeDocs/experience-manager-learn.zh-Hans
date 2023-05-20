---
title: 設定AEM資料來源
description: 設定MySQL支援的資料來源以儲存和擷取表單資料
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 5%

---

# 設定資料來源

AEM有許多方式可用來與外部資料庫整合。 整合資料庫的最常見方式之一，是透過使用Apache Sling Connection Pooled DataSource [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下載並部署適當的 [MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 在AEM中。
建立Apache Sling Connection Pooled DataSource並提供以下熒幕擷取畫面中指定的屬性。 資料庫結構描述會作為本教學課程資產的一部分提供給您。

![data-source](assets/data-source.PNG)

資料庫有一個名為formdata的表格，包含3欄，如下面的熒幕擷取畫面所示。

![資料庫](assets/data-base.PNG)


>[!NOTE]
>請務必為您的資料來源命名 **aemformstutorial**. 範常式式碼會使用名稱來連線至資料庫。

| 属性名称 | 价值 |
| ------------------------|--------------------------------------- |
| 資料來源名稱 | SaveAndContent |
| JDBC驅動程式類別 | com.mysql.cj.jdbc.Driver |
| JDBC連線URI | jdbc:mysql://localhost：3306/aemformstutorial |

## Assets

建立結構描述的SQL檔案可以是 [已從此處下載](assets/sign-multiple-forms.sql). 您必須使用MySql Workbench匯入此檔案，才能建立結構描述和表格。

## 后续步骤

[建立OSGi服務以儲存和擷取資料庫中的資料](./create-osgi-service.md)
