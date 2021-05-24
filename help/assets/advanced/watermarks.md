---
title: AEM Assets中的水印
description: AEM as aCloud Service的水印功能允许使用任何PNG图像对自定义图像演绎版加水印。
feature: asset compute微服务
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: 内容管理
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 3%

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
