---
title: 從提交的資料xml擷取節點
description: 自訂程式步驟，將位於承載資料夾下的寫入檔案新增至檔案系統
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# 從提交的資料xml擷取節點

此自訂流程步驟是從另一個xml檔案中擷取節點，以建立新的xml檔案。 當您想要將提交的資料與xdp範本合併以產生pdf時，需要使用此選項。 例如，當您提交最適化表單時，您需要與xdp範本合併的資料位於資料元素內。 在這種情況下，您需要透過擷取適當的資料元素來建立另一個xml檔案。

以下熒幕擷圖顯示您需要傳遞至自訂流程步驟的引數
![process-step](assets/create-xml-process-step.png)
以下是引數
* Data.xml — 您要從中擷取節點的xml檔案
* datatomerge.xml — 使用擷取的節點建立的新xml
* /afData/afUnboundData/data — 要擷取的節點


以下熒幕擷圖顯示正在裝載資料夾下建立的datamerge.xml
![create-xml](assets/create-xml.png)

[自訂套件組合可從這裡下載](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
