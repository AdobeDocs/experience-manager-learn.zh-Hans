---
title: 整合 [!DNL ServiceNow]
description: 使用表單資料模型建立並顯示所有事件。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 2%

---

# 將AEM Forms與整合 [!DNL ServiceNow]

在中建立及顯示事件 [!DNL ServiceNow] 使用AEM Forms中的表單資料模型。

## 前提条件

* [!DNL ServiceNow] 帳戶。
* 熟悉 [建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 熟悉 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## 資產範例

本文提供的範例資產包括

* 雲端服務設定
* Swagger檔案以建立事件並擷取所有事件
* 以Swagger檔案為基礎的表單資料模型
* 要建立和清單的最適化表單 [!DNL ServiceNow] 事件

## 在您的伺服器上部署資產

* 下載 [資產範例](assets/service-now.zip)
* 使用將資產匯入AEM [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 用於這項整合的swagger檔案位於 ```/conf/9957/settings/cloudconfigs/fdm``` crx存放庫中的資料夾
* 編輯 [CreateIncident雲端服務設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)以符合您的ServiceNow執行個體。
* 編輯 [GetAllIncidents雲端服務設定](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) 以符合您的ServiceNow執行個體。 您需要變更主機、使用者名稱和密碼，以符合您的ServiceNow執行個體認證。

## 存取ServiceNow執行個體認證

* 按一下您的使用者設定檔
   ![按一下使用者設定檔](assets/snow-1.png)

* 按一下管理執行個體密碼
* 執行個體詳細資料如下所示
   ![執行個體詳細資訊](assets/snow-3.png)

## 測試整合

* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* 在說明與註解欄位中輸入一些值，然後按一下「建立事件」按鈕
* 新建立事件的事件ID應填入文字欄位中，下表應列出所有事件。
