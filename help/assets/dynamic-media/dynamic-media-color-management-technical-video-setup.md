---
title: 了解使用AEM Dynamic Media进行色彩管理
description: 在此视频中，我们将探索Dynamic Media色彩管理，以及如何使用它为AEM Assets提供色彩校正预览功能。
sub-product: dynamic-media
feature: Image Profiles, Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 15%

---


# 了解使用AEM Dynamic Media进行色彩管理{#understanding-color-management-with-aem-dynamic-media}

在此视频中，我们将探索Dynamic Media色彩管理，以及如何使用它为AEM Assets提供色彩校正预览功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[启用](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) Dynamic Media AEM以使用此功能。

此功能可作为功能包用于AEM 6.1和6.2版本。

## 颜色管理配置节点{#xml-template-for-the-color-management-configuration-node}的XML模板

以下是“色彩管理”配置节点的XML模板。 可以将此XML模板复制到AEM开发项目中，并使用适合项目的配置进行配置。

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

### 默认Adobe颜色用户档案的列表列在{#list-of-default-adobe-color-profiles-are-listed-below}下

| 名称 | 色彩空间 | 描述 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB(1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Coated FOGRA27(ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Coated FOGRA39(ISO 12647-2:2004) |
| CoatedGraCol | CMYK | Coated GRACoL 2006(ISO 12647-2:2004) |
| ColorMatchRGB | RGB | 颜色匹配RGB |
| 欧洲ISOC | CMYK | 欧洲ISO涂层FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoated | CMYK | Japan Color 2001 Coated |
| JapanColorSpeable | CMYK | Japan Color 2002报纸 |
| JapanColorUncoated | CMYK | Japan Color 2001 Uncoated |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated(Ad) |
| 新闻纸SNAP2007 | CMYK | 美国新闻纸（2007年快照） |
| NTSC | RGB | NTSC(1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | Photoshop 4默认CMYK |
| PS5默认 | CMYK | Photoshop 5默认CMYK |
| SheetfedCoated | CMYK | 美国Sheetfed Coated v2 |
| SheetfedUncoated | CMYK | 美国平板纸未涂层v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 非涂层FOGRA29(ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated(SWOP)v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28(ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Web Coated SWOP 2006 3级纸 |
| WebCoatedGrade5 | CMYK | Web Coated SWOP 2006五级纸 |
| WebUncoated | CMYK | 美国Web Uncoated v2 |
| WideGalytRGB | RGB | 宽色域RGB |

## 其他资源{#additional-resources}

* [配置Dynamic Media颜色管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
