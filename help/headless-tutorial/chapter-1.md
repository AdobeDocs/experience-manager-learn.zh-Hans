---
title: 第1章——教程设置和下载
seo-title: AEM Content Services入门——第1章——教程设置
description: AEM无头教程的第1章教程的AEM实例的基线设置。
seo-description: AEM无头教程的第1章教程的AEM实例的基线设置。
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---


# 教程设置

始终建议使用最新版AEM和AEM WCM核心组件。

* AEM 6.5 或更高版本
* AEM WCM核心组件2.4.0或更高版本
   * 包含在以下 [WKND Mobile AEM应用程序内容包中](#wknd-mobile-application-packages)

在开始本教程之前，请确保在本地计 [算机上安装并运行以下AEM实例](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM作者** ，端 **口4502**
* **AEM Publish** on **port 4503**

## WKND Mobile应用程序包{#wknd-mobile-application-packages}

使用在AEM作者和AEM发 **布** 上安装以下AEM内容包 [!DNL AEM Package Manager]。

* [ui.apps:GitHub >资产> com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM核心组件的代理组件
   * [!DNL WKND Mobile] AEM Content Services页面的CSS（用于小样式）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] 站点结构
   * [!DNL WKND Mobile] DAM文件夹结构
   * [!DNL WKND Mobile] 图像资产

在 [第7章](./chapter-7.md) ，我们将使用Android [!DNL WKND Mobile] Studio和提供的APK(Android应 [用程序包](https://developer.android.com/studio) )运行Android移动应用程序：

* [[!DNL Android移动应用程序：GitHub >资产> wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 第AEM内容包

这组内容包创建相关章节和所有前几章中描述的内容和配置。 这些包是可选的，但可以加快内容创建。

* [第二章内容：GitHub >资产> com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第三章内容：GitHub >资产> com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第四章内容：GitHub >资产> com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第五章内容：GitHub >资产> com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 源代码

[!DNL AEM指南- WKND Mobile [!DNL Android Mobile App] GitHub项目]中 [提供AEM项目和的源代码](https://github.com/adobe/aem-guides-wknd-mobile)。 无需为本教程构建或修改源代码，它的提供是为了在构建教程的各个方面时实现完全透明。

如果您发现教程或代码存在问题，请保留 [GitHub问题](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 跳到结尾

为了跳到教程的结尾， [可在AEM作者和AEM发布上安装com.adobe.aem.guides.](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) wknd-mobile.content.chapter-5 **** .zip内容包。 请注意，内容和配置不会在AEM作者中显示为已发布，但由于手动部署，所有必需的内容和配置都将在AEM发布中可用，允许 [!DNL WKND Mobile App] 访问内容。


## 下一步

* [第2章——定义事件内容片段模型](./chapter-2.md)
