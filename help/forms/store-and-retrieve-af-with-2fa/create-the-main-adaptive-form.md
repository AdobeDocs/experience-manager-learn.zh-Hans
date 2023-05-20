---
title: 建立主要的最適化表單
description: 建立最適化表單以擷取應徵者資訊，並建立最適化表單以擷取已儲存的最適化表單
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 建立主要的最適化表單

表單 **StoreAFWithAttachments** 是主要的最適化表單。 此最適化表單是使用案例的入口點。 在此表單中，會擷取使用者詳細資訊，包括行動電話號碼。 此表單也可新增一些附件。 按一下「儲存並退出」按鈕時，會執行伺服器端程式碼以將表單資料儲存在資料庫中，並產生唯一的應用程式ID並將其呈現給使用者以便安全儲存。 此應用程式ID可用來擷取與應用程式相關聯的行動電話號碼。

![主要應用程式表單](assets/6552.JPG)

此表單已與 **bootboxjs540、storeAFWithAttachments** 在課程中先前建立的使用者端程式庫，以及在提交表單時觸發的AEM工作流程。


* 範例表單是根據 [自訂自適應表單範本](assets/custom-template-with-page-component.zip) 這些資料需要匯入至AEM，範例表單才能正確呈現。

* 已完成 [StoreAfWithAttachments表單](assets/store-af-with-attachments-form.zip) 可下載並匯入您的AEM執行個體。

* 此 [與此表單關聯的AEM工作流程](assets/workflow-model-store-af-with-attachments.zip) 需要匯入至您的AEM執行個體，表單才能運作。


## 后续步骤

[建立表單擷取儲存的表單](./retrieve-saved-form.md)