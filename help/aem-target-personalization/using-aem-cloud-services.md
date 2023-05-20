---
title: 使用Cloud Services整合Adobe Experience Manager與Adobe Target
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: 如何使用AEM Cloud Service將Adobe Experience Manager (AEM)與Adobe Target整合的分步逐步解說
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---

# 使用AEM舊版Cloud Services

在本節中，我們將討論如何使用舊版Cloud Services透過Adobe Target設定Adobe Experience Manager (AEM)。

>[!NOTE]
>
> 搭配Adobe Target的AEM舊版Cloud Service為 **僅限** 用來建立與Adobe Target的直接AEM Author後端連線，方便將內容從AEM發佈到Target。 Adobe Launch是用來在公開的AEM所提供的網站體驗上公開Adobe Target。

為了使用AEM體驗片段選件來強化您的個人化活動，請前往下一章，並使用舊版雲端服務整合AEM與Adobe Target。 從AEM以HTML/JSON選件的形式將體驗片段推送到Target，以及保持Target選件與AEM同步時，需要這項整合。 實作時需要這項整合 [概觀一節中討論的情境1](./overview.md#personalization-using-aem-experience-fragment).

## 前提条件

* **AEM**

   * 完成本教學課程需要AEM作者和發佈執行個體。 如果您尚未設定AEM執行個體，您可以依照步驟進行 [此處](./implementation.md#set-up-aem).

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 布建了下列解決方案的Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 客戶需要布建以下Experience Platform Launch和Adobe I/O： [Adobe支援](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html) 或聯絡您的系統管理員


### 將AEM與Adobe Target整合

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用Adobe IMS驗證建立Adobe TargetCloud Service(*使用Adobe Target API*) (00:34)
2. 取得Adobe Target使用者端代碼(01:50)
3. 建立適用於Adobe Target的Adobe IMS設定(02:08)
4. 建立技術帳戶以存取Adobe I/O控制檯中的Target API (02:08)
5. 將Adobe TargetCloud Service新增至AEM體驗片段(04:12)

此時，您已成功整合 [AEM與Adobe Target搭配使用舊版Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) 如選項2中詳述。 您現在應該能夠在AEM中建立體驗片段，並將體驗片段發佈為HTML選件或JSON選件至Adobe Target，然後可以使用建立活動。
