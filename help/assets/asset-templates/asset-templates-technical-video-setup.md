---
title: 使用AEM Assets和InDesign Server设置资源模板
description: 资产模板允许营销人员创建、管理和交付数字资产和打印资产。 与InDesign服务器集成后，使用资产模板可更轻松地创建营销宣传册、名片、传单、广告和明信片。 本节介绍了如何使用AEM配置InDesign服务器。
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# 使用AEM Assets和InDesign Server设置资源模板{#set-up-asset-templates-with-aem-assets-and-indesign-server}

资产模板允许营销人员创建、管理和交付数字资产和打印资产。 与InDesign服务器集成后，使用资产模板可更轻松地创建营销宣传册、名片、传单、广告和明信片。 本节介绍了如何使用AEM配置InDesign服务器。

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>上传INDDInDesign时，AEM **必须**&#x200B;连接到正在运行的模板服务器。 对INDD文件的部分初始处理需要InDesign服务器。

## 下载InDesign Server试用版 {#download-indesign-server-trial}

下载[InDesign Server试用下载网站](https://www.adobeprerelease.com/)

## 起始InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
