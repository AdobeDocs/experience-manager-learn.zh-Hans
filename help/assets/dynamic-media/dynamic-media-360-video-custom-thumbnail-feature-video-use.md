---
title: 在AEM Assets中使用Dynamic Media 360视频和自定义视频缩略图
description: AEM 6.5中的Dynamic Media Viewer增强功能包括额外支持360个视频渲染、360个媒体查看器（video360Social和video360VR），以及选择自定义视频缩略图的功能。
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 656
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 3%

---

# 在AEM Assets中使用Dynamic Media 360视频和自定义视频缩略图

AEM 6.5中的Dynamic Media Viewer增强功能包括额外支持360个视频渲染、360个媒体查看器（video360Social和video360VR），以及选择自定义视频缩略图的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>视频假定您的AEM实例在Dynamic Media S7模式下运行。  [有关使用Dynamic Media设置AEM的说明，请访问此处](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). 上传视频时，默认情况下，如果长宽比为2:1，Dynamic Media会将素材处理为360视频。 即，宽高比为2:1。

>[!NOTE]
>
>Dynamic Media 360媒体组件仅支持360个视频。

## Dynamic Media 360视频

360度视频，也称为球形视频，是一种同时录制每个方向的视图的视频记录，使用全方位相机或相机集合拍摄。 在平面显示器上播放期间，用户可控制观看方向，移动设备上的播放通常利用内置的陀螺仪控制。  它可让您超越单个摄影的极限。 营销人员可以在360个视频的帮助下为用户提供引人入胜的体验。  我们开始吧。 通过指定位于/conf/global/settings/cloudconfigs/dmscene7/jcr：content的双属性s7PanoramicAR，可以修改公司的DMS7配置中的全景图像长宽比标准。

## Dynamic Media 360视频

Dynamic Media视频现在支持为您的视频选择自定义缩略图。 用户可以从AEM Assets中选择现有资源，也可以选择视频帧作为缩略图。

## Dynamic 360媒体查看器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social查看器**</td>
      <td>**Video360VR查看器**</td>
   </tr>
   <tr>
      <td>Dynamic Media运行模式</td>
      <td>仅限Dynamic Media Scene7模式</td>
      <td>仅限Dynamic Media Scene7模式<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>用例</td>
      <td>
         <p>对于不支持陀螺仪的网站和设备</p>
         <p> </p>
      </td>
      <td>
         <p>为支持陀螺仪的设备提供虚拟现实体验 </p>
      </td>
   </tr>
   <tr>
      <td>音频 — 立体声模式</td>
      <td>否</td>
      <td>是</td>
   </tr>
   <tr>
      <td>视频播放</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>视点导航</td>
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
      <td>HTML5播放器</td>
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
