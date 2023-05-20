---
title: AdobeAsset Link和AEM
description: 設計師和創意使用者可在他們最愛的Adobe Creative Cloud案頭應用程式中使用Adobe Experience Manager資產。 適用於Adobe Creative Cloud for enterprise的AdobeAsset Link擴充功能加強在Adobe XD、Photoshop、InDesign和Illustrator等Creative Cloud工具中搜尋和瀏覽、排序、預覽、上傳資產、取出、修改、存回和檢視AEM資產的中繼資料的功能。
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 1%

---


# Adobe資產連結3.0

設計師和創意使用者可在他們最愛的Adobe Creative Cloud案頭應用程式中使用Adobe Experience Manager資產。

Adobe Creative Cloud for enterprise的Adobe Asset Link擴充功能加強在Creative Cloud應用程式中搜尋和瀏覽、排序、預覽、上傳資產、取出、修改、存回和檢視AEM資產的中繼資料的功能。

## AdobeAsset Link功能

+ Adobe Asset Link可與AEM Assets和Assets Essentials整合。
+ Adobe Asset Link會自動設定與雲端型AEM環境(AEM Assetsas a Cloud Service和Assets Essentials)的連線
+ Adobe Asset Link是適用於Adobe Creative Cloud應用程式的擴充功能：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用其AdobeEnterprise ID或Federated ID自動驗證AEM
+ 在AEM中瀏覽和搜尋數位資產
+ 使用面板存取AEM中資產的檔案詳細資訊：
   + 缩略图
   + 基本中繼資料
   + 版本
+ 將資產放置、下載或拖放至其版面配置中
+ 從AEM出庫資產，並在其Creative Cloud資產帳戶中處理資產(WIP)，以修改資產
+ 完成修改後，將資產簽回AEM，新版本會反映在AEM中
+ 從AdobeAsset Link應用程式內面板搜尋AEM中的資產
+ 直接從Asset Link面板瀏覽AEM Assets集合和智慧型集合
+ 直接從面板將新建立的資產新增到AEM
+ 將資產直接拖放到InDesign框架中

## 將資產置於InDesign中

Adobe Asset Link提供Adobe Asset Link與AEM之間的InDesign直接連結支援。 透過InDesign直接連結支援，您可以將&#x200B;__置入已連結__ 或 __置入副本__)或透過InDesign Asset Link面板，將數位資產從AEM拖放到Adobe中。 此外，也推出*For Placement Only+ (FPO)轉譯。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>僅使用您的Adobe Creative CloudEnterprise ID或Federated ID。 請確定 [設定AEM以進行Adobe資產連結](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

您可以使用下列其中一個選項，將資產放置到InDesign版面配置中：

+ **置入副本**  — 內嵌資產（使用「置入副本」選項）會在將二進位檔案下載至本機系統後，將原始資產的副本置入InDesign版面中。 Adobe資產連結不會維持內嵌副本與原始資產之間的任何連結。 如果原始資產在AEM中經過修改，您必須從InDesign檔案中刪除內嵌資產，然後從AEM中重新內嵌資產。

+ **置入已連結**  — 使用InDesign檔案時，除了直接內嵌資產外（使用快顯選單中的「置入副本」選項），您還可以選擇參照AEM中的資產。 引用資產可讓您與其他使用者合作，並在AEM中併入對原始資產所做的任何更新。 若要從AEM參照資產，請使用快顯選單中的「置入已連結」選項。

### 僅供置入的影像

當使用InDesign Asset Link從AEM將大型資產檔案放入Adobe檔案時，創意使用者在起始放置操作後需要等候幾秒鐘。 這會影響整體使用者體驗。 透過Adobe資產連結，您可以暫時從AEM置入原始資產的低解析度影像，藉此減少置入影像所需的時間。 同時提升整體使用者體驗和生產力。 低解析度的影像會暫時放置，當列印或發佈需要最終輸出時，您需要以原始影像取代FPO轉譯。 如果您想要以個別的原始影像取代多個FPO影像，請導覽至 **_「視窗>連結」_** 面板，然後下載原始資產。 下載原始影像後，選擇以原始影像取代所有FPO。

FPO轉譯是原始資產的輕量級替代專案。 它們具有相同的外觀比例，但與原始影像相比，大小較小。 目前，InDesign僅支援匯入下列影像型別的FPO轉譯：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

如果FPO轉譯無法用於AEM中的特定資產，則會改為參考原始高解析度資產。 對於FPO影像，狀態FPO會顯示在「InDesign連結」面板中。

## 使用AEM AssetsAdobeAsset Link驗證

Adobe Asset Link驗證如何在Adobe Identity Management Services (IMS)和Adobe Experience Manager Author的環境中運作。

![AdobeAsset Link架構](assets/adobe-asset-link-article-understand.png)

1. AdobeAsset Link擴充功能會透過Adobe Creative Cloud案頭應用程式提出授權請求，以Adobe身分管理服務(IMS)，並在成功時接收持有人權杖。
1. Adobe Asset Link擴充功能會透過HTTP(S)連線至AEM Author，包括取得的持有人權杖 **步驟1**，使用擴充功能設定JSON中提供的配置(HTTP/HTTPS)、主機和連線埠。
1. AEM持有者驗證處理常式會從要求中擷取持有者權杖，並使用Adobe IMS進行驗證。
1. Adobe IMS驗證持有人權杖後，就會在AEM中建立使用者（如果尚未存在），並從Adobe IMS同步設定檔和群組/成員資格資料。 AEM使用者會獲得標準AEM登入權杖，這會在HTTP(S)回應中以Cookie形式傳回AdobeAsset Link擴充功能。
1. 後續互動(即 瀏覽、搜尋、存回/取出資產等) 使用AdobeAsset Link擴充功能時，會對AEM作者產生HTTP(S)請求，且會使用標準AEM登入權杖驗證處理常式來驗證AEM。

>[!NOTE]
>
>登入權杖到期時， **步驟1-5** 將自動叫用，使用持有人權杖驗證Adobe資產連結擴充功能，並重新發出新的有效登入權杖。

## 其他資源

+ [AdobeAsset Link網站](https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html)
