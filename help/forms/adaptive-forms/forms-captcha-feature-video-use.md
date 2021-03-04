---
title: 将CAPTCHA与AEM Adaptive Forms结合使用
seo-title: 将CAPTCHA与AEM Adaptive Forms结合使用
description: 在AEM Adaptive Forms中添加和使用CAPTCHA。
seo-description: 在AEM Adaptive Forms中添加和使用CAPTCHA。
feature: 自适应表单
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---


# 将CAPTCHA与AEM Adaptive Forms一起使用{#using-captchas-with-aem-adaptive-forms}

在AEM Adaptive Forms中添加和使用CAPTCHA。

请访问[AEM Forms示例](https://forms.enablementadobe.com/content/samples/samples.html?query=0)页面，获取此功能的实时演示的链接。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*此视频介绍使用内置的AEM CAPTCHA服务和Google的reCAPTCHA服务将CAPTCHA添加到AEM自适应表单的过程。*

>[!NOTE]
>
>此功能仅从AEM 6.3开始提供。

>[!NOTE]
>
>**要在发布实例上配置reCaptcha，请按照以下步骤操作**
>
>在创作实例上配置reCaptach
>
>在创作实例上打开felix [ web console](http://localhost:4502/system/console/bundles)
>
>搜索com.adobe.granite.crypto.file包
>
>记下bundle id。 我的例子是20
>
>导航到创作实例上文件系统上的bundle id
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 复制HMAC和主控文件

在您的发布实例上打开[felix web console](http://localhost:4502/system/console/bundles)。 搜索com.adobe.granite.crypto.file包。 请记下bundle id
导航到发布实例的文件系统上的bundle id
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 删除现有的HMAC和主控文件。
* 粘贴从创作实例复制的HMAC和主控文件

重新启动AEM发布服务器

## 支撑材料{#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

