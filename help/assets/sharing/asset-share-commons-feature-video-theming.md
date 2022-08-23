---
title: 资产共享共用中的主题介绍
description: 有关功能和技术了解的材料资产共享共用
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 1%

---

# 资产共享共用中的主题介绍 {#asset-share-commons-theme}

资产共享共用中的主题简介。 该视频将逐步介绍如何使用自定义颜色方案创建新主题。

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

在此视频中，将根据资产共享共享共用深色主题创建一个新主题。 配色方案将与自定义徽标匹配，以便为网站提供一致的外观。

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

* [资产共享共用版本下载](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [ACS AEM Commons 3.11.0+版本下载](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)
