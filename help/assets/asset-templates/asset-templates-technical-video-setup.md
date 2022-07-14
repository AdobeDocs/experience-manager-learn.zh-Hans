---
title: 使用AEM Assets和InDesign Server设置资产模板
description: 资产模板允许营销人员创建、管理和交付用于数字和打印的数字资产。 与InDesign服务器集成后，使用资产模板可以更轻松地创建营销手册、名片、传单、广告和明信片。 使用AEM配置InDesign服务器的过程将在此部分中介绍。
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: 6dd7164f5ec045b4cffd7732fd83ad9a91fdd511
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# 使用AEM Assets和InDesign Server设置资产模板{#set-up-asset-templates-with-aem-assets-and-indesign-server}

资产模板允许营销人员创建、管理和交付用于数字和打印的数字资产。 与InDesign服务器集成后，使用资产模板可以更轻松地创建营销手册、名片、传单、广告和明信片。 使用AEM配置InDesign服务器的过程将在此部分中介绍。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **必须** 上载INDD模板时，连接到正在运行的InDesign服务器。 INDD文件的初始处理部分需要InDesign服务器。

## 下载InDesign Server试用版 {#download-indesign-server-trial}

下载 [InDesign Server试用版下载网站](https://www.adobeprerelease.com/)

## 开始InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
