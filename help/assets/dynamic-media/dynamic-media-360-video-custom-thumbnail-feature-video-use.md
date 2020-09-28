---
title: 将Dynamic Media 360视频和自定义视频缩略图与AEM Assets
seo-title: 将Dynamic Media 360视频和自定义视频缩略图与AEM Assets
description: AEM 6.5中的Dynamic Media Viewer增强功能包括新增了对360视频渲染、360媒体查看器（video360Social和video360VR）的支持以及选择自定义视频缩略图的功能。
seo-description: AEM 6.5中的Dynamic Media Viewer增强功能包括新增了对360视频渲染、360媒体查看器（video360Social和video360VR）的支持以及选择自定义视频缩略图的功能。
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: 动态媒体
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# 将Dynamic Media 360视频和自定义视频缩略图与AEM Assets

AEM 6.5中的Dynamic Media Viewer增强功能包括新增了对360视频渲染、360媒体查看器（video360Social和video360VR）的支持以及选择自定义视频缩略图的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>视频假定AEM实例在Dynamic Media S7模式下运行。  [有关使用Dynamic Media设置AEM的说明，请参阅此处](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。 在上传视频时，如果视频的长宽比为2:1，则默认情况下，Dynamic Media会将该视频处理为360视频。 即，宽高比为2:1。

>[!NOTE]
>
>Dynamic Media 360媒体组件仅支持360个视频。

## Dynamic Media 360视频

360度视频，也称为球形视频，是一种视频记录，其中，同时记录每个方向的视图，使用全向摄像机拍摄，或采集摄像机。 在平面显示器上播放时，用户可以控制观看方向，而在移动设备上播放通常采用内置的陀螺仪控制。  它让您超越单一摄影的极限。 营销人员可以借助360个视频为用户提供引人入胜的体验。  我们开始吧。 在公司的DMS7配置中，可以通过在/conf/global/settings/cloudconfigs/dmscene7/jcr:content指定多次属性s7PanoramicAR来修改全景图像宽高比标准。

## Dynamic Media 360视频

Dynamic Media视频现在支持为视频选择自定义缩略图的功能。 用户可以从AEM Assets选择现有资产或选择视频帧作为缩略图。

## 动态360媒体查看器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR查看器**</td>
   </tr>
   <tr>
      <td>Dynamic Media运行模式</td>
      <td>仅Dynamic MediaScene7模式</td>
      <td>仅Dynamic MediaScene7模式<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>用例</td>
      <td>
         <p>适用于不支持陀螺仪的网站和设备</p>
         <p> </p>
      </td>
      <td>
         <p>为支持陀螺仪的设备提供虚拟现实体验 </p>
      </td>
   </tr>
   <tr>
      <td>音频——立体声模式</td>
      <td>否</td>
      <td>是</td>
   </tr>
   <tr>
      <td>视频播放</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>视图点导航</td>
      <td>
         <ul>
            <li>鼠标拖动（在桌面系统上）</li>
            <li>轻扫（触控设备）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>禁用鼠标和拖动选项</li>
            <li>使用内置陀螺仪</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5 Player</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>社交共享选项</td>
      <td>是</td>
      <td>否</td>
   </tr>
</tbody>
</table>

## 其他资源{#additional-resources}

[在Scene7模式下配置Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
