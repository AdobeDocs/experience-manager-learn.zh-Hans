---
title: 使用批次API產生互動式通訊檔案
description: 使用批次API產生列印管道檔案的資產範例
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 2%

---

# 批次API

您可以使用批次API從範本產生多個互動式通訊。 範本是沒有任何資料的互動式通訊。 Batch API會將資料與範本結合，以產生互動式通訊。 此API適用於大量生產互動式通訊。 例如，電話帳單、多個客戶的信用卡對帳單。

[深入瞭解批次產生API](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供使用批次API產生互動式通訊檔案的資產範例。

## 使用Watched資料夾產生批次

* 匯入 [互動式通訊範本](assets/Beneficiaries-confirmation.zip) 至您的AEM Forms伺服器。
* 匯入 [觀察資料夾設定](assets/batch-generation-api.zip). 這將會建立名為的資料夾 `batchAPI` 在您的C磁碟機中。

**如果您在非Windows作業系統上執行AEM Forms，請遵循下列3個步驟：**

1. [開啟watched資料夾](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 選取BatchAPIWatchedFolder，然後按一下「編輯」。
3. 變更路徑以符合您的作業系統。

![路径](assets/watched-folder-batch-api-basic.PNG)

* 下載並解壓縮的內容 [zip檔案](assets/jsonfile.zip). zip檔案包含名為的資料夾 `jsonfile` 其中包含 `beneficiaries.json` 檔案。 此檔案包含產生3份檔案的資料。

* 放下 `jsonfile` 資料夾放入您的watched資料夾的輸入資料夾。
* 擷取資料夾以進行處理之後，請檢查您的watched資料夾的結果資料夾。 您應該會看到3個PDF檔案產生

## 使用REST請求產生批次

您可以叫用 [批次API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) 透過REST要求。 您可以為其他應用程式公開REST端點，以叫用API來產生檔案。
提供的資產範例會顯示用於產生互動式通訊檔案的REST端點。 此servlet接受下列引數：

* fileName — 資料檔案在檔案系統上的位置。
* templatePath - IC範本路徑
* saveLocation — 在檔案系統上儲存產生檔案的位置
* channelType — 列印、網頁或兩者
* recordId — 要設定互動式通訊名稱的元素的JSON路徑

下列熒幕擷圖顯示引數及其值
![範例要求](assets/generate-ic-batch-servlet.PNG)

## 在您的伺服器上部署範例資產

* 匯入 [ICT範本](assets/ICTemplate.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 匯入 [自訂提交處理常式](assets/BatchAPICustomSubmit.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 匯入 [最適化表單](assets/BatchGenerationAPIAF.zip) 使用 [Forms和檔案介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 部署並開始 [自訂OSGI套件組合](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) 使用 [Felix Web主控台](http://localhost:4502/system/console/bundles)
* [透過提交表單觸發批次產生](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
