---
title: Dynamic Media Classic主要工作流程和預覽資產
description: 瞭解Dynamic Media Classic中的主要工作流程，其中包括三個步驟 — 建立（和上傳）、製作（和發佈）和傳送。 然後瞭解如何在Dynamic Media Classic中預覽資產。
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2703'
ht-degree: 1%

---

# Dynamic Media Classic主要工作流程和預覽資產 {#main-workflow}

Dynamic Media支援「建立」（和「上傳」）、「作者」（和「發佈」）和「傳送」工作流程。 首先，上傳資產，然後使用這些資產進行動作（例如建立影像集），最後再發佈以使其上線。 對於某些工作流程，「建置」步驟是選用的。 例如，如果您的目標是隻執行動態調整大小和縮放影像，或轉換和發佈視訊以進行串流，則沒有必要的建置步驟。

![图像](assets/main-workflow/create-author-deliver.jpg)

Dynamic Media Classic解決方案中的工作流程包含三個主要步驟：

1. 建立（和上傳）來源內容
2. 製作（和發佈）資產
3. 傳遞資產

## 步驟1：建立（和上傳）

這是工作流程的開始。 在此步驟中，您會收集或建立適合您使用之工作流程的來源內容，並將其上傳至Dynamic Media Classic。 此系統支援影像、視訊和字型的多種檔案型別，也支援PDF、Adobe Illustrator和Adobe InDesign的檔案型別。

