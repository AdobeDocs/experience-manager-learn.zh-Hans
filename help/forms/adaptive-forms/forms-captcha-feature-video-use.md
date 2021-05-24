---
title: 将CAPTCHA与AEM Adaptive Forms结合使用
seo-title: 将CAPTCHA与AEM Adaptive Forms结合使用
description: 在AEM自适应Forms中添加和使用CAPTCHA。
seo-description: 在AEM自适应Forms中添加和使用CAPTCHA。
feature: 自适应Forms，工作流
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
topic: 开发
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---


# 将CAPTCHA与AEM Adaptive Forms结合使用{#using-captchas-with-aem-adaptive-forms}

在AEM自适应Forms中添加和使用CAPTCHA。

请访问[AEM Forms示例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)页面，获取此功能的实时演示链接。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*此视频介绍如何使用内置的AEM CAPTCHA服务以及Google reCAPTCHA服务，将CAPTCHA添加到AEM自适应表单的过程。*

>[!NOTE]
>
>此功能仅在AEM 6.3之后可用。

>[!NOTE]
>
>**要在发布实例上配置reCaptcha，请按照以下步骤操作**
>
>在创作实例上配置reCaptach
>
>在创作实例上打开felix [web控制台](http://localhost:4502/system/console/bundles)
>
>搜索com.adobe.granite.crypto.file包
>
>记下包ID。 就我来说是20
>
>导航到创作实例上文件系统上的包ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 复制HMAC和主控文件

在您的发布实例上打开[felix web控制台](http://localhost:4502/system/console/bundles)。 搜索com.adobe.granite.crypto.file包。 记下包ID
导航到发布实例文件系统上的包ID
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 删除现有的HMAC和主控文件。
* 粘贴从创作实例复制的HMAC和主控文件

重新启动AEM发布服务器

## 辅助材料{#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

