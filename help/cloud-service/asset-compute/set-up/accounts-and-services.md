---
title: 設定Asset compute擴充性的帳戶和服務
description: 開發Asset compute工作者需要存取帳戶和服務，包括AEMas a Cloud Service、App Builder，以及Microsoft或Amazon提供的雲端儲存空間。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6264
thumbnail: 40377.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 707657ad-221e-4dab-ac2a-46a4fcbc55bc
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 1%

---

# 設定帳戶和服務

本教學課程需要布建下列服務，並可透過學習者的Adobe ID存取。

所有Adobe服務都必須透過相同的Adobe組織，使用您的Adobe ID進行存取。

+ [AEM as a Cloud Service](#aem-as-a-cloud-service)
+ [App Builder](#app-builder)
   + 布建可能需要2到10天的時間
+ 雲端儲存空間
   + [Azure Blob儲存體](https://azure.microsoft.com/en-us/services/storage/blobs/)
   + 或 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card)

>[!WARNING]
>
>繼續閱讀本教學課程之前，請先確認您具備上述所有服務的存取權。
> 
> 請檢閱以下章節，瞭解如何設定及布建必要的服務。

## AEM as a Cloud Service{#aem-as-a-cloud-service}

需要存取AEMas a Cloud Service環境，才能設定AEM Assets處理設定檔以叫用自訂Asset compute背景工作。

理想情況下，沙箱計畫或非沙箱開發環境可供使用。

請注意，本機AEM SDK並不足以完成本教學課程，因為本機AEM SDK無法與Asset compute微服務通訊，而是需要真正的AEMas a Cloud Service環境。

## App Builder{#app-builder}

此 [App Builder](https://developer.adobe.com/app-builder/) framework可用來建置自訂動作並將其部署到Adobe的無伺服器平台Adobe I/O Runtime。 AEMAsset compute專案是特別建立的App Builder專案，可透過處理設定檔與AEM Assets整合，並提供存取及處理資產二進位檔的能力。

若要存取App Builder，請註冊預覽。

1. [註冊App Builder試用版](https://developer.adobe.com/app-builder/trial/).
1. 請等候約2 - 10天，直到透過電子郵件通知您已布建完成，再繼續本教學課程。
   + 如果您不確定是否已布建，請繼續進行後續步驟，如果您無法建立 __App Builder__ 專案位置 [Adobe Developer主控台](https://developer.adobe.com/console/) 您尚未布建。

## 雲端儲存空間

asset compute專案的本機開發需要雲端儲存空間。

將Asset compute背景工作部署到Adobe I/O Runtime以供AEMas a Cloud Service直接使用時，此雲端儲存空間並非絕對必要，因為AEM會提供用來讀取資產並寫入轉譯的雲端儲存空間。

### Microsoft Azure Blob儲存體{#azure-blob-storage}

如果您還沒有Microsoft Azure Blob儲存體的存取權，請註冊 [12個月免費帳戶](https://azure.microsoft.com/en-us/free/).

不過，本教學課程將使用Azure Blob儲存體 [Amazon S3](#amazon-s3) 也只能用於教學課程的微幅變化。

>[!VIDEO](https://video.tv.adobe.com/v/40377?quality=12&learn=on)

_布建Azure Blob Storage的點進（無音訊）_

1. 登入您的 [Microsoft Azure帳戶](https://azure.microsoft.com/en-us/account/).
1. 導覽至 __儲存體帳戶__ Azure服務區段
1. 點選 __+新增__ 建立新的Blob儲存體帳戶的方式
1. 建立新的 __資源群組__ 如有需要，例如： `aem-as-a-cloud-service`
1. 提供 __儲存體帳戶名稱__&#x200B;例如： `aemguideswkndassetcomput`
   + 此 __儲存體帳戶名稱__  用於 [設定雲端儲存空間](../develop/environment-variables.md) 在本機Asset compute開發工具中
   + 此 __存取金鑰__ 在以下情況下，也需要與儲存體帳戶相關聯： [設定雲端儲存空間](../develop/environment-variables.md).
1. 保留其他專案為預設值，然後點選 __檢閱+建立__ 按鈕
   + 或者，選取 __位置__ 靠近您。
1. 檢閱布建請求以瞭解正確性，然後點選 __建立__ 按鈕（若已滿足）

### Amazon S3{#amazon-s3}

使用 [Microsoft Azure Blob儲存體](#azure-blob-storage) 不過，建議您完成本教學課程 [Amazon S3](https://aws.amazon.com/s3/?did=ft_card&amp;trk=ft_card) 也可以使用。

如果使用Amazon S3儲存空間，請在以下情況下指定Amazon S3雲端儲存空間認證： [設定專案的環境變數](../develop/environment-variables.md#amazon-s3).

如果您需要針對本教學課程布建雲端儲存空間，建議使用 [Azure Blob儲存體](#azure-blob-storage).
