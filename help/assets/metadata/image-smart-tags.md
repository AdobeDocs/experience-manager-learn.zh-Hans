---
title: 使用AEM Assets的影像適用的智慧標籤
description: 影像的智慧標籤可依據影像內容，自動且聰明地將中繼資料標籤新增至影像資產，藉此增強AEM搜尋功能。
topic: Content Management
feature: Smart Tags
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 645
thumbnail: 17019.jpg
last-substantial-update: 2022-06-09T00:00:00Z
exl-id: c72dc489-70e6-48ca-99a8-663d4c0652ba
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 3%

---

# 影像的智慧標籤

影像適用的AEM Assets智慧標籤可自動將衍生的中繼資料標籤新增至影像資產，讓您更輕鬆快速地找到正確的影像，進而改善撰寫體驗，藉此增強AEM Assets的搜尋功能。

>[!VIDEO](https://video.tv.adobe.com/v/17019?quality=12&learn=on)

## 設定AEM 6.x{#set-up}

>[!NOTE]
> 影像的智慧標籤會自動針對AEMas a Cloud Service布建。

>[!VIDEO](https://video.tv.adobe.com/v/17023?quality=12&learn=on)

使用Smart Content Service之前，請確定下列專案會在Adobe I/O上建立整合：

* 具有組織管理員許可權的Adobe ID帳戶
* 您的組織已啟用「智慧內容服務」

影片詳細說明設定用於智慧標籤影像的Adobe I/O智慧內容服務所需的下列工作。

* 在AEM中建立Smart Content Service設定以產生公開金鑰。 为 OAuth 集成获取公共证书。
* 在Adobe I/O中建立整合，並上傳產生的公開金鑰。
* 從Adobe I/O使用API金鑰和其他憑證設定您的AEM執行個體。
* 可選擇在資產上傳時啟用自動標籤。

## 其他资源

* [AEM Assets智慧標籤檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
