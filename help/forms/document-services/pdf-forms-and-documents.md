---
title: 瞭解不同型別的PDF forms和檔案
description: PDF實際上是一系列檔案格式，本文介紹對表單開發人員而言重要和相關的PDF型別。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

可攜式檔案格式(PDF)實際上是一系列檔案格式，本文會詳細說明與表單開發人員最相關的格式。 各種PDF型別的許多技術細節和標準都在不斷演化和變化。 其中部分格式與規格為國際標準化組織(ISO)標準，部分為Adobe所擁有的特定智慧財產。

本文說明如何建立各種型別的PDF。 它可協助您瞭解如何使用及為何要使用各種。 所有這些型別在用於檢視和使用PDF的Premier使用者端工具(Adobe Acrobat DC)中都能發揮最佳效能。

以下範例為Acrobat DC中的PDF/A檔案。

![Pdfa](assets/pdfa-file-in-acrobat.png)

範例檔案可以是 [已從此處下載](assets/pdf-file-types.zip)

## XML Forms架構PDF(XFAPDF)

Adobe使用術語XFAPDF表單，來指稱您使用AEM Forms Designer建立的互動式和動態Forms。 您使用Designer建立的Forms和檔案是以Adobe的XML Forms架構(XFA)為基礎。 在許多方面，XFAPDF檔案格式比HTML檔案更接近傳統PDF檔案。 例如，下列程式碼會向您展示XFAPDF檔案中簡單文字物件的外觀。

![文字欄位](assets/text-field.JPG)

XFA Forms以XML為基礎。 此良好的架構和彈性的格式可讓AEM Forms伺服器將您的設計工具檔案轉換為不同的格式，包括傳統的PDF、PDF/A和HTML。 您可以選取「版面配置編輯器」的「XML來源」標籤，在Designer中檢視Forms的完整XML結構。 您可以在AEM Forms Designer中建立靜態和動態XFA Forms。

## 靜態PDF

靜態XFAPDF forms版面在執行階段從不變更，但可為使用者提供互動式。 以下是靜態XFAPDF forms的一些優點：

* 靜態XFAPDF forms版面在執行階段從不變更，但可為使用者提供互動式。
* 靜態Forms支援Acrobat的評論和標籤工具。
* 靜態Forms可讓您匯入和匯出Acrobat註解。
* 靜態Forms支援字型子設定，此技術可在AEM Forms伺服器上完成。
* 靜態Forms可使用現代瀏覽器隨附的內建PDF檢視器呈現。

>[!NOTE]
>
> 您可以將XDP儲存為PDF靜態PDF表單，以使用AEM Forms Designer建立靜態Adobe



### 動態Forms

動態XFAPDF可在執行階段變更其配置，因此不支援註釋和標籤功能。 不過，動態XFAPDF確實提供下列優點：

* 動態表單支援可變更表單版面配置與分頁的使用者端指令碼。 例如，如果您將Purchase Order.xdp儲存為動態表單，它會展開並編頁，以因應無窮無盡的資料量
* 動態表單在執行階段支援表單的所有屬性，而靜態表單僅支援子集

>[!NOTE]
>
> 您可以將XDP儲存為Adobe動態XML表單，以使用AEM Forms Designer建立動態PDF

>[!NOTE]
>
> 動態表單無法使用現代瀏覽器的內建pdf檢視器呈現。

### PDF檔案(傳統PDF)

認證檔案可為PDF檔案和Forms收件者提供對其真實性和完整性的更多保證。

最受歡迎和普及的PDF格式是傳統的PDF檔案。 建立傳統PDF檔案有許多方式，包括使用Acrobat和許多協力廠商工具。 Acrobat提供下列所有方式來建立傳統PDF檔案。 如果您尚未安裝Acrobat，則電腦上可能看不到這些選項。

* 擷取案頭應用程式的列印資料流：選擇製作應用程式的「列印」命令，並選取Adobe PDF印表機圖示。 您將建立檔案的PDF檔案，而不是列印的檔案復本
* 搭配Microsoft Office應用程式使用Acrobat PDFMaker外掛程式：安裝Acrobat時，它會將Adobe PDF功能表新增至Microsoft Office應用程式，並將圖示新增至Office功能區。 您可以使用這些新增的功能，直接在Microsoft Office中建立PDF檔案
* 使用Acrobat Distiller將Postscript和封裝的Postscript (EPS)檔案轉換為PDF：Distiller通常用於列印發佈和其他需要從Postscript格式轉換為PDF格式的工作流程
* 本質上，傳統PDF與XFAPDF非常不同。 它沒有相同的XML結構，而且由於它是透過擷取檔案的列印資料流建立的，所以傳統的PDF是靜態且唯讀的檔案。

認證檔案可為PDF檔案及表單收件者提供其真實性和完整性的額外保證。

### Acroforms

Acroform是Adobe的舊版互動式表單技術，可追溯至Acrobat第3版。 Adobe提供 [Acrobat Forms API參考](assets/FormsAPIReference.pdf)日期為2003年5月，目的是提供此技術的技術細節。 Acroform是下列專案的組合：

* 定義表單靜態版面與圖形的傳統PDF。
* 使用Adobe Acrobat程式的表單工具錨定在頂端的互動式表單欄位。 這些表單工具是AEM Forms Designer中可用工具的一小部分。

### PDF/A (封存PDF)

PDF/A (封存PDF)以傳統PDF的檔案儲存優點為基礎，有許多特定細節，可強化長期封存。 傳統的PDF檔案格式為長期檔案儲存提供了許多好處。 PDF的精巧特性有助於輕鬆傳輸並節省空間，而其結構良好的特性可啟用強大的索引和搜尋功能。 傳統PDF廣泛支援中繼資料，而PDF在支援不同電腦環境方面擁有悠久的歷史。

就像PDF一樣，PDF/A是ISO標準規格。 它由包括資訊與影像管理協會(AIIM)、國家印刷裝置協會(NPES)和美國法院管理辦公室在內的一個工作組所開發。 由於PDF/A規格的目標是提供長期的封存格式，因此會省略許多PDF功能，讓檔案可自我包含。 以下是有關規格的幾個要點，這些要點可增強PDF/A檔案的長期可重複性：

* 所有內容都必須包含在檔案中，而且外部來源（例如超連結、字型或軟體程式）不能有相依性。
* 所有字型都必須內嵌，而且它們必須是擁有電子檔案無限使用授權的字型。
* 不允許JavaScript
* 不允許透明度
* 不允許加密
* 不允許音訊和視訊內容
* 必須以與裝置無關的方式定義色彩空間
* 所有中繼資料都必須遵循特定標準

### 檢視PDF/A檔案

範例檔案中的兩個檔案是從相同的Microsoft Word檔案建立的。 一個建立為傳統PDF，另一個建立為PDF/A檔案。 在Acrobat Professional中開啟這兩個檔案：

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

雖然檔案看起來相同，但PDF/A檔案會開啟，頂端有一個藍色列，表示您正在以PDF/A模式檢視此檔案。 這個藍色列是Acrobat的檔案訊息列，當您開啟特定型別的PDF檔案時，就會看到這個訊息列。

![Pdf-img](assets/pdfa-message.png)

檔案訊息列包含指示，以及可能有助於您完成任務的按鈕。 它標示有色彩，當您開啟特殊型別的PDF(例如這個PDF/A檔案)以及經過認證和數位簽署的PDF時，您會看到藍色。 當您參與PDF稽核時，PDF forms的橫條會變更為紫色，而黃色會變更為橫條。

>[!NOTE]
>
> 如果您按一下「啟用編輯」，您會將此檔案從PDF/A規範中移除。
