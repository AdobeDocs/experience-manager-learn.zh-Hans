---
title: 將資產部署在本機
description: 在本機AEM執行個體上部署教學課程資產
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---

# 在您的系統上部署

請依照下列步驟，讓此使用案例在本機AEM執行個體上運作。

* [部署DevelopingWithServiceUser套裝](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) 包含在zip檔案中。

* 在Apache Sling服務使用者對應程式服務中新增以下專案 **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service** 使用 [configMgr](http://localhost:4502/system/console/configMgr).

* [部署電子報套裝](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 此套件組合包含列出資料夾內容與組合所選電子報的程式碼。

* [使用封裝管理程式匯入封裝](assets/newsletter.zip). 此套件包含使用者端程式庫和測試解決方案的範例pdf檔案。

* [匯入最適化表單範例](assets/sample-adaptive-form.zip). 此表單將列出可選取的新聞稿。

* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
選取要下載的電子報。選取的電子報將合併成一個pdf並傳回給您。
