---
title: 搭配Acrobat Sign使用AEM Forms
description: Acrobat Sign和AEM Forms可自動化複雜的交易，並加入法律電子簽章，以提供順暢的數位體驗。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 11%

---

# 搭配Acrobat Sign使用AEM Forms

Acrobat Sign可啟用最適化表單的電子簽章工作流程。 电子签名改进了法律、销售、工资单、人力资源管理和其他许多方面的文档的处理工作流。AEM Forms與Acrobat Sign之間的整合可讓您進行下列工作

* 使用最適化Forms來擷取資料，並顯示自動產生的記錄檔案(DoR)以供簽名
* 根據您的PDF範本建立最適化Forms。 將資料與pdf範本合併，並顯示相同的資料以供簽名
* 使用「簽署檔案」工作流程元件傳送檔案以供簽署

## 前提条件

您需要下列專案才能將Acrobat Sign與AEM Forms整合：

* 啟用SSL的AEM Forms伺服器
* 作用中的Acrobat Sign開發人員帳戶。
* Acrobat Sign API應用計畫
* Acrobat Sign API應用程式的認證（使用者端ID和使用者端密碼）。
