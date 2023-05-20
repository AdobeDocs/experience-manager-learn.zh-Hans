---
title: 從MySQL資料庫儲存和擷取含有附件的表單資料
description: 多部分教學課程，逐步引導您完成儲存和擷取含有附件的表單資料的步驟
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# 使用2FA儲存及擷取最適化表單資料

本教學課程將逐步引導您完成使用2FA儲存和擷取含有附件的最適化表單資料的步驟。 本教學課程使用MySQL資料庫來儲存最適化表單資料。 只要您已在AEM中部署資料庫特定驅動程式，就可以使用您選擇的資料庫來儲存資料。 概略來說，若要完成使用案例，必須執行下列步驟：

* 使用GuideBridge API存取最適化表單資料

* 對servlet進行POST呼叫。 此servlet會將資料儲存在資料庫中，並將表單附件儲存在CRX存放庫中。 資料庫中儲存的資料會與GUID相關聯。

* 當您想要使用儲存的資料填入調適型表單時，您會擷取與GUID相關聯的資料，並使用 **request.setAttribute** 方法。

## 使用案例示範

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 前提条件

此內容的對象應具有下列領域的一些經驗：

* 最適化表單
* 表单数据模型
* OSGi服務/元件
* AEM使用者端資料庫


## 后续步骤

[設定資料來源](./configure-data-source.md)