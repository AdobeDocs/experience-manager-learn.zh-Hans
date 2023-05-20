---
title: 顯示多個pdf檔案
description: 在最適化表單中循環瀏覽多個pdf檔案。
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 4%

---

# 以傳送方式顯示多個pdf檔案

常見的使用案例是在提交表單前，將多個PDF檔案顯示給表單填寫工具以供稽核。

為了完成此使用案例，我們利用 [Adobe PDF內嵌API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[您可以在這裡體驗此範例的即時示範。](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

已執行下列步驟以完成整合

## 建立自訂元件以顯示多個PDF檔案

已建立自訂元件(pdf-carousel)來循環pdf檔案

## 客户端库

已建立使用者端資料庫，以使用Adobe PDF Embed API顯示PDF。 要顯示的PDF會在pdf轉盤元件中指定。

## 建立最適化表單

使用某些標籤建立最適化表單（此範例有3個標籤）在前兩個標籤中新增一些最適化表單元件在第三個標籤中新增pdf轉盤元件設定pdf轉盤元件，如以下熒幕擷圖所示
![pdf-carousel](assets/pdf-carousel-af-component.png)

**內嵌PDFAPI金鑰**  — 這是可用來內嵌pdf的金鑰。 此金鑰僅適用於localhost。 您可以建立 [您自己的金鑰](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 並將其與其他網域相關聯。

**指定PDF檔案**  — 您可在此處指定要在轉盤中顯示的pdf檔案。


## 在您的伺服器上部署範例

若要在本機伺服器上測試此專案，請遵循下列步驟：

1. [匯入使用者端資源庫](assets/pdf-carousel-client-lib.zip) 至您的本機AEM執行個體 [使用封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
1. [匯入pdf轉盤元件](assets/pdf-carousel-component.zip) 至您的本機AEM執行個體 [使用封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
1. [匯入最適化表單 ](assets/adaptive-form-pdf-carousel.zip) 至您的本機AEM執行個體 [使用封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
1. [匯入範例pdf以顯示](assets/pdf-carousel-sample-documents.zip) 至您的本機AEM執行個體 [使用資產檔案上傳連結](http://localhost:4502/assets.html/content/dam)
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 按Tab鍵至「要檢閱的檔案」標籤。 您應該會在轉盤元件中看到三份PDF檔案。
