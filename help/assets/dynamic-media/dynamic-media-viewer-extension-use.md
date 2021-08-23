---
title: 将Dynamic Media查看器与Adobe Analytics和AdobeLaunch结合使用
description: 用于 Adobe Launch 的 Dynamic Media 查看器扩展以及 Dynamic Media 查看器 5.13 版允许 Dynamic Media 客户、Adobe Analytics 客户和 Adobe Launch 客户在其 Adobe Launch 配置中使用特定于 Dynamic Media 查看器的事件和数据。
sub-product: Dynamic Media
feature: 资产分析
version: 6.3, 6.4, 6.5
topic: 内容管理
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 16%

---


# 将Dynamic Media查看器与Adobe Analytics和AdobeLaunch结合使用{#using-dynamic-media-viewers-adobe-analytics-launch}

对于具有Dynamic Media和Adobe Analytics的客户，您现在可以使用Dynamic Media查看器扩展来跟踪Dynamic Media查看器在您网站上的使用情况。

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> 在Dynamic Media Scene7模式下运行Adobe Experience Manager，以实现此功能。 您还需要[将Adobe Experience Platform Launch与AEM实例](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)集成。

通过引入Dynamic Media查看器扩展，Adobe Experience Manager现在为Dynamic Media查看器(5.13)提供的资产提供了高级分析支持，从而在站点页面上使用Dynamic Media查看器时，可以更加精细地控制事件跟踪。

如果您已经拥有AEM Assets和Sites，则可以将Launch资产与AEM创作实例相集成。 将launch集成与您的网站关联后，您可以向页面添加Dynamic Media组件，并启用查看器事件跟踪。

对于仅限AEM Assets的客户或Dynamic Media Classic客户，用户可以获取查看器的嵌入代码并将其添加到页面中。 然后，可以手动将Launch脚本库添加到页面，以便进行查看器事件跟踪。

下表列出了Dynamic Media查看器事件及其支持的参数：

<table>
   <tbody>
      <tr>
         <td>查看器事件名称</td>
         <td>参数引用</td>
      </tr>
      <tr>
         <td> 通用 </td>
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
         <td> %event.detail.dm.SPIN.frameumber% </td>
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
         <td> 塔格 </td>
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

* [将Adobe Experience Manager与Launch集成Adobe](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [在Dynamic Media Scene7模式下运行Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [将 Dynamic Media 查看器与 Adobe Analytics 和 Adobe Launch 集成](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
