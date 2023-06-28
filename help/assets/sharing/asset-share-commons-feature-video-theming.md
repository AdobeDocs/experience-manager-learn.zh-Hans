---
title: Asset Share Commons的主题简介
description: 有关功能和技术了解Assets共享公域的材料
version: 6.4, 6.5
topic: Content Management
feature: Asset Distribution
role: Developer
level: Intermediate
last-substantial-update: 2022-06-22T00:00:00Z
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 1%

---

# Asset Share Commons的主题简介 {#asset-share-commons-theme}

简单介绍资产共享共用中的主题。 该视频介绍了使用自定义颜色方案创建新主题的过程。

>[!VIDEO](https://video.tv.adobe.com/v/20572?quality=12&learn=on)

在此视频中，根据资产共享Commons Dark主题创建了新主题。 颜色方案将匹配自定义徽标，使网站具有一致的外观。

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

下载 [自定义客户端库主题](assets/asc-theme-demo.zip)

## 其他资源{#additional-resources}

* [Asset Share Commons版本下载](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+发行版下载](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
