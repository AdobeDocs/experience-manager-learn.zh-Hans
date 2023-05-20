---
title: 透過AEM Dynamic Media瞭解色彩管理
description: 在本影片中，我們將探索Dynamic Media色彩管理，以及如何使用它在AEM Assets中提供色彩校正預覽功能。
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 17%

---

# 透過AEM Dynamic Media瞭解色彩管理{#understanding-color-management-with-aem-dynamic-media}

在本影片中，我們將探索Dynamic Media色彩管理，以及如何使用它在AEM Assets中提供色彩校正預覽功能。

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[啟用Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=zh-Hans) AEM以使用此功能。

此功能以Feature Pack的形式提供給AEM 6.1和6.2版本。

## 色彩管理組態節點的XML範本 {#xml-template-for-the-color-management-configuration-node}

以下是「色彩管理」組態節點的XML範本。 此XML範本可複製到AEM開發專案，並使用適合專案的設定進行設定。

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

### 預設Adobe色彩設定檔清單如下 {#list-of-default-adobe-color-profiles-are-listed-below}

| 名称 | 色彩空间 | 描述 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | AppleRGB |
| CIERGB | RGB | CIERGB |
| CoatedFogra27 | CMYK | COATED FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | COATED FOGRA39 (ISO 12647-2:2004) |
| CoatedGraCol | CMYK | 鍍膜GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatchRGB |
| EuropeISOCoated | CMYK | 歐洲ISO銅版FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoated | CMYK | Japan Color 2001塗裝 |
| JapanColorNewspaper | CMYK | Japan Color 2002報紙 |
| JapanColorUncoated | CMYK | Japan Color 2001 Uncoated |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated （廣告） |
| NewsprintSNAP2007 | CMYK | 美國新聞紙(SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhotoRGB |
| PS4Default | CMYK | Photoshop 4預設CMYK |
| PS5Default | CMYK | Photoshop 5預設CMYK |
| SheetfedCoated | CMYK | U.S. Sheetfed Coated v2 |
| SheetfedUncoated | CMYK | U.S. Sheetfed Uncoated v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | sRGBRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | 未塗層的FOGRA29 (ISO 12647-2:2004) |
| 網頁塗層 | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | 網頁版SWOP 2006 3級紙張 |
| WebCoatedGrade5 | CMYK | 網頁版SWOP 2006 5級紙張 |
| WebUncoated | CMYK | U.S. Web Uncoated v2 |
| WideGamutRGB | RGB | 寬色域RGB |

## 其他资源{#additional-resources}

* [設定Dynamic Media色彩管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
