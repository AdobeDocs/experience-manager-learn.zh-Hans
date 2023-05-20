---
title: AEMas a Cloud Service的本機開發環境
description: Adobe Experience Manager (AEM)本機開發環境概觀。
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 14%

---

# 本機開發環境設定 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概述"
>abstract="为 AEM as a Cloud Service 设置本地开发环境包括开发、构建和编译 AEM 项目所需的开发工具，以及允许开发人员在通过 Adobe Cloud Manager 将新功能部署到 AEM as a Cloud Service 之前在本地快速验证这些新功能的本地运行时。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="开发准则"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hans" text="开发基础"

本教學課程會逐步引導您使用AEM as a Cloud Service SDK為Adobe Experience Manager (AEM)設定本機開發環境。 其中包括開發、建置和編譯AEM專案所需的開發工具，以及本機執行時間，讓開發人員在透過Adobe Cloud Manager將新功能部署到AEMas a Cloud Service之前，可以快速在本機驗證這些功能。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEMas a Cloud Service本機開發環境技術棧疊](./assets/overview/aem-sdk-technology-stack.png)

AEM的本機開發環境可分成三個邏輯群組：

+ 此 __AEM專案__ 包含自訂AEM應用程式的自訂程式碼、設定和內容。
+ 此 __本機AEM執行階段__ 會在本機執行AEM作者和發佈服務的本機版本。
+ 此 __本機Dispatcher執行階段__ 會執行本機版本的Apache HTTP Web Server和Dispatcher。

本教學課程會逐步解說如何安裝和設定上圖中醒目提示的專案，為AEM開發提供穩定的本機開發環境。

## 檔案系統組織

本教學課程建立AEMas a Cloud ServiceSDK成品和AEM專案程式碼的位置，如下所示：

+ `~/aem-sdk` 是包含AEMas a Cloud ServiceSDK所提供各種工具的組織資料夾
+ `~/aem-sdk/author` 包含AEM作者服務
+ `~/aem-sdk/publish` 包含AEM Publish服務
+ `~/aem-sdk/dispatcher` 包含Dispatcher工具
+ `~/code/<project name>` 包含自訂AEM專案原始碼

請注意 `~` 是使用者目錄的縮寫。 在Windows中，這相當於 `%HOMEPATH%`；

## AEM專案的開發工具

AEM專案是自訂程式碼基底，包含透過Cloud Manager部署到AEMas a Cloud Service的程式碼、設定和內容。 基準線專案結構是透過 [AEM專案Maven原型](https://github.com/adobe/aem-project-archetype).

教學課程的這個區段會示範如何：

+ 安装 [!DNL Java]
+ 安裝 [!DNL Node.js] （和npm）
+ 安装 [!DNL Maven]
+ 安装 [!DNL Git]

[設定AEM專案的開發工具](./development-tools.md)

## 本地 AEM 运行时

AEMas a Cloud ServiceSDK提供 [!DNL QuickStart Jar] 執行本機版本的AEM。 此 [!DNL QuickStart Jar] 可用於在本機執行AEM作者服務或AEM發佈服務。 請注意，雖然 [!DNL QuickStart Jar] 提供本機開發體驗，並非所有AEMas a Cloud Service可用功能都包含在 [!DNL QuickStart Jar].

教學課程的這個區段會示範如何：

+ 安装 [!DNL Java]
+ 下載AEM SDK
+ 執行 [!DNL AEM Author Service]
+ 執行 [!DNL AEM Publish Service]

[設定本機AEM執行階段](./aem-runtime.md)

## 本機 [!DNL Dispatcher] 執行階段

AEMas a Cloud ServiceSDK的Dispatcher工具提供設定本機 [!DNL Dispatcher] 執行階段。 [!DNL Dispatcher] 工具為 [!DNL Docker]-based並提供命令列工具以傳輸 [!DNL Apache HTTP] Web伺服器和 [!DNL Dispatcher] 設定檔案為相容的格式並將其部署到 [!DNL Dispatcher] 在中執行 [!DNL Docker] 容器。

教學課程的這個區段會示範如何：

+ 下載AEM SDK
+ 安裝 [!DNL Dispatcher] 工具
+ 執行本機 [!DNL Dispatcher] 執行階段

[設定本機 [!DNL Dispatcher] 執行階段](./dispatcher-tools.md)
