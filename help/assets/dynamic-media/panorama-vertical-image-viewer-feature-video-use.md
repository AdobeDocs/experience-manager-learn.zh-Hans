---
title: 搭配AEM Assets Dynamic Media使用全景和垂直影像檢視器
description: AEM 6.4中的Dynamic Media Viewer增強功能包括新增全景影像檢視器、全景虛擬現實影像檢視器和垂直影像檢視器。 全景檢視器可讓您輕鬆享受引人入勝的沈浸式體驗，無需任何自訂開發，即可呈現房間、屬性、位置或風景。
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 2%

---

# 搭配AEM Assets Dynamic Media使用全景和垂直影像檢視器{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4中的Dynamic Media Viewer增強功能包括新增全景影像檢視器、全景虛擬現實影像檢視器和垂直影像檢視器。 全景檢視器可讓您輕鬆享受引人入勝的沈浸式體驗，無需任何自訂開發，即可呈現房間、屬性、位置或風景。

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>影片假設您的AEM執行個體在Dynamic Media S7模式下執行。 [有關使用Dynamic Media設定AEM的指示，請參閱此處。](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 全景和全景VR檢視器

系統會根據影像的外觀比例或關鍵字將影像視為全景。 依照預設，外觀比例為2的影像會被視為全景影像。 如果滿足上述條件，全景影像檢視器預設集將可用於影像預覽。 您可以在公司的DMS7設定中修改全景影像外觀比例標準，方法是在/conf/global/settings/cloudconfigs/dmscene7/jcr：content中指定雙重屬性s7PanoramicAR。 關鍵字會儲存在資產中繼資料節點的dc：keyword屬性中。 如果關鍵字包含下列任何組合：

* 等矩形，
* 球形+全景，
* 球形+全景，

無論其外觀比例為何，都會被視為全景影像資產。

## 垂直影像檢視器

對於水準色票，視消費者的桌上型電腦熒幕大小而定，有時直到使用者向下捲動頁面後，才會顯示色票。 透過使用垂直影像檢視器並放置垂直色票，可確保無論熒幕大小為何，都能看見色票。 它也會將主要影像大小最大化。 使用水準色票時，必須在頁面上保留空間，以確保這些色票有很高的可見度，並降低主要影像的大小。 使用垂直版面時，您不必擔心配置此空間，因此可以將主要影像大小最大化。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>全景和VR檢視器</td>
   <td>垂直影像檢視器</td>
  </tr>
  <tr>
   <td>Dynamic Media執行模式</td>
   <td>僅限Dynamic Media Scene7模式</td>
   <td>DMS7和Dynamic Media</td>
  </tr>
  <tr>
   <td>用例</td>
   <td><p>全景檢視器和虛擬現實檢視器為使用者提供更吸引人的體驗。 使用者甚至可以在訂房前就離開飯店房間，或是在無須預約的情況下離開出租物業。 使用者可以簽出位置和更多可能性。 這裡的主要焦點是在消費者造訪您的網站時提供更好的體驗，最終提高您的轉換率。</p> <p> </p> </td> 
   <td><p>垂直影像檢視器有助於最大化產品影像檢視體驗，為消費者提供最佳的產品呈現，進而推動轉換並最小化回報。</p> <p> </p> </td>
  </tr>
  <tr>
   <td>可用 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[在Scene7模式下設定Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[在混合模式下設定Dynamic Media](https://helpx.adobe.com/cn/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>全景檢視器可搭配全景影像使用，而非用於一般影像。
