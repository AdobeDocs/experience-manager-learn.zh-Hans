---
title: 在AEM Forms中使用Watched資料夾
description: 在AEM Forms中設定及使用watched資料夾
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# 在AEM Forms中使用Watched資料夾{#using-watched-folders-in-aem-forms}

管理員可以設定網路資料夾（稱為Watched資料夾），以便在使用者將檔案(例如PDF檔案)放入Watched資料夾時，會啟動預先設定的工作流程、服務或指令碼操作，以處理新增的檔案。 服務執行指定的操作後，會將結果檔案儲存在指定的輸出資料夾中。 如需工作流程、服務和指令碼的詳細資訊。

若要進一步瞭解如何建立Watched資料夾， [按一下這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Watched資料夾用於以批次模式產生檔案。 使用watched資料夾機制，您可以為列印通道產生互動式通訊，或使用輸出服務將資料與範本合併。

本文將涵蓋透過watched資料夾機制使用輸出服務將資料與範本合併的使用案例。

Output服務是OSGi服務，屬於AEM Document Services的一部分。 輸出服務支援各種輸出格式和AEM Forms Designer的輸出設計功能。 輸出服務可以轉換XFA範本和XML資料，以產生多種格式的列印檔案。

若要進一步瞭解輸出服務， [請按這裡](https://helpx.adobe.com/aem-forms/6/output-service.html).
若要在系統上設定watched資料夾，請遵循下列步驟：
* [下載並解壓縮zip檔案的內容](assets/outputservicewatchedfolderkt.zip).此zip檔案包含建立Watched資料夾的套件，以及使用Watched資料夾機制測試輸出服務的範例檔案
   * Windows系統

      * 使用封裝管理員將outputservicewatchedfolder.zip匯入AEM
      * 這會在您的C磁碟機上建立名為outputservicewatchedfolder的watched資料夾。
   * 非Windows系統
      * [開啟watched資料夾的組態設定](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * 設定輸出服務節點的資料夾路徑屬性，以指向適當的位置
      * 儲存您的變更
      * 上述位置是您的Watched資料夾。

將SamplePdfFile和SampleXdpFile資料夾拖放到watched資料夾的輸入資料夾中。 成功處理檔案時，結果會放置在監視資料夾的結果資料夾下。


>[!NOTE]
>
>如果與watched資料夾關聯的指令碼需要多個檔案，您需要建立一個資料夾，並將所有需要的檔案放在資料夾中，並將資料夾放入您的watched資料夾的輸入資料夾中。
>
>如果與watched資料夾關聯的指令碼只需要一個輸入檔案，您可以將檔案直接拖放到watched資料夾的輸入資料夾中
