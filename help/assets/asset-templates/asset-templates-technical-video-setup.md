---
title: 使用AEM Assets和InDesign Server设置资产模板
description: 资产模板使营销人员能够创建、管理和投放适用于数字和印刷品的数字资产。 与InDesign服务器集成后，使用资产模板可更轻松地创建营销宣传册、名片、传单、广告和明信片。 本节介绍使用AEM配置InDesign服务器。
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---


# 使用AEM Assets和InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}设置资产模板

资产模板使营销人员能够创建、管理和投放适用于数字和印刷品的数字资产。 与InDesign服务器集成后，使用资产模板可更轻松地创建营销宣传册、名片、传单、广告和明信片。 本节介绍使用AEM配置InDesign服务器。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>上载INDD模板时，AEM **必须**&#x200B;连接到正在运行的InDesign服务器。 INDD文件的初始处理部分需要InDesign服务器。

## 下载InDesign Server试用版{#download-indesign-server-trial}

下载[InDesign Server试用版下载网站](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## 开始InDesign Server{#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
