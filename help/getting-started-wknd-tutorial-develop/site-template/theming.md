---
title: 主題設定工作流程 | AEM快速網站建立
description: 瞭解如何更新Adobe Experience Manager網站的主題來源，以套用品牌特定的樣式。 瞭解如何使用Proxy伺服器檢視CSS和Javascript更新的即時預覽。 本教學課程也將涵蓋如何使用Adobe Cloud Manager的前端管道將主題更新部署到AEM網站。
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 1%

---

# 主題設定工作流程 {#theming}

在本章中，我們會更新Adobe Experience Manager網站的主題來源，以套用品牌特定的樣式。 我們瞭解如何使用Proxy伺服器來檢視CSS和Javascript更新的預覽，同時針對即時網站進行編碼。 本教學課程也將涵蓋如何使用Adobe Cloud Manager的前端管道將主題更新部署到AEM網站。

最後，我們的網站已更新，納入符合WKND品牌的樣式。

## 前提条件 {#prerequisites}

此教學課程包含多個部分，並假設您已完成下列步驟： [頁面範本](./page-templates.md) 章節已完成。

## 目標

1. 瞭解如何下載及修改網站的主題來源。
1. 瞭解針對即時網站的程式碼如何供即時預覽。
1. 瞭解使用Adobe Cloud Manager的前端管道將編譯的CSS和JavaScript更新交付為主題一部分的端到端工作流程。

## 更新主題 {#theme-update}

接下來，變更主題來源，讓網站符合WKND品牌的外觀和風格。

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

影片的高層級步驟：

1. 在AEM中建立本機使用者，以搭配Proxy開發伺服器使用。
1. 從AEM下載主題來源，並使用本機IDE （如VSCode）開啟。
1. 修改主題來源，並使用Proxy開發伺服器來即時預覽CSS和JavaScript變更。
1. 更新主題來源，使雜誌文章符合WKND品牌樣式和模型。

### 解決方案檔案

下載完成的樣式 [WKND範例主題](assets/theming/WKND-THEME-src-1.1.zip)

## 使用前端管道部署主題 {#deploy-theme}

使用Cloud Manager的前端管道將主題的更新部署到AEM環境。

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

影片的高層級步驟：

1. 建立新的Git [Cloud Manager中的存放庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. 將您的主題來源專案新增到Cloud Manager Git存放庫：

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 設定 [前端管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) 在Cloud Manager中部署前端計畫碼。
1. 執行前端管道以將更新部署到目標AEM環境。

### 存放庫範例

有兩個GitHub存放庫範例可作為參考使用：

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 用於「真實世界」專案的範例。

## 恭喜！ {#congratulations}

恭喜，您剛才已更新並部署主題至AEM！

### 后续步骤 {#next-steps}

更深入瞭解AEM開發，並透過使用建立網站，進一步瞭解基礎技術 [AEM專案原型](../project-archetype/overview.md).