檢視完整清單 [支援的檔案型別](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

您可以透過數種不同的方式上傳來源內容：

- 直接從您的案頭或本機網路。 [瞭解如何](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- 從Dynamic Media Classic FTP伺服器。 [瞭解如何](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

預設模式為「從案頭」，您可在其中瀏覽本機網路上的檔案並開始上傳。

![图像](assets/main-workflow/upload.jpg)

>[!TIP]
>
>請勿手動新增資料夾。 請改為從FTP執行上傳，並使用 **包含子資料夾** 在Dynamic Media Classic中重新建立資料夾結構的選項。

根據預設，系統會啟用兩個最重要的上傳選項： **標籤為發佈**，我們先前已討論過這個問題，並且 **覆寫**. 覆寫表示如果上傳的檔案與系統中已存在的檔案同名，則新檔案會取代現有版本。 如果取消核取此選項，可能無法上傳檔案。

### 上傳影像時覆寫選項

您可為整個公司設定四種不同的「覆寫影像」選項，且這些選項經常會被誤解。 簡而言之，您設定規則後，或是希望更頻繁地覆寫具有相同名稱的資產，或是希望較不頻繁地發生覆寫（在這種情況下，新影像將以&quot;-1&quot;或&quot;-2&quot;副檔名重新命名）。

- **在目前檔案夾中以相同的基本影像名稱/副檔名覆寫**.
此選項是最嚴格的取代規則。 您需要將取代影像上傳到與原始影像相同的資料夾，而且取代影像的副檔名必須與原始影像相同。 如果不符合這些要求，則會建立副本。

- **目前檔案夾內若有基本資產名稱相同者（無論副檔名為何），將予以覆寫**.
需要您將取代影像上傳到與原始影像相同的資料夾，但副檔名可能與原始影像不同。 例如，chair.tif會取代chair.jpg。

- **在任意文件夹内，使用相同的基本资源名称和扩展名进行覆盖**.
取代影像的副檔名必須與原始影像相同（例如，chair.jpg必須取代chair.jpg，而不是chair.tif ）。 不過，您可以將取代影像上傳至與原始影像不同的資料夾。 更新後的影像位於新資料夾中；檔案無法再在其原始位置找到。

- **在任意文件夹内，使用相同的基本资源名称（不区分扩展名）进行覆盖**.
此選項是最具包容性的取代規則。 您可以將取代影像上傳至與原始檔案不同的資料夾、上傳副檔名不同的檔案，以及取代原始檔案。 如果原始檔案位於不同的資料夾中，取代影像會位於上傳該影像的新資料夾中。

進一步瞭解 [覆寫影像選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

雖然並非必要，但使用上述兩種方法其中之一上傳時，您可以指定該特定上傳的工作選項 — 例如，排程週期性上傳、在上傳時設定裁切選項和許多其他選項。 這些工具對於某些工作流程來說可能很有價值，因此如果可供您使用，也值得考慮。

進一步瞭解 [工作選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

上傳是任何工作流程中的第一個必要步驟，因為Dynamic Media Classic無法處理其系統中尚未包含的任何內容。 在上傳期間，系統會於幕後使用集中式Dynamic Media Classic資料庫註冊每個上傳的資產、指派ID並將其複製到儲存空間。 此外，系統會將影像檔案轉換為允許動態調整大小和縮放的格式，並將視訊檔案轉換為MP4網頁易記格式。

### 概念：上傳影像至Dynamic Media Classic時會發生下列情況

上傳任何型別的影像至Dynamic Media Classic時，都會轉換為稱為金字塔TIFF或PTIFF的主影像格式。 PTIFF類似於TIFF圖層點陣圖影像的格式，除了檔案包含相同影像的多種大小（解析度），而不是不同的圖層。

![图像](assets/main-workflow/pyramid-p-tiff.png)

轉換影像時，Dynamic Media Classic會拍攝影像完整大小的「快照」、將快照縮放一半並儲存、再次縮放一半並儲存，以此類推，直到填滿原始大小的偶數倍數為止。 例如，2000畫素的PTIFF在同一個檔案中具有1000、500、250和125畫素大小（或更小）。 PTIFF檔案是Dynamic Media Classic中稱為「主影像」的格式。

當您請求特定大小的影像時，建立PTIFF可讓Dynamic Media Classic的影像伺服器快速找到下一個較大的影像，並將其縮放。 例如，如果您上傳2000畫素的影像並請求100畫素的影像，Dynamic Media Classic會找到125畫素的版本，並將其縮放至100畫素，而非從2000縮放至100畫素。 如此一來，操作速度非常快。 此外，在縮放影像時，這可讓縮放檢視器僅要求縮放影像的拼貼，而非整個完整解析度影像。 這就是主要影像格式(PTIFF檔案)支援動態大小和縮放的方式。

同樣地，您可以將主要來源視訊上傳至Dynamic Media Classic，而在上傳時，Dynamic Media Classic可以自動調整其大小，並將其轉換為MP4 Web友好格式。

### 決定您上傳之影像的最佳大小的經驗法則

**以您所需的最大尺寸上傳影像。**

- 如果您需要縮放，請上傳最大維度中1500-2500畫素範圍的高解析度影像。 請考量您要提供多少細節、來源影像的品質，以及要顯示的產品大小。 例如，上傳小圓環的1000畫素影像，但上傳整個房間場景的3000畫素影像。
- 如果您不需要縮放，請以顯示的確切大小上傳它。 例如，如果您要將標誌或啟動畫面/橫幅影像放在頁面上，請完全按照1:1的大小上傳影像，並完全按照該大小呼叫。

**上傳至Dynamic Media Classic之前，請勿上傳或爆裂您的影像。** 例如，請勿將較小的影像上取樣，使其成為2000畫素的影像。 看起來不太好。 上傳前，請使影像儘可能完美。

**縮放沒有大小下限，但檢視器預設不會縮放超過100%。** 如果您的影像太小，則完全不會縮放，或僅會放大很小的一部分，以防止影像看起來不好。

**雖然影像大小沒有下限，但我們不建議上傳巨量影像。** 大型影像可以視為4000個以上的畫素。 上傳此大小的影像可能會顯示影像中的灰塵或毛髮等潛在瑕疵。 這類影像會佔用Dynamic Media Classic伺服器上的更多空間，讓您超出合約的儲存空間限制。

進一步瞭解 [正在上傳檔案](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## 步驟2：製作（和發佈）

建立及上傳內容後，您將透過執行一個或多個子工作流程，從上傳的資產中創作新的多媒體資產。 這包括所有不同型別的集合集合 — 影像、色票、迴轉和混合媒體集，以及範本。 其中也包含影片。 我們稍後會更詳細地說明每種型別的影像收集集和視訊豐富媒體。 不過，在大部分情況下，您一開始會選取一或多個資產（或未選取任何資產），然後選擇要建立的資產型別。 例如，您可以選取主要影像以及該影像的幾個檢視，然後選擇建置影像集，也就是相同產品的替代檢視集合。

>[!IMPORTANT]
>
>請確定您的所有資產都已標示為發佈。 雖然所有資產預設會在上傳時自動標示為發佈，但您上傳內容中任何新撰寫的資產也需要標示為發佈。

建立新資產後，您將執行發佈工作。 您可以手動執行此動作，或排程自動執行的發佈工作。 發佈會將所有內容從私人的Dynamic Media Classic sphere複製到公共的、發佈方程式的伺服器sphere。 Dynamic Media發佈工作的產品是每個已發佈資產的唯一URL。

您發佈到的伺服器取決於內容型別和工作流程。 例如，所有影像會傳送至「影像伺服器」，而串流視訊會傳送至「FMS伺服器」。 為方便起見，我們將以「發佈」單一事件形式發佈至單一伺服器。

發佈會發佈所有標籤為發佈的內容，而不僅僅是您的內容。 通常由單一管理員代表所有人發佈，而非執行發佈的個別使用者。 管理員可視需要發佈，或設定重複每日、每週甚至每10分鐘自動發佈的工作。 依您的業務適用的排程發佈。

>[!TIP]
>
>自動化您的發佈工作，並排程完整發佈，以在每天凌晨12:00或傍晚的任何時間執行。

### 概念：瞭解Dynamic Media Classic URL

Dynamic Media Classic工作流程的最終產品是指向資產的URL （無論是影像集或最適化視訊集）。 這些URL非常可預測，且遵循相同的模式。 如果是影像，則每個影像都是從PTIFF主影像產生的。

以下是包含幾個範例的影像URL語法：

![图像](assets/main-workflow/dmc-url.jpg)

在URL中，問號左側的所有內容都是特定影像的虛擬路徑。 問號右側的所有內容都是「影像伺服器」修飾元，也就是處理影像的說明。 如果有多個修飾元，它們會以&amp;符號分隔。

在第一個範例中，影像「Backpack_A」的虛擬路徑為 `http://sample.scene7.com/is/image/s7train/BackpackA`. 「影像伺服器」修飾元會將影像大小調整為250畫素的寬度（從wid=250），並使用Lanczos內插演演算法重新取樣影像，隨著調整大小而銳利化（從resMode=sharp2）。

第二個範例會將所謂的「影像預設集」套用至相同的Backpack_A影像，如$所示！_template300$。 運算式兩側的$符號表示影像預設集（一組封裝的影像修飾元）正在套用至影像。

瞭解Dynamic Media Classic URL的組合方式後，您就能瞭解如何以程式設計方式變更URL，以及如何更深入地將其整合至您的網站和後端系統。

### 概念：瞭解快取延遲

新上傳和發佈的資產會立即顯示，而更新的資產可能會受到10小時快取延遲的約束。 依預設，所有已發佈資產在到期前至少有10小時。 我們稱之為「最小值」，因為每次檢視影像時，它都會啟動一個直到10小時後才到期的時鐘，而此期間沒有任何人檢視過該影像。 此10小時期間是資產的「存留時間」。 該資產的快取一旦過期，即可傳送更新版本。

除非發生錯誤，且影像/資產的名稱與先前發佈的版本相同，但影像出現問題，否則這通常不是問題。 例如，您意外上傳了低解析度版本，或您的藝術指導未核准影像。 在此情況下，您會想要重新叫用原始影像，並使用相同的資產ID以新版本取代。

瞭解如何 [手動清除需要更新之URL的快取](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=zh-Hans).

>[!TIP]
>
>為避免快取延遲問題，請一律提前進行 — 晚上、一天、兩週等。 及時建置，讓內部各方能夠進行QA/接受，在發佈給公眾之前證明您的工作。 即使您是在前一個晚上工作，也允許您在當晚進行變更並重新發佈。 到早上時，10小時已經過去，快取會以正確的影像更新。

- 進一步瞭解 [建立發佈工作](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- 進一步瞭解 [發佈](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html).

## 步驟3：傳遞

請記住，Dynamic Media Classic工作流程的最終產品是指向資產的URL。 URL可能會指向個別影像、影像集、迴轉集或某些其他影像集集合或影片。 您需要取用該URL並對其執行操作，例如編輯您的HTML，以便 `<IMG>` 標籤會指向Dynamic Media Classic影像，而非指向來自您目前網站的影像。

在「傳送」步驟中，您必須將這些URL整合至您的網站、行動應用程式、電子郵件促銷活動，或您要顯示資產的任何其他數位接觸點。

將影像的Dynamic Media Classic URL整合至網站的範例：

![图像](assets/main-workflow/example-url-1.png)

紅色的URL是唯一特定於Dynamic Media Classic的元素。

您的IT團隊或整合合作夥伴可以率先撰寫及變更程式碼，將Dynamic Media Classic URL整合至您的網站。 Adobe有一個諮詢團隊，可提供技術、創意或一般指導，以協助您完成這項工作。

對於更複雜的解決方案（例如縮放檢視器或結合縮放與替代檢視的檢視器），URL通常會指向Dynamic Media Classic託管的檢視器，且該URL內的也是資產ID的參考。

將在新快顯視窗的檢視器中開啟影像集的連結（紅色）範例：

![图像](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>您需要將Dynamic Media Classic URL整合至您的網站、行動應用程式、電子郵件和其他數位接觸點 — Dynamic Media Classic不適合您！

## 預覽資產

您可能想要預覽已上傳或正在建立或編輯的資產，以確保在客戶檢視資產時，資產能依您想要的方式顯示。 您可以按一下任何「 」來存取「預覽」視窗 **預覽** 按鈕（位於資產的縮圖上） **瀏覽/建置面板**，或前往 **檔案>預覽**. 在瀏覽器視窗中，它將預覽面板中目前的任何資產，無論是影像、影片或影像集之類的建置資產。

### 動態大小預覽（影像預設集）

您可以使用預覽多種大小的影像 **大小** 預覽。 這會載入可用影像預設集清單。 稍後我們將討論影像預設集，但將其視為以具名大小載入您的影像，並搭配特定銳利化和影像品質的「配方」。

### 縮放預覽

您也可以使用 **縮放** 可在許多預先建立的縮放預設集之一中預覽影像的選項，這些預設集是以不同的包含縮放檢視器為基礎。

進一步瞭解 [預覽資產](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html).
