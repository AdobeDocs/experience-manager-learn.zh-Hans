---
title: 第1章 — 教程设置和下载 — 内容服务
description: AEM Headless教程的第1章为教程的AEM实例设置基线。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 1%

---

# 教程设置

始终建议使用最新版本的AEM和AEM WCM核心组件。

* AEM 6.5或更高版本
* AEM WCM核心组件2.4.0或更高版本
   * 包含在 [以下的WKND Mobile AEM应用程序内容包](#wknd-mobile-application-packages)

在开始本教程之前，请确保以下AEM实例为 [已在本地计算机上安装并运行](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)：

* **AEM创作** 日期 **端口4502**
* **AEM发布** 日期 **端口4503**

## WKND移动应用程序包{#wknd-mobile-application-packages}

在上安装以下AEM内容包 **两者** AEM Author和AEM Publish，使用 [!DNL AEM Package Manager].

* [ui.apps： GitHub >资产> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM核心组件的代理组件
   * [!DNL WKND Mobile] AEM Content Services页面的CSS（用于次要样式）
* [ui.content： GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 站点结构
   * [!DNL WKND Mobile] DAM文件夹结构
   * [!DNL WKND Mobile] 图像资产

在 [第7章](./chapter-7.md) 我们将运行 [!DNL WKND Mobile] Android移动应用程序，使用 [Android Studio](https://developer.android.com/studio) 以及提供的APK（Android应用程序包）：

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 章节AEM内容包

这组内容包将创建相关章节以及前面所有章节中所述的内容和配置。 这些包是可选的，但可以加快内容创建。

* [第2章内容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第3章内容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第4章内容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第5章内容： GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 源代码

AEM项目和 [!DNL Android Mobile App] 可在 [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). 无需为本教程构建或修改源代码，提供此源代码是为了使构建教程各个方面的方式完全透明。

如果您发现教程或代码有问题，请留言 [GitHub问题](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## 跳至结尾

为了跳至本教程的结尾， [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 内容包可以安装在 **两者** AEM创作和AEM发布。 请注意，内容和配置将不会在AEM Author中显示为已发布，但是，由于手动部署，所有必需的内容和配置在AEM Publish上可用，允许 [!DNL WKND Mobile App] 以访问内容。


## 下一步

* [第2章 — 定义事件内容片段模型](./chapter-2.md)
