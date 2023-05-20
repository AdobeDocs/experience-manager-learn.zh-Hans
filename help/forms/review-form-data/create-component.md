---
title: 建立元件以列出表單資料
description: 有關建立摘要元件的教學課程，用於在提交前稽核表單資料。
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# 建立元件以彙總表單資料

已建立簡單元件以列出表單資料以供檢閱。 此 [guidebridge API的造訪功能](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) 用於循環檢視表單欄位。 與此元件相關聯的clientlibrary程式碼會取得表單上的面板/表格元件。 此面板/表格元件表單欄位標題、值和SOM運算式的子元素是使用GuidBridge API方法擷取。 然後會使用標題、值和SOM運算式來建構簡單的HTML表格，供一般使用者在提交表單之前檢閱/編輯表單資料。

例如，下面的熒幕擷圖顯示您建立的表格，其中列出了 **您的詳細資料**. TR中的最後一個TD是用來編輯欄位的值，使用欄位SOM運算式。

![造訪函式](assets/visit-function.png)

## 后续步骤

[在本機系統上測試解決方案](./deploy-on-your-system.md)
