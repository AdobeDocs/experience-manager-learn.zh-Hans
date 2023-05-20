---
title: 將工具列的下一個和上一個按鈕分開
description: 空白顯示工具列按鈕
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 空白顯示工具列按鈕

當您在AEM Forms的工具列中新增「下一個」和「上一個」按鈕時，依照預設，這些按鈕會彼此相鄰。 您可能想要將「下一步」按鈕推到工具列的最右側，同時保留左側的「上一步/上一步」按鈕

![工具列間距](assets/toolbar-spacing.png)


## 設定工具列樣式

使用樣式編輯器可輕鬆完成上述使用案例。 將「上一個/下一個」按鈕新增至工具列後，請確定您已從「編輯」選單中選取「樣式」圖層。 選取樣式模式後，選取工具列以開啟其樣式設定屬性表。 展開「Dimension和位置」區段，確認您看到所有屬性。 設定下列屬性
* Dimension和位置
   * 寬度：100%
   * 位置：相對

儲存您的變更

## 設定「下一步」按鈕的樣式

選取「下一步」按鈕，並確定您開啟了「下一步」按鈕（而不是「下一步」按鈕文字）的樣式特性表。 設定下列屬性
* Dimension和位置
   * 位置：絕對頂端1px右端1px
* 边框
   * 邊框半徑：4px（上、右、下、左）

儲存您的變更
