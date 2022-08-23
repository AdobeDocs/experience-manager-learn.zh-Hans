---
title: 了解使用AEM Dynamic Media进行色彩管理
description: 在此视频中，我们将探索Dynamic Media色彩管理，以及如何使用它在中为AEM Assets提供颜色校正预览功能。
sub-product: dynamic-media
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 17%

---

# 了解使用AEM Dynamic Media进行色彩管理{#understanding-color-management-with-aem-dynamic-media}

在此视频中，我们将探索Dynamic Media色彩管理，以及如何使用它在中为AEM Assets提供颜色校正预览功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[启用Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=zh-Hans) 在AEM中使用此功能。

此功能可作为功能包，在AEM 6.1和6.2版本中使用。

## 色彩管理配置节点的XML模板 {#xml-template-for-the-color-management-configuration-node}

以下是“色彩管理”配置节点的XML模板。 此XML模板可复制到AEM开发项目中，并使用适合项目的配置进行配置。

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

### 下面列出了默认Adobe颜色配置文件列表 {#list-of-default-adobe-color-profiles-are-listed-below}

| 名称 | 色彩空间 | 描述 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB（1998年） |
| AppleRGB | RGB | AppleRGB |
| CIERGB | RGB | CIERGB |
| CoatedFogra27 | CMYK | 涂层FOGRA27(ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | 涂层FOGRA39(ISO 12647-2:2004) |
| CoatedGraCol | CMYK | 涂层GRACoL 2006(ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatchRGB |
| EuropeISOCoated | CMYK | 欧洲ISO涂层FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoated | CMYK | 日本颜色2001涂层 |
| JapanColorJeappe | CMYK | 日本彩色2002报纸 |
| JapanColorUncoated | CMYK | 日本颜色2001无涂层 |
| JapanColorWebCoated | CMYK | 日本Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated（广告） |
| 新闻纸SNAP2007 | CMYK | 美国新闻纸（2007年快照） |
| NTSC | RGB | NTSC（1953年） |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhotoRGB |
| PS4Default | CMYK | Photoshop 4默认CMYK |
| PS5默认 | CMYK | Photoshop 5默认CMYK |
| SheetfedCoated | CMYK | 美国钣金涂层v2 |
| SheetfedUncoated | CMYK | 美国平板纸未涂层v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGBsRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 无涂层FOGRA29(ISO 12647-2:2004) |
| WebCoated | CMYK | 美国涂层网络(SWOP)v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28(ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Web版SWOP 2006三级纸 |
| WebCoatedGrade5 | CMYK | Web版SWOP 2006五级纸 |
| WebUncoated | CMYK | 美国Web Uncoated v2 |
| 宽色域RGB | RGB | 宽色域RGB |

## 其他资源{#additional-resources}

* [配置Dynamic Media色彩管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
