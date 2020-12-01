---
title: 使用AEM Dynamic Media了解颜色管理
seo-title: 使用AEM Dynamic Media了解颜色管理
description: 在此视频中，我们将探索Dynamic Media颜色管理，以及如何使用它为AEM Assets提供颜色校正预览功能。
seo-description: 在此视频中，我们将探索Dynamic Media颜色管理，以及如何使用它为AEM Assets提供颜色校正预览功能。
uuid: dc14d067-11a2-4662-acfd-f9f6f1d738ee
discoiquuid: b2b9ccc9-96b5-4bea-9995-2e6b353c469d
sub-product: 动态媒体
feature: image-profiles, video-profiles
topics: images, videos, renditions, authoring, integrations, publishing, metadata
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 13%

---


# 了解使用AEM Dynamic Media进行色彩管理{#understanding-color-management-with-aem-dynamic-media}

在此视频中，我们将探索Dynamic Media颜色管理，以及如何使用它为AEM Assets提供颜色校正预览功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[启用Dynamic ](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) Media AEM以使用此功能。

此功能作为功能包提供给AEM 6.1和6.2版本。

## 颜色管理配置节点{#xml-template-for-the-color-management-configuration-node}的XML模板

以下是“颜色管理”配置节点的XML模板。 可以将此XML模板复制到AEM开发项目中，并使用适合项目的配置进行配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### 默认Adobe颜色列表的用户档案列在{#list-of-default-adobe-color-profiles-are-listed-below}下

| 名称 | 色彩空间 | 描述 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB(1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Coated FOGRA27(ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Coated FOGRA39(ISO 12647-2:2004) |
| CoatedGraCol | CMYK | 涂层GRACoL 2006(ISO 12647-2:2004) |
| ColorMatchRGB | RGB | 颜色匹配RGB |
| EuropeISOCopated | CMYK | 欧洲ISO涂层FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoated | CMYK | Japan Color 2001 Coated |
| JapanColorSpeaper | CMYK | Japan Color 2002报纸 |
| JapanColorUncoated | CMYK | Japan Color 2001 Uncoated |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated(Ad) |
| 新闻纸SNAP2007 | CMYK | 美国新闻纸(SNAP 2007) |
| NTSC | RGB | NTSC(1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | Photoshop4默认CMYK |
| PS5默认 | CMYK | Photoshop5默认CMYK |
| SheetfedCoated | CMYK | 美国平板电脑涂层v2 |
| 平板纸未涂层 | CMYK | 美国平板纸未涂层v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 非涂层FOGRA29(ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated(SWOP)v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28(ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Web Coated SWOP 2006 3级纸 |
| WebCoatedGrade5 | CMYK | Web Coated SWOP 2006五级纸 |
| WebUncoated | CMYK | U.S. Web Uncoated v2 |
| WideGamyRGB | RGB | 宽色域RGB |

## 其他资源{#additional-resources}

* [配置Dynamic Media颜色管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
