---
title: 将CAPTCHA与AEM Adaptive Forms结合使用
description: 在AEM自适应Forms中添加和使用CAPTCHA。
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# 将CAPTCHA与AEM Adaptive Forms结合使用{#using-captchas-with-aem-adaptive-forms}

在AEM自适应Forms中添加和使用CAPTCHA。

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*此视频介绍了使用内置的AEM CAPTCHA服务以及Google reCAPTCHA服务将CAPTCHA添加到AEM自适应表单的过程。*

>[!NOTE]
>
>此功能仅在AEM 6.3之后可用。

>[!NOTE]
>
>**要在发布实例上配置reCaptcha，请按照以下步骤操作**
>
>在创作实例上配置reCaptach
>
>打开Felix [Web控制台](http://localhost:4502/system/console/bundles) 在创作实例上
>
>搜索com.adobe.granite.crypto.file包
>
>记下包ID。 就我来说是20
>
>导航到创作实例上文件系统上的包ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 复制HMAC和主控文件
>
打开 [felix web console](http://localhost:4502/system/console/bundles) 在发布实例上。 搜索com.adobe.granite.crypto.file包。 记下包ID
导航到发布实例文件系统上的包ID
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* 删除现有的HMAC和主控文件。
* 粘贴从创作实例复制的HMAC和主控文件
>
重新启动AEM发布服务器

## 辅助材料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
