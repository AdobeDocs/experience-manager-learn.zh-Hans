---
title: AEM Assets水印
description: AEM作为Cloud Service的水印功能，允许使用任何PNG图像对自定义图像演绎版进行水印。
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# 水印

AEM作为Cloud Service的水印功能，允许使用任何PNG图像对自定义图像演绎版进行水印。

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi配置

以下OSGi配置存根可以更新并添加到AEM项目的 `ui.config` 子项目中。

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
