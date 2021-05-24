---
title: 第1章 — 教程设置和下载 — 内容服务
seo-title: AEM Content Services快速入门 — 第1章 — 教程设置
description: 《AEM Headless教程》的第1章，介绍了本教程中AEM实例的基线设置。
seo-description: 《AEM Headless教程》的第1章，介绍了本教程中AEM实例的基线设置。
feature: 内容片段、 API
topic: 无外设、内容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%

---


# 教程设置

始终建议使用最新版本的AEM和AEM WCM核心组件。

* AEM 6.5 或更高版本
* AEM WCM核心组件2.4.0或更高版本
   * 包含在](#wknd-mobile-application-packages)下方的[WKND Mobile AEM应用程序内容包中

在启动本教程之前，请确保在本地计算机](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)上安装并运行以下AEM实例：[

* **AEM**  Authoron **端口4502**
* **AEM** Publish **端口4503**

## WKND移动应用程序包{#wknd-mobile-application-packages}

使用[!DNL AEM Package Manager]在&#x200B;**AEM作者和AEM发布上同时安装以下AEM内容包。**

* [ui.apps:GitHub >资产> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM核心组件的代理组件
   * [!DNL WKND Mobile] AEM Content Services页面的CSS（用于次要样式）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 网站结构
   * [!DNL WKND Mobile] DAM文件夹结构
   * [!DNL WKND Mobile] 图像资产

在[第7章](./chapter-7.md)中，我们将使用[Android Studio](https://developer.android.com/studio)和提供的APK（Android应用程序包）运行[!DNL WKND Mobile] Android移动应用程序：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章节AEM内容包

这组内容包将创建相关章节和所有前几章中所述的内容和配置。 这些包是可选的，但可以加快内容的创建速度。

* [第二章内容：GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第三章内容：GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第四章内容：GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第五章内容：GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 源代码

[[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)上提供了AEM项目和[!DNL Android Mobile App]的源代码。 在本教程中，无需构建或修改源代码，提供此源代码是为了在构建教程所有方面的方式上实现完全透明。

如果您发现本教程或代码存在问题，请保留[GitHub问题](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 跳到结尾

为了跳到本教程的结尾，[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包可同时安装在&#x200B;**AEM创作和AEM发布上。**&#x200B;请注意，内容和配置将不会在AEM作者中显示为已发布，但由于手动部署，所有必需的内容和配置将在AEM发布中可用，从而允许[!DNL WKND Mobile App]访问该内容。


## 下一步

* [第2章 — 定义事件内容片段模型](./chapter-2.md)
