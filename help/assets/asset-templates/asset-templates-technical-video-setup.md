---
title: 使用AEM Assets和InDesign Server设置资产模板
description: 资产模板允许营销人员创建、管理和交付用于数字和打印的数字资产。 与InDesign服务器集成后，使用资产模板可以更轻松地创建营销手册、名片、传单、广告和明信片。 使用AEM配置InDesign服务器的过程将在此部分中介绍。
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# 使用AEM Assets和InDesign Server设置资产模板{#set-up-asset-templates-with-aem-assets-and-indesign-server}

资产模板允许营销人员创建、管理和交付用于数字和打印的数字资产。 与InDesign服务器集成后，使用资产模板可以更轻松地创建营销手册、名片、传单、广告和明信片。 使用AEM配置InDesign服务器的过程将在此部分中介绍。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>上传INDD模板后，AEM **必须**&#x200B;连接到正在运行的InDesign服务器。 INDD文件的初始处理部分需要InDesign服务器。

## 下载InDesign Server试用版 {#download-indesign-server-trial}

下载[InDesign Server试用版下载网站](https://www.adobe.com/devnet/premiere/sdk/cs5/indesign-server-trial-downloads.html)

## 开始InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
