---
title: 设置Adobe项目Firefly以实现Asset compute可扩展性
description: asset compute项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发人员控制台中的Adobe项目Firefly才能设置和部署它们。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---

# 设置Adobe项目Firefly

asset compute项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发人员控制台中的Adobe项目Firefly才能设置和部署它们。

## 在Adobe开发人员控制台中创建和设置Adobe项目Firefly{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_设置Adobe项目Firefly的点进（无音频）_

1. 使用与已配置的[帐户和服务](./accounts-and-services.md)关联的Adobe ID登录到[Adobe开发人员控制台](https://console.adobe.io)。 确保您是&#x200B;__系统管理员__，或在&#x200B;__开发人员角色__&#x200B;中，获得正确的Adobe组织。
1. 通过点按&#x200B;__新建项目>模板中的项目>项目Firefly__&#x200B;创建Firefly项目

   _如果“创__&#x200B;建新项&#x200B;__目”按钮或“项__&#x200B;目&#x200B;__”字段不可用，则表示您的Adobe组织未配 [置“项目Firefly”](#request-adobe-project-firefly)。_

   + __项目标题__:  `WKND AEM Asset Compute`
   + __应用程序名称__:  `wkndAemAssetCompute<YourName>`
      + __应用程序名称__&#x200B;在所有Firefly项目中必须是唯一的，以后不能修改。 在公司或组织的名称前添加前缀，并在后缀中添加有意义的后缀，是一种好的方法，例如：`wkndAemAssetCompute`。
      + 要进行自我启用，通常最好将您的名称发布到&#x200B;__应用程序名称__，例如`wkndAemAssetComputeJaneDoe`，以避免与其他项目Firefly项目发生冲突。
   + 在&#x200B;__Workspaces__&#x200B;下，添加一个名为`Development`的新环境
   + 在&#x200B;__Adobe I/O Runtime__&#x200B;下，确保选中了&#x200B;__包含运行时和每个工作区__
   + 点按&#x200B;__保存__&#x200B;以保存项目
1. 在AdobeFirefly项目中，从工作区选择器中选择`Development`
1. 点按&#x200B;__+添加服务> API__&#x200B;以打开&#x200B;__添加API__&#x200B;向导，使用此方法添加以下API:

   + __Experience Cloud>Asset compute__
      + 选择&#x200B;__生成键对__&#x200B;并点按&#x200B;__生成键对__&#x200B;按钮，然后将下载的`config.zip`保存到安全位置，以供以后使用](#private-key)[
      + 点按&#x200B;__Next__
      + 选择产品配置文件&#x200B;__集成 — Cloud Service__，然后点按&#x200B;__保存配置的API__
   + __Adobe Services > I/O事件，然__ 后点按保 __存配置的API__
   + __Adobe服务> I/O管理API，然__ 后点按保 __存配置的API__

## 访问private.key{#private-key}

在设置[Asset computeAPI集成](#set-up)时，会生成一个新的密钥对，并自动下载`config.zip`文件。 此`config.zip`包含生成的公共证书和与`private.key`匹配的文件。

1. 将`config.zip`解压缩到文件系统上的安全位置，因为`private.key`是[稍后使用](../develop/environment-variables.md)
   + 绝不应将密钥和私钥作为安全事项添加到Git中。

## 查看服务帐户(JWT)凭据

本地[Adobe I/O开发工具](../develop/development-tool.md)使用此Asset compute项目的凭据与Adobe I/O Runtime进行交互，需要将其纳入到Asset compute项目中。 熟悉服务帐户(JWT)凭据。

![Adobe开发人员服务帐户凭据](./assets/firefly/service-account.png)

1. 在Adobe I/O项目Firefly项目中，确保选择`Development`工作区
1. 点按&#x200B;__凭据__&#x200B;下的&#x200B;__服务帐户(JWT)__
1. 查看显示的Adobe I/O凭据
   + 底部列出的&#x200B;__公钥__&#x200B;与在将&#x200B;__Asset computeAPI__&#x200B;添加到此项目时下载的`config.zip`中的&#x200B;__private.key__&#x200B;对应。
      + 如果私钥丢失或受损，则可以删除匹配的公钥，并使用此界面在中生成新的密钥对或将其上传到Adobe I/O。
