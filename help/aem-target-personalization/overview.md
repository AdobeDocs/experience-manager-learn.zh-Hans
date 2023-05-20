---
title: AEM 和 Adobe Target 使用入门
description: 端對端教學課程，說明如何使用Adobe Experience Manager和Adobe Target建立和提供個人化體驗。 在本教學課程中，您還將瞭解端對端流程中涉及的不同角色，以及他們如何彼此合作
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---

# AEM 和 Adobe Target 使用入门 {#getting-started-with-aem-target}

AEM和Target都是強大的解決方案，而且功能似乎重疊。 客戶有時很難瞭解搭配使用這些產品來提供個人化體驗的方式和時機。 為了提供最佳化體驗給每位一般使用者，組織內不同的團隊應密切合作，並定義各自的職責。

在本教學課程中，我們會介紹AEM和Target的三種不同案例，可幫助您瞭解哪些對您的組織最合適，以及不同團隊如何共同作業。

* 案例1 ：使用AEM體驗片段進行個人化
* 案例2 ：使用視覺化體驗撰寫器進行個人化
* 案例3 ：個人化完整網頁體驗

## 使用AEM體驗片段進行個人化 {#personalization-using-aem-experience-fragment}

針對此案例，我們將使用AEM和Target。 很明顯這兩種產品都有各自的優點，而且需要為網站使用者提供個人化體驗 **個人化內容(來自AEM的內容)** 和 **intelligent way (Target)** 根據特定使用者提供這些內容。

AEM可協助您建立個人化內容，將您的所有內容和資產整合在一個中央位置，為您的個人化策略提供助力。 AEM可讓您在單一位置輕鬆建立桌上型電腦、平板電腦和行動裝置的內容，而不需撰寫程式碼。 不需要為每個裝置建立頁面，AEM會自動使用您的內容調整每個體驗。 您也可以透過按一下按鈕，將內容從AEM匯出至Adobe Target作為選件。

我們現在在Target以AEM優惠方案的形式提供個人化內容。 Target可讓您根據結合行為、情境和離線變數的規則型和AI驅動機器學習方法的組合，大規模提供這些選件。  透過Target，您可以輕鬆設定和執行A/B及多變數(MVT)活動，以決定最佳選件、內容和體驗。

**體驗片段** 將內容/體驗建立者連結至使用Target推動業務成果的個人化專業人員，可謂是跨出一大步。

* AEM內容編輯器作者將內容個人化成體驗片段及其變數
* AEM會將體驗片段HTML匯出至&#x200B;Target
* Target會&#x200B;使用AEM體驗片段標籤作為活動中的選件
* Target提供體驗片段HTML，AEM提供參照的影像

   ![使用體驗片段圖表進行個人化](assets/personalization-use-case-1/use-case-1-diagram.png)

**若要實作此案例，您需要：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)
* [使用舊版Cloud Services的AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實施上述整合後，讓我們探索 [詳細情境](./personalization-use-case-1.md).***

## 使用視覺化體驗撰寫器進行個人化

行銷人員無需變更任何程式碼，即可快速變更其網站，以使用Adobe Target視覺化體驗撰寫器(VEC)執行測試。 VEC是WYSIWYG （您所見即所得）使用者介面，可讓您輕鬆建立及測試網站內容中的個人化體驗和選件。 您可以拖放、交換及修改網頁（或選件）或行動網頁的版面和內容，以建立Target活動的體驗和選件。

VEC是Adobe Target的主要功能之一。 VEC可讓行銷人員和設計人員使用視覺化介面來建立和變更內容。 您可以做出許多設計選擇，而不需要直接編輯程式碼。 您也可以使用撰寫器中提供的編輯選項，編輯HTML和JavaScript。

* 內容駐留在AEM中，內容編輯人員可建立和管理網站頁面
* Target使用AEM代管的網站頁面來執行測試和個人化
* Target提供個人化內容
* 使用Adobe Target VEC建立全新內容
* 同時適用於AEM託管網站和非AEM託管網站

   ![使用視覺化體驗撰寫器圖表進行個人化](assets/personalization-use-case-3/use-case-diagram-3.png)

**若要實作此案例，您需要：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實施上述整合後，讓我們探索 [情境詳細資料。](./personalization-use-case-3.md)***

## 個人化完整網頁體驗

將Adobe Experience Manager與Adobe Target整合可協助您為網站使用者提供個人化體驗。 此外，它還有助於您更清楚瞭解網站內容的哪些版本最能改善指定測試期間的轉換。 例如，A/B測試會比較兩個或更多版本的網站內容，以檢視哪些內容最能提高您的轉換、銷售或您識別的其他量度。 行銷人員可以在Adobe Target中建立活動，以瞭解使用者如何與您的網站內容互動，以及互動如何影響您的網站量度。

* 內容駐留在AEM中，內容編輯人員可建立和管理網站頁面
* Target使用AEM代管的網站頁面來執行測試和個人化
* Target提供個人化內容
* 此處未建立任何全新內容
* 同時適用於AEM和非AEM網站

   ![圖表](assets/personalization-use-case-2/use-case-2-diagram.png)

**若要實作此案例，您需要：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實施上述整合後，讓我們探索 [情境詳細資料。](./personalization-use-case-2.md)***
