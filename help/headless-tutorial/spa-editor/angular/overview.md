---
title: AEM SPA Editor 和 Angular 快速入门
description: 创建您的首个 Angular 单页面应用程序 (SPA)，该应用程序可在带 WKND SPA 的 Adobe Experience Manager (AEM) 中编辑。
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 14%

---

# 在AEM中建立您的第一個AngularSPA {#introduction}

歡迎使用專為「 」的新手開發人員設計的多部分教學課程 **SPA編輯器** Adobe Experience Manager (AEM)中的功能。 本教學課程會逐步引導您為虛擬生活風格品牌WKND實作Angular應用程式。 angular應用程式是以AEM SPA Editor開發並設計來部署，可將Angular元件對應至AEM元件。 部署至AEM的完整SPA可透過AEM的傳統內嵌編輯工具動態撰寫。

![實作的最終SPA](assets/wknd-spa-implementation.png)

*WKND SPA實作*

## 关于

此多部分教學課程的目標是教導開發人員如何實作Angular應用程式，以使用AEM的SPA編輯器功能。 在真實世界情境中，開發活動會依人員細分，通常涉及 **前端開發人員** 和 **後端開發人員**. 我們認為任何參與AEM SPA Editor專案的開發人員完成本教學課程都會有助益。

本教學課程的設計用途為 **AEMas a Cloud Service** 並且向後相容於 **AEM 6.5.4+** 和 **AEM 6.4.8+**. SPA的實作方式：

* [Maven AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA編輯器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [angular](https://angular.io/)

*預計約需1-2小時完成教學課程的每個部分。*

## 最新程式碼

您可在上找到教學課程的所有程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

此 [最新的程式碼基底](https://github.com/adobe/aem-guides-wknd-spa/releases) 可作為可下載的AEM套件使用。

## 前提条件

在開始進行本教學課程之前，您需要具備下列條件：

* HTML、CSS和JavaScript的基本知識
* 基本熟悉 [angular](https://angular.io/)
* [AEMAS A CLOUD SERVICESDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)， [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

*雖然不一定需要，但若能大致瞭解 [開發傳統AEM Sites元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans).*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成本教學課程的必要條件。 熒幕截圖和影片都是使用在Mac作業系統環境上執行的AEMas a Cloud ServiceSDK擷取，並具有 [Visual Studio Code](https://code.visualstudio.com/) 作為IDE。 除非另有說明，否則命令和程式碼應獨立於本機作業系統。

>[!NOTE]
>
> **不熟悉AEMas a Cloud Service？** 檢視 [以下是使用AEMas a Cloud ServiceSDK設定本機開發環境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).
>
> **AEM 6.5的新手嗎？** 檢視 [遵循指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans).

## 后续步骤 {#next-steps}

您還在等什麼?!導覽至「 」，開始進行教學課程 [SPA編輯器專案](create-project.md) 章節，並瞭解如何使用SPA專案原型產生啟用AEM編輯器的專案。

## 回溯相容性 {#compatibility}

本教學課程的專案程式碼是針對AEMas a Cloud Service所建置。 為了讓專案程式碼向下相容 **6.5.4+** 和 **6.4.8+** 已進行數項修改。

此 [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** 已作為相依性納入：

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

其他Maven設定檔，已命名 `classic` 已新增以修改建置，以針對AEM 6.x環境：

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

此 `classic` 設定檔預設為停用。 如果使用AEM 6.x完成教學課程，請新增 `classic` 每當收到指示要執行Maven構建時的設定檔：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

產生AEM實作的新專案時，一律使用最新版本的 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 並更新 `aemVersion` 以鎖定您預期的AEM版本。
