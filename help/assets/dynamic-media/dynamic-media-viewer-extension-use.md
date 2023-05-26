---
title: 在Adobe Analytics和Adobe Launch中使用Dynamic Media查看器
description: 用于 Adobe Launch 的 Dynamic Media 查看器扩展以及 Dynamic Media 查看器 5.13 版允许 Dynamic Media 客户、Adobe Analytics 客户和 Adobe Launch 客户在其 Adobe Launch 配置中使用特定于 Dynamic Media 查看器的事件和数据。
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 22%

---

# 在Adobe Analytics和Adobe Launch中使用Dynamic Media查看器{#using-dynamic-media-viewers-adobe-analytics-launch}

对于具有Dynamic Media和Adobe Analytics的客户，您现在可以使用Dynamic Media Viewer扩展跟踪您的网站上Dynamic Media Viewer的使用情况。

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> 在Dynamic Media Scene7模式下运行Adobe Experience Manager以实现此功能。 您还需要 [将Adobe Experience Platform Launch与您的AEM实例集成](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html).

通过引入Dynamic Media Viewer扩展，Adobe Experience Manager现在为通过Dynamic Media查看器(5.13)交付的资源提供高级分析支持，从而当在Sites页面上使用Dynamic Media Viewer时，提供对事件跟踪的更细粒度控制。

如果您已经拥有AEM Assets和Sites，则可以将Launch资产与AEM创作实例集成。 将Launch集成与您的网站关联后，您可以向页面添加Dynamic Media组件，并为查看器启用事件跟踪。

对于仅AEM Assets客户或Dynamic Media Classic客户，用户可以获取查看器的嵌入代码并将其添加到页面中。 然后，可以手动将Launch脚本库添加到页面以进行查看器事件跟踪。

下表列出了Dynamic Media Viewer事件及其支持的参数：

<table>
   <tbody>
      <tr>
         <td>查看器事件名称</td>
         <td>参数引用</td>
      </tr>
      <tr>
         <td> 公共 </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> 横幅 <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> 项目 </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> 加载 </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> 元数据 </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> 里程碑 </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> 页面 </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> 暂停 </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> 播放 </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> 旋转 </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> 停止 </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> 交换 </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> 色板 </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> 缩放 </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## 其他资源{#additional-resources}

* [将Adobe Experience Manager与Adobe Launch集成](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [在Dynamic Media Scene7模式下运行Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=zh-Hans)
* [将 Dynamic Media 查看器与 Adobe Analytics 和 Adobe Launch 集成](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
