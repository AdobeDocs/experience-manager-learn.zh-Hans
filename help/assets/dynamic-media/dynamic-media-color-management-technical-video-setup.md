---
title: 了解使用AEM Dynamic Media进行颜色管理
description: 在本视频中，我们将介绍Dynamic Media色彩管理，以及如何使用它在for AEM Assets中提供色彩校正预览功能。
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Feature Video
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
duration: 302
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 11%

---

# 了解使用AEM Dynamic Media进行颜色管理{#understanding-color-management-with-aem-dynamic-media}

在本视频中，我们将介绍Dynamic Media色彩管理，以及如何使用它在for AEM Assets中提供色彩校正预览功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[启用Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) 在AEM中使用此功能。

此功能作为Feature Pack在AEM 6.1和6.2版本中提供。

## 颜色管理配置节点的XML模板 {#xml-template-for-the-color-management-configuration-node}

下面是“颜色管理”配置节点的XML模板。 可以将此XML模板复制到AEM开发项目中，并使用与项目相应的配置对其进行配置。

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

### 默认Adobe颜色配置文件的列表如下 {#list-of-default-adobe-color-profiles-are-listed-below}

| 名称 | 色彩空间 | 描述 |
| ------------------- | ---------- | ------------------------------------- |
| adobergb | RGB | Adobe RGB（1998年） |
| AppleRGB | RGB | AppleRGB |
| CIERGB | RGB | CIERGB |
| CoatedFogra27 | CMYK | 涂层的FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | 涂层纸FOGRA39 (ISO 12647-2:2004) |
| CoatedGraCol | CMYK | 涂层纸GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatchRGB |
| 欧洲ISOCoated | CMYK | 欧洲ISO铜版FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoated | CMYK | Japan Color 2001涂布 |
| JapanColorPaper | CMYK | 《日本彩色2002报纸》 |
| JapanColorUncoated | CMYK | Japan Color 2001无涂层 |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| Japanawebcoated | CMYK | 日本Web Coated (Ad) |
| 新闻纸快照2007 | CMYK | 美国新闻纸(SNAP 2007) |
| NTSC | RGB | NTSC （1953年） |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhotoRGB |
| PS4默认 | CMYK | Photoshop 4默认CMYK |
| PS5默认 | CMYK | Photoshop 5默认CMYK |
| SheetfedCoated | CMYK | U.S. Sheetfed Coated v2 |
| SheetfedUncoated | CMYK | U.S. Sheetfed Uncoated v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | sRGBRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 无涂层的FOGRA29 (ISO 12647-2:2004) |
| WebCoat | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | 网页涂层的FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Web涂层的SWOP 2006 3级纸 |
| WebCoatedGrade5 | CMYK | Web涂层的SWOP 2006 5级纸 |
| WebUncoated | CMYK | U.S. Web Uncoated v2 |
| 宽色域RGB | RGB | 宽色域RGB |

## 其他资源{#additional-resources}

* [配置Dynamic Media色彩管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
