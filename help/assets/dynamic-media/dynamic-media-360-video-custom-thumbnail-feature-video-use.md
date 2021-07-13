---
title: 在AEM Assets中使用Dynamic Media 360视频和自定义视频缩略图
description: AEM 6.5中的Dynamic Media查看器增强功能包括添加了对360视频渲染、360媒体查看器（video360Social和video360VR）以及选择自定义视频缩略图的支持。
sub-product: dynamic-media
feature: 视频配置文件
version: 6.3, 6.4, 6.5
topic: 内容管理
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 5%

---


# 在AEM Assets中使用Dynamic Media 360视频和自定义视频缩略图

AEM 6.5中的Dynamic Media查看器增强功能包括添加了对360视频渲染、360媒体查看器（video360Social和video360VR）以及选择自定义视频缩略图的支持。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>视频假定您的AEM实例在Dynamic Media S7模式下运行。  [有关在Dynamic Media中设置AEM的说明，请参阅此处](https://helpx.adobe.com/cn/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。在您上传视频时，如果视频的长宽比为2:1，则默认情况下，Dynamic Media会将视频处理为360视频。 即，宽高比为2:1。

>[!NOTE]
>
>Dynamic Media 360媒体组件仅支持360个视频。

## Dynamic Media 360视频

360度视频，也称为球形视频，是录像，其中同时记录每个方向的视图，使用全方位摄像机拍摄，或摄像机集合。 在平面显示器上播放期间，用户可以控制观看方向，而在移动设备上播放通常使用内置的陀螺仪控制。  它让您可以超越单张照片的限制。 营销人员可借助360个视频为用户提供引人入胜的体验。  我们开始吧。 在公司的DMS7配置中，可以通过在/conf/global/settings/cloudconfigs/dmscene7/jcr:content处指定双属性s7PanoramicAR来修改全景图像宽高比标准。

## Dynamic Media 360视频

Dynamic Media视频现在支持为视频选择自定义缩略图。 用户可以从AEM Assets中选择现有资产，也可以选择视频帧作为缩略图。

## Dynamic 360 Media查看器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social查看器**</td>
      <td>**Video360VR查看器**</td>
   </tr>
   <tr>
      <td>Dynamic Media运行模式</td>
      <td>仅Dynamic Media Scene7模式</td>
      <td>Dynamic Media Scene7模式（仅限a0/&gt;）
         <br><br>
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
            <li>轻扫（触摸设备）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>鼠标和拖动选项处于禁用状态</li>
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
