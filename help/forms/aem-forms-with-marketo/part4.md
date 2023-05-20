---
title: AEM Forms與Marketo（第4部分）
description: 使用AEM Forms表單資料模型將AEM Forms與Marketo整合的教學課程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# 使用表單資料模型建立最適化表單

下一步是建立最適化表單，並將其在上一步建立的表單資料模型作為基礎。
使用者輸入銷售機會ID，並叫用以Tab鍵瀏覽Marketo服務以透過ID取得銷售機會。 然後，服務作業的結果會對應至最適化Forms的適當欄位。

1. 建立最適化表單，並以「空白表單範本」為基礎，將其與先前步驟中建立的表單資料模型建立關聯。
1. 在編輯模式下開啟表單
1. 將TextField元件和Panel元件拖放至最適化表單上。 將TextField元件的標題設為「輸入銷售機會Id」，並將其名稱設為「LeadId」
1. 將2個TextField元件拖放到Panel元件上
1. 將2個Textfield元件的「名稱」和「標題」設定為「名字」和「姓氏」
1. 將「最小值」設定為1，「最大值」設定為–1，將「面板」元件設定為可重複的元件。 這是必要的，因為Marketo服務會傳回一系列Lead物件，而您需要有可重複的元件來顯示結果。 不過，在此案例中，我們只傳回一個Lead物件，因為我們正在依據其ID搜尋Lead物件。
1. 在LeadId欄位上建立規則，如下圖所示
1. 預覽表單，並在LeadID欄位中輸入有效的Lead ID，然後跳出。 「名字」和「姓氏」欄位應填入服務電話的結果。

下列熒幕擷圖說明規則編輯器設定

![規則編輯器](assets/ruleeditor.jfif)

## 调试

如果您使用本文提供的套件組合，建議您啟用 [偵錯記錄](http://localhost:4502/system/console/slinglog) 下列類別：

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
