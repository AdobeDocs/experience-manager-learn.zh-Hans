---
title: 在AEM Forms CS中使用批次API產生檔案
description: 設定並觸發批次作業以產生檔案。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 14%

---

# 简介

批次請求是指一次產生數十、數百或數千份類似檔案的位置。 範例：財務公司可能會產生信用卡對帳單，以傳送給其所有客戶。
批次API （非同步API）適用於排程的高輸送量多檔案產生使用案例。 这些 API 会批量生成文档。例如，每月生成的电话帐单、信用卡对帐单和收益对帐单。

若要使用AEM Forms CS批次作業API，需要下列設定

1. 設定Azure儲存體帳戶
1. 建立Azure儲存備份雲端設定
1. 建立批次資料存放區設定
1. 執行批次API

建議您先熟悉 [API檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) 再繼續使用本教學課程。
