---
title: 瞭解AEM中的Sling模型匯出工具
description: Apache Sling Model 1.3.0引入了Sling模型匯出工具，這是一種將Sling模型物件匯出或序列化為自訂抽象的簡潔方法。 本文將使用Sling模型填入HTL指令碼的傳統使用案例並置，利用Sling模型匯出工具框架將Sling模型序列化為JSON。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# 瞭解 [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0引入 [!DNL Sling Model Exporter]，優雅的匯出或序列化方式 [!DNL Sling Model] 物件放入自訂抽象概念。 本文將使用的傳統使用案例並列 [!DNL Sling Models] 填入HTL指令碼，利用 [!DNL Sling Model Exporter] 序列化a的框架 [!DNL Sling Model] 轉換為JSON。

## 傳統Sling模型HTTP請求流程

的傳統使用案例 [!DNL Sling Models] 是為資源或請求提供業務抽象，這提供HTL指令碼（或以前的JSP）用於存取業務功能的介面。

正在開發常見模式 [!DNL Sling Models] 代表AEM元件或頁面，並使用 [!DNL Sling Model] 物件，用來向HTL指令碼提供資料，以及顯示在瀏覽器中的HTML結束結果。

### Sling模型HTTP請求流程

![Sling模型請求流程](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] 向AEM中的資源提出請求。

   示例: `HTTP GET /content/my-resource.html`

1. 根據請求資源的 `sling:resourceType`，則會解析適當的指令碼。

1. 指令碼會將請求或資源調整至所需的內容 [!DNL Sling Model].

1. 指令碼會使用 [!DNL Sling Model] 物件以產生HTML轉譯。

1. 指令碼產生的HTML會傳回HTTP回應中。

此傳統模式在產生HTML的情境下運作良好，例如 [!DNL Sling Model] 可透過HTL輕鬆運用。 建立更具結構化的資料（例如JSON或XML）是一項非常枯燥的工作，因為HTL本身並不適合這些格式的定義。

## [!DNL Sling Model Exporter] HTTP要求流程

Apache [!DNL Sling Model Exporter] 隨附於Sling提供的Jackson Exporter，可自動序列化 [!DNL Sling Model] 物件併入JSON。 Jackson Exporter雖然相當可設定，但其核心會檢查 [!DNL Sling Model] 物件，並使用任何「getter」方法作為JSON索引鍵產生JSON，而getter傳回值作為JSON值。

的直接序列化 [!DNL Sling Models] 可讓他們透過使用傳統方式來建立的HTML回應，為一般網頁請求提供服務 [!DNL Sling Model] 請求流程（請參閱上文），但也會公開Web服務或JavaScript應用程式可使用的JSON轉譯。

![Sling模型匯出程式HTTP要求流程](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*此流程說明使用提供的Jackson Exporter產生JSON輸出的流程。 自訂匯出工具的使用遵循相同的流程，但採用其輸出格式。*

1. 在AEM中對資源發出HTTPGET請求，選擇器和擴充功能註冊到 [!DNL Sling Model]的匯出工具。

   示例: `HTTP GET /content/my-resource.model.json`

1. Sling解析請求資源的 `sling:resourceType`，此元件為動態產生的Sling Exporter Servlet的選擇器和擴充功能，此元件已對應至 [!DNL Sling Model] 使用匯出程式。
1. 已解析的Sling匯出程式Servlet會叫用 [!DNL Sling Model Exporter] 針對 [!DNL Sling Model] 從請求或資源改寫的物件（由Sling模型改寫決定）。
1. 匯出工具會序列化 [!DNL Sling Model] 會根據「匯出工具選項」和「匯出工具專用」的Sling模型註解，並將結果傳回Sling匯出工具Servlet。
1. Sling Exporter Servlet傳回 [!DNL Sling Model] HTTP回應中的。

>[!NOTE]
>
>而Apache Sling專案提供可序列化的Jackson Exporter [!DNL Sling Models] 對於JSON，匯出工具架構也支援自訂匯出工具。 例如，專案可實作自訂匯出工具，將 [!DNL Sling Model] 轉換為XML。

>[!NOTE]
>
>不僅如此 [!DNL Sling Model Exporter] *序列化* [!DNL Sling Models]，也可以將其匯出為Java物件。 匯出至其他Java物件在HTTP請求流程中不會發揮作用，因此不會出現在上圖中。

## 支援材料

* [Apache [!DNL Sling Model Exporter] 框架檔案](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
