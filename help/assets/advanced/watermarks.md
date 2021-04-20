---
title: AEM Assets中的水印
description: AEM作为Cloud Service的水印功能，允许使用任何PNG图像对自定义图像演绎版进行水印。
feature: Asset Compute Microservices
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

---


# 水印

AEM作为Cloud Service的水印功能，允许使用任何PNG图像对自定义图像演绎版进行水印。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi配置

可以更新以下OSGi配置存根并将其添加到AEM项目的`ui.config`子项目中。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
