---
title: AEM Sites — 專案原型快速入門
description: AEM Sites — 專案原型快速入門。 WKND教學課程是多部分教學課程，專為Adobe Experience Manager的新手開發人員設計。 此教學課程會逐步引導您為虛擬生活風格品牌WKND實作AEM網站。 此教學課程涵蓋專案設定、maven原型、核心元件、可編輯範本、使用者端程式庫和元件開發等基本主題。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 20%

---

# AEM Sites — 專案原型快速入門 {#project-archetype}

歡迎使用專為Adobe Experience Manager (AEM)的新手開發人員設計的多部分教學課程。 本教程介绍了虚拟生活方式品牌 WKND 的 AEM 站点的实现。 

本教學課程從使用開始 [AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 以產生新專案。

本教學課程的設計用途為 **AEMas a Cloud Service** 並且向後相容於 **AEM 6.5.14+**. 使用下列專案實作網站：

* [Maven AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [可编辑模板](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=zh-Hans)
* [样式系统](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計約需1-2小時完成教學課程的每個部分。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成本教學課程的必要條件。 熒幕截圖和影片都是使用在macOS環境中執行的AEMas a Cloud ServiceSDK擷取，並具有 [Visual Studio Code](https://code.visualstudio.com/) 作為IDE。 除非另有說明，否則命令和程式碼應獨立於本機作業系統。

### 必要的軟體

下列專案應在本機安裝：

* [本機AEM **作者** 例項](https://experience.adobe.com/#/downloads) (Cloud Service SDK或6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.js](https://nodejs.org/en/) （LTS — 長期支援）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) 或同等的IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 教學課程中使用的工具

>[!NOTE]
>
> **不熟悉AEMas a Cloud Service？** 檢視 [以下是使用AEMas a Cloud ServiceSDK設定本機開發環境的指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans).
>
> **AEM 6.5的新手嗎？** 檢視 [遵循指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans).

## GitHub {#github}

本教學課程的程式碼可在AEM Guide存放庫的GitHub上找到：

**[GitHub： WKND網站專案](https://github.com/adobe/aem-guides-wknd)**

此外，教學課程的每一部分在GitHub中都有各自的分支。 使用者只需出庫與上一個零件對應的分支即可隨時開始進行教學課程。

## 后续步骤 {#next-steps}

您還在等什麼？ 導覽至「 」，開始進行教學課程 [專案設定](project-setup.md) 章節，並瞭解如何使用AEM專案原型產生新的Adobe Experience Manager專案。
