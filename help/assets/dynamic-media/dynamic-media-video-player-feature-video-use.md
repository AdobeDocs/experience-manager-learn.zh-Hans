---
title: 在AEM Dynamic Media中使用视频播放器
description: AEM Dynamic Media视频播放器过去依赖Flash运行时在桌面客户端和浏览器上支持自适应视频流播放，现在则对基于flash的内容流更具攻击性。 随着HLS(Apple的HTTP实时流视频交付协议)的推出，现在无需依赖Flash即可对内容进行流式处理。
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 568
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# 在AEM Dynamic Media中使用视频播放器{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media视频播放器过去依赖Flash运行时在桌面客户端和浏览器上支持自适应视频流播放，现在则对基于flash的内容流更具攻击性。 随着HLS(Apple的HTTP实时流视频交付协议)的推出，现在无需依赖Flash即可对内容进行流式处理。

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## 快速了解非Flash视频播放器 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

HLS浏览器支持如下，对于不支持的浏览器，我们回退到渐进式视频交付

>[!NOTE]
>
> 从2022年3月15日起，Dynamic Media混合模式不支持Internet Explorer 11上的视频流。 请升级到6.5.12或更高版本以回退到IE 11上的渐进式播放。

<table> 
 <thead> 
  <tr> 
   <th> <p>设备</p> </th>
   <th> <p>浏览器</p> </th>
   <th > <p>视频播放模式</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Internet Explorer 9和10</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr>
   <td> <p>桌面</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media - Scene 7模式：HLS视频流</p> 
        <p>Dynamic Media — 混合模式：渐进式下载</p>
   </td>
  </tr>
  <tr>
   <td> <p>桌面</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Firefox 45或更高版本</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome(Android 6或更早版本)</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome (Android 7或更高版本)</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Android（默认浏览器）</p> </td>
   <td> <p>渐进式下载</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
  <tr> 
   <td> <p>移动设备</p> </td>
   <td> <p>黑莓</p> </td>
   <td> <p>HLS视频流</p> </td>
  </tr>
 </tbody>
</table>
