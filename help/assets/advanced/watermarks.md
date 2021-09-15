---
title: AEM Assets中的水印
description: AEM as aCloud Service的水印功能允许使用任何PNG图像对自定义图像演绎版加水印。
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---

# 水印

AEM as aCloud Service的水印功能允许使用任何PNG图像对自定义图像演绎版加水印。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi配置

以下OSGi配置存根可以更新并添加到AEM项目的`ui.config`子项目中。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
