---
title: 将CAPTCHA与AEM Adaptive Depative Forms结合使用
seo-title: 将CAPTCHA与AEM Adaptive Depative Forms结合使用
description: 在AEM Adaptive Forms中添加和使用CAPTCHA。
seo-description: 在AEM Adaptive Forms中添加和使用CAPTCHA。
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# 将CAPTCHA与AEM Adaptive Depative Forms结合使用{#using-captchas-with-aem-adaptive-forms}

在AEM Adaptive Forms中添加和使用CAPTCHA。

请访问 [AEM Forms示例](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 页，获取此功能的实时演示的链接。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*此视频介绍使用内置的AEM CAPTCHA服务和Google的reCAPTCHA服务将CAPTCHA添加到AEM Adaptive Form的过程。*

>[!NOTE]
>
>此功能仅在AEM 6.3之后可用。

>[!NOTE]
>
>**要在发布实例上配置reCaptcha，请按照以下步骤操作**
>
>在创作实例上配置reCaptach
>
>在创作实 [例上打开](http://localhost:4502/system/console/bundles) felix web控制台
>
>搜索com.adobe.granite.crypto.file包
>
>请记下捆绑id。 我的例子是20
>
>在创作实例上导航到文件系统上的bundle id
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 复制HMAC和主控文件

在您的 [发布实例上](http://localhost:4502/system/console/bundles) ，打开Felix Web控制台。 搜索com.adobe.granite.crypto.file捆绑。 请注意捆绑id
导航到发布实例的文件系统上的bundle id
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 删除现有的HMAC和主控文件。
* 粘贴从创作实例复制的HMAC和主控文件

重新启动AEM发布服务器

## 支持材料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

