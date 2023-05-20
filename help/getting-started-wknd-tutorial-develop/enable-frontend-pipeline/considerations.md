---
title: 開發考量事項
description: 啟用前端管道後，請考量對前端和後端開發流程的影響。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 開發考量事項

啟用前端管道後，僅在AEMas a Cloud Service環境中部署前端資源，對本機AEM開發有一些影響，您必須調整Git分支模型。

## 目标

* 如何擁有順暢的前端與後端開發流程
* 檢閱完整棧疊和前端管道之間的相依性


## 本機開發考量事項

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## 調整後的開發方法

* 對於使用AEM SDK的本機開發，後端開發團隊仍然需要透過以下方式產生clientlib `ui.frontend` 模組，但在Cloud Manager部署到AEMas a Cloud Service環境期間您必須略過。 這顯示如何隔離中概述的專案設定變更的難題 [更新專案](update-project.md) 章節。

A __解決方案__ 可能是調整您的Git分支模型，並確保AEM專案設定變更絕不會回到 __本機開發__ AEM後端開發人員使用的分支。


* 如果您匯入新元件或更新同時變更的現有元件，作為AEM專案持續增強功能的一部分 `ui.app` 和 `ui.frontend` 模組，您必須執行完整棧疊和前端管道。
