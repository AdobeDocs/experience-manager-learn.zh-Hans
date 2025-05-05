---
title: 在AEM自适应Forms中使用验证码
description: 在AEM自适应Forms中添加和使用验证码。
feature: Adaptive Forms,Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 在AEM自适应Forms中使用验证码{#using-captchas-with-aem-adaptive-forms}

在AEM自适应Forms中添加和使用验证码。

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*本视频介绍如何使用内置的AEM CAPTCHA服务和AEM的reCAPTCHA服务将验证码添加到Google自适应表单的过程。*

>[!NOTE]
>
>此功能仅在AEM 6.3之后可用。

>[!NOTE]
>
>**要在发布实例上配置reCaptcha，请按照以下步骤操作**
>
>在创作实例上配置reCaptach
>
>在创作实例上打开Felix [Web控制台](http://localhost:4502/system/console/bundles)
>
>搜索com.adobe.granite.crypto.file包
>
>记下捆绑包ID。 在我的实例上，此值为20
>
>导航到创作实例上文件系统上的捆绑包ID
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* 复制HMAC和主文件
>
>在发布实例上打开[felix Web控制台](http://localhost:4502/system/console/bundles)。 搜索com.adobe.granite.crypto.file包。 请注意捆绑包ID
>
>导航到发布实例的文件系统中的捆绑包ID
>
>* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* 删除现有的HMAC和主文件。
>* 粘贴从创作实例复制的HMAC和主文件
>
>重新启动AEM发布服务器

## 支持材料 {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
