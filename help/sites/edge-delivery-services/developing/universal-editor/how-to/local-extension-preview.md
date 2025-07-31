---
title: 预览Universal Editor扩展
description: 了解如何在开发期间预览本地运行的通用编辑器扩展。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner, Intermediate, Experienced
doc-type: Tutorial
jira: KT-18658
source-git-commit: f0ad5d66549970337118220156d7a6b0fd30fd57
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---


# 预览本地通用编辑器扩展

>[!TIP]
> 了解如何[创建通用编辑器扩展](https://developer.adobe.com/uix/docs/services/aem-universal-editor/)。

要在开发过程中预览通用编辑器扩展，您需要：

1. 在本地运行扩展。
2. 接受自签名证书。
3. 在通用编辑器中打开页面。
4. 更新位置URL以加载本地扩展。

## 在本地运行扩展

这假定您已创建了一个[Universal Editor扩展](https://developer.adobe.com/uix/docs/services/aem-universal-editor/)，并希望在本地测试和开发时预览该扩展。

使用以下内容启动Universal Editor扩展：

```bash
$ aio app run
```

您将看到如下输出：

```
To view your local application:
  -> https://localhost:9080
To view your deployed application in the Experience Cloud shell:
  -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
```

默认情况下，这将在`https://localhost:9080`运行您的扩展。


## 接受自签名证书

通用编辑器需要HTTPS才能加载扩展。 由于本地开发使用自签名证书，因此您的浏览器必须明确信任该证书。

打开新的浏览器选项卡，然后通过`aio app run`命令导航到本地扩展URL输出：

```
https://localhost:9080
```

您的浏览器将显示证书警告。 接受证书以继续。

![接受自签名证书](./assets/local-extension-preview/accept-certificate.png)

接受后，您将看到本地扩展的占位符页面：

![扩展可访问](./assets/local-extension-preview/extension-accessible.png)


## 在通用编辑器中打开页面

通过[通用编辑器控制台](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/)打开通用编辑器，或者在AEM Sites中编辑使用通用编辑器的页面：

![在通用编辑器中打开页面](./assets/local-extension-preview/open-page-in-ue.png)


## 加载扩展

在Universal Editor中，在界面的顶部中央找到&#x200B;**位置**&#x200B;字段。 展开并更新位置字段&#x200B;**中的** URL，**不是浏览器地址栏**。

附加以下查询参数：

* `devMode=true` — 启用通用编辑器的开发模式。
* `ext=https://localhost:9080` — 加载本地运行的扩展。

示例：

```
https://author-pXXX-eXXX.adobeaemcloud.com/content/aem-ue-wknd/index.html?devMode=true&ext=https://localhost:9080
```

![更新通用编辑器位置URL](./assets/local-extension-preview/update-location-url.png)


## 预览扩展

对浏览器执行&#x200B;**硬重新加载**&#x200B;以确保使用更新的URL。

Universal Editor现在将加载您的本地扩展 — 仅在浏览器会话中加载。

您在本地所做的任何代码更改都将立即反映出来。

![本地扩展已加载](./assets/local-extension-preview/extension-loaded.png)

