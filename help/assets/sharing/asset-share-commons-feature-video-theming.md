---
title: Asset Share Commons中的主题介绍
seo-title: Asset Share Commons中的主题介绍
description: 功能和技术理解的材料Assets Share Commons
seo-description: 功能和技术理解的材料Assets Share Commons
uuid: 5991a015-392a-4bb5-8332-192681505b07
discoiquuid: 08a5a394-c62b-4748-b303-33117f283612
contentOwner: dgonzale
feature: asset-share, brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '131'
ht-degree: 1%

---


# Asset Share Commons {#asset-share-commons-theme}中的主题介绍

Asset Share Commons中主题的简介。 视频将逐步介绍如何使用自定义颜色方案创建新主题。

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

在此视频中，将基于Asset Share Commons Dark主题创建新主题。 颜色方案将与自定义徽标匹配，使站点具有一致的外观。

## 主题变量

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

下载[自定义客户端库主题](assets/asc-theme-demo.zip)

## 其他资源{#additional-resources}

* [Asset Share Commons版本下载](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+版本下载](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)