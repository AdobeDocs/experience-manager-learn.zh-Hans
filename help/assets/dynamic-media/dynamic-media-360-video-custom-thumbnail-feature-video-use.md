---
title: 透過AEM Assets使用Dynamic Media 360影片和自訂影片縮圖
description: AEM 6.5中的Dynamic Media Viewer增強功能包括新增支援360個視訊轉譯、360個媒體檢視器（video360Social和video360VR），以及選取自訂視訊縮圖的能力。
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 4%

---

# 透過AEM Assets使用Dynamic Media 360影片和自訂影片縮圖

AEM 6.5中的Dynamic Media Viewer增強功能包括新增支援360個視訊轉譯、360個媒體檢視器（video360Social和video360VR），以及選取自訂視訊縮圖的能力。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>影片假設您的AEM執行個體在Dynamic Media S7模式下執行。  [有關使用Dynamic Media設定AEM的指示，請參閱此處](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). 上傳視訊時，預設情況下，如果長寬比為2:1，Dynamic Media會將影片處理為360視訊。 即寬高比是2:1。

>[!NOTE]
>
>Dynamic Media 360媒體元件僅支援360部影片。

## Dynamic Media 360影片

360度影片（也稱為球形影片）是同時錄製每個方向檢視的影片，使用全方位相機或相機系列拍攝。 在平面顯示器上播放期間，使用者可控制檢視方向，行動裝置上的播放通常利用內建的陀螺儀控制。  它可讓您超越單一攝影的限制。 行銷人員可藉助360部影片為使用者提供引人入勝的體驗。  讓我們開始吧。 您可以在公司的DMS7設定中修改全景影像外觀比例標準，方法是在/conf/global/settings/cloudconfigs/dmscene7/jcr：content中指定雙重屬性s7PanoramicAR。

## Dynamic Media 360影片

Dynamic Media影片現在支援為您的影片選取自訂縮圖的功能。 使用者可以從AEM Assets中選取現有資產，或選取視訊影格作為縮圖。

## Dynamic 360媒體檢視器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social檢視器**</td>
      <td>**Video360VR檢視器**</td>
   </tr>
   <tr>
      <td>Dynamic Media執行模式</td>
      <td>僅限Dynamic Media Scene7模式</td>
      <td>僅限Dynamic Media Scene7模式<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>用例</td>
      <td>
         <p>針對不支援陀螺儀的網站和裝置</p>
         <p> </p>
      </td>
      <td>
         <p>為支援陀螺儀的裝置提供虛擬現實體驗 </p>
      </td>
   </tr>
   <tr>
      <td>音訊 — 立體聲模式</td>
      <td>否</td>
      <td>是</td>
   </tr>
   <tr>
      <td>視訊播放</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>檢視點導覽</td>
      <td>
         <ul>
            <li>滑鼠拖曳（在案頭系統上）</li>
            <li>滑動（觸控裝置）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>滑鼠和拖曳選項已停用</li>
            <li>使用內建陀螺儀</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5播放器</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>社交分享選項</td>
      <td>是</td>
      <td>否</td>
   </tr>
</tbody>
</table>

## 其他资源{#additional-resources}

[在Scene7模式下設定Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
