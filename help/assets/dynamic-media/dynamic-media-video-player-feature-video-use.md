---
title: 在AEM Dynamic Media中使用視訊播放器
description: AEM Dynamic Media視訊播放器過去仰賴Flash執行階段在案頭使用者端和瀏覽器上支援最適化視訊串流，現在對flash型內容串流攻擊性更強。 隨著HLS (Apple的HTTP即時串流視訊傳送通訊協定)的推出，內容現在可以串流處理，而不需依賴Flash。
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 5%

---


# 在AEM Dynamic Media中使用視訊播放器{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media視訊播放器過去仰賴Flash執行階段在案頭使用者端和瀏覽器上支援最適化視訊串流，現在對flash型內容串流攻擊性更強。 隨著HLS (Apple的HTTP即時串流視訊傳送通訊協定)的推出，內容現在可以串流處理，而不需依賴Flash。

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## 快速瀏覽非Flash視訊播放器 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

HLS瀏覽器支援如下，對於不支援的瀏覽器，我們會退回漸進式視訊傳送

>[!NOTE]
>
> 自2022年3月15日起，Dynamic Media Hybrid不支援Internet Explorer 11上的視訊串流。 請升級至6.5.12或更新版本，以便回復到IE 11上的漸進式播放。

<table> 
 <thead> 
  <tr> 
   <th> <p>设备</p> </th>
   <th> <p>浏览器</p> </th>
   <th > <p>視訊播放模式</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Internet Explorer 9和10</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr>
   <td> <p>桌面</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media - Scene 7模式： HLS視訊串流</p> 
        <p>Dynamic Media — 混合模式：漸進式下載</p>
   </td>
  </tr>
  <tr>
   <td> <p>桌面</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Firefox 45或更新版本</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>鉻黃</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome （Android 6或更舊版本）</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome （Android 7或更新版本）</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Android （預設瀏覽器）</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>黑莓</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
 </tbody>
</table>
