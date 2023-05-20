---
title: AEM SPA Editor 和 React 快速入门
description: 创建您的首个 React 单页面应用程序 (SPA)，该应用程序可在带 WKND SPA 的 Adobe Experience Manager (AEM) 中编辑。了解如何结合使用 React JS 框架和 AEM 的 SPA 编辑器来创建 SPA。此多节教程演练了为虚构的生活方式品牌 WKND 实施 React 应用程序的过程。本教程涉及创建 SPA 的全过程以及与 AEM 的集成。
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 24%

---

# 在AEM中建立您的第一個React SPA {#overview}

歡迎使用專為「 」的新手開發人員設計的多部分教學課程 **SPA編輯器** Adobe Experience Manager (AEM)中的功能。 本教學課程會逐步引導您為虛擬生活風格品牌WKND實作React應用程式。 React應用程式是使用AEM SPA Editor開發並設計來部署，可將React元件對應至AEM元件。 部署至AEM的完整SPA可透過AEM的傳統內嵌編輯工具動態撰寫。

![實作的最終SPA](assets/wknd-spa-implementation.png)

*WKND SPA實作*

## 关于

本教學課程的設計用途為 **AEMas a Cloud Service** 並且向後相容於 **AEM 6.5.4+** 和 **AEM 6.4.8+**.

## 最新程式碼

您可在上找到教學課程的所有程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

此 [最新的程式碼基底](https://github.com/adobe/aem-guides-wknd-spa/releases) 可作為可下載的AEM套件使用。

## 前提条件

在開始進行本教學課程之前，您需要具備下列條件：

* HTML、CSS和JavaScript的基本知識
* 基本熟悉 [React](https://reactjs.org/tutorial/tutorial.html)

*雖然不一定需要，但若能大致瞭解 [開發傳統AEM Sites元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans).*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成本教學課程的必要條件。 熒幕截圖和影片都是使用在Mac作業系統環境上執行的AEMas a Cloud ServiceSDK擷取，並具有 [Visual Studio Code](https://code.visualstudio.com/) 作為IDE。 除非另有說明，否則命令和程式碼應獨立於本機作業系統。

### 必要的軟體

* [AEMAS A CLOUD SERVICESDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)， [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) 或 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **不熟悉AEMas a Cloud Service？** 檢視 [以下是使用AEMas a Cloud ServiceSDK設定本機開發環境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).
>
> **AEM 6.5的新手嗎？** 檢視 [遵循指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans).

## 后续步骤 {#next-steps}

您還在等什麼?!導覽至「 」，開始進行教學課程 [建立專案](create-project.md) 章節，並瞭解如何使用SPA專案原型產生啟用AEM編輯器的專案。
