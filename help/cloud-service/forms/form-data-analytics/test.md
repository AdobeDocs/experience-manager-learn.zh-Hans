---
title: 使用Adobe Analytics提交表單資料欄位的相關報告
description: 將AEM Forms CS與Adobe Analytics整合以報告表單資料欄位
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# 測試您的解決方案

使用多種表單值組合，預覽及提交您的表單。 請等數到30分鐘，在Adobe Analytics報表中檢視您的資料。 設定為Prop的資料比設定為eVar的資料更早出現在報表中。

## 报表包

Adobe Analytics中擷取的表單資料會以環形圖格式呈現

**依州的提交內容**

![applicantsbystate](assets/donut.png)

欄位驗證錯誤

![field-validation-error](assets/donut-field-validation.png)

## 调试

確認最適化表單使用的設定容器與包含Adobe啟動設定的設定容器相同。

若要確認表單正在將資料傳送至Adobe Analytics，請執行下列動作

* 在瀏覽器中開啟開發人員工具。
* 在Console面板的下列文字中輸入。

```javascript
_satellite.setDebug(true)
```

與表單互動，同時保持主控台視窗開啟。 您應該會看到類似這樣的內容

![console-debug](assets/debug.png)

## 使用Adobe Experience Platform Debugger

新增 [AEP偵錯工具擴充功能](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) （您必須登入）才能取得更多偵錯資訊

![platform-debugger](assets/platform-debugger.png)
