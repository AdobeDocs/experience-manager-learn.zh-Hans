---
title: 使用AEM Assets和InDesign Server設定資產範本
description: 資產範本可讓行銷人員建立、管理和提供數位資產和列印資產。 與InDesign伺服器整合時，資產範本可讓您輕鬆建立行銷手冊、名片、傳單、廣告和明信片。 本節將說明使用AEM設定InDesign伺服器的相關資訊。
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# 使用AEM Assets和InDesign Server設定資產範本{#set-up-asset-templates-with-aem-assets-and-indesign-server}

資產範本可讓行銷人員建立、管理和提供數位資產和列印資產。 與InDesign伺服器整合時，資產範本可讓您輕鬆建立行銷手冊、名片、傳單、廣告和明信片。 本節將說明使用AEM設定InDesign伺服器的相關資訊。

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **必須** 上傳INDD範本時連線至執行中的InDesign伺服器。 INDD檔案上的部分初始處理需要InDesign伺服器。

## 下載InDesign Server試用版 {#download-indesign-server-trial}

下載 [InDesign Server試用下載網站](https://www.adobeprerelease.com/)

## 起始InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
