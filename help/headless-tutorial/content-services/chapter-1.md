---
title: 第1章 — 教學課程設定和下載 — 內容服務
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: AEM Headless教學課程的第1章教學課程的AEM執行個體的基準線設定。
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 1%

---

# 教學課程設定

建議一律使用最新版AEM和AEM WCM核心元件。

* AEM 6.5 或更高版本
* AEM WCM Core Components 2.4.0或更新版本
   * 包含在 [下方的WKND Mobile AEM應用程式內容套件](#wknd-mobile-application-packages)

開始進行本教學課程之前，請確定下列AEM例項為 [已在本機電腦上安裝並執行](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)：

* **AEM作者** 於 **連線埠4502**
* **AEM發佈** 於 **連線埠4503**

## WKND行動應用程式套件{#wknd-mobile-application-packages}

在上安裝下列AEM內容套件 **兩者** AEM Author和AEM Publish，使用 [!DNL AEM Package Manager].

* [ui.apps： GitHub >資產> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM核心元件的Proxy元件
   * [!DNL WKND Mobile] AEM Content Services頁面的CSS （用於次要樣式）
* [ui.content： GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 站点结构
   * [!DNL WKND Mobile] DAM資料夾結構
   * [!DNL WKND Mobile] 影像資產

在 [第7章](./chapter-7.md) 我們將執行 [!DNL WKND Mobile] Android行動應用程式，使用 [Android Studio](https://developer.android.com/studio) 以及提供的APK （Android應用程式套件）：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章節AEM內容套件

這組內容套件會建立相關章節及所有先前章節中說明的內容和設定。 這些套件是選用套件，但可加快內容建立速度。

* [第2章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第3章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第4章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第5章內容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 源代码

AEM專案的原始程式碼和 [!DNL Android Mobile App] 可在以下網址取得： [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). 您不需要為本教學課程建置或修改原始程式碼，提供原始程式碼是為了在建置教學課程所有內容的方式上完全透明。

如果您發現教學課程或程式碼有問題，請留下 [GitHub問題](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## 跳至結尾

若要跳至教學課程結尾，請前往 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 內容套件可以安裝在 **兩者** AEM Author和AEM Publish。 請注意，內容和設定不會顯示為AEM Author中發佈，但由於手動部署，所有必要內容和設定都可在AEM Publish上取得，允許 [!DNL WKND Mobile App] 以存取內容。


## 下一步

* [第2章 — 定義事件內容片段模型](./chapter-2.md)
