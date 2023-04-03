---
title: 设置App Builder以实现Asset compute可扩展性
description: asset compute项目是特别定义的应用程序生成器项目，因此，需要访问Adobe Developer控制台中的应用程序生成器，才能设置和部署这些项目。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# 设置应用程序生成器

asset compute项目是特别定义的应用程序生成器项目，因此，需要访问Adobe Developer控制台中的应用程序生成器，才能设置和部署这些项目。

## 在Adobe Developer控制台中创建和设置应用程序生成器{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_设置应用程序生成器的点进（无音频）_

1. 登录到 [Adobe Developer控制台](https://console.adobe.io) 使用与已配置的 [帐户和服务](./accounts-and-services.md). 确保您是 __系统管理员__ 或 __开发人员角色__ 的Adobe组织。
1. 通过点按 __通过模板>应用程序生成器创建新项目>项目__

   _如果__&#x200B;创建新项目&#x200B;__按钮或__&#x200B;应用程序生成器&#x200B;__类型不可用，这表示您的Adobe组织不可用 [已配置App Builder](#request-adobe-project-app-builder)._

   + __项目标题__: `WKND AEM Asset Compute`
   + __应用程序名称__: `wkndAemAssetCompute<YourName>`
      + 的 __应用程序名称__ 必须在所有FApp Builderrefly项目中是唯一的，且以后不能修改。 在公司或组织的名称前添加前缀，并在后缀中添加有意义的后缀，是一种好的方法，例如： `wkndAemAssetCompute`.
      + 要进行自我启用，通常最好将您的名称发布到 __应用程序名称__，例如 `wkndAemAssetComputeJaneDoe` 以避免与其他应用程序生成器项目发生冲突。
   + 在 __工作区__ 添加名为 `Development`
   + 在 __Adobe I/O Runtime__ 确保 __将运行时包含在每个工作区中__ 已选择
   + 点按 __保存__ 保存项目
1. 在应用程序生成器项目中，选择 `Development` 从工作区选择器
1. 点按 __+添加服务> API__ 打开 __添加API__ 向导，请使用此方法添加以下API:

   + __Experience Cloud>Asset compute__
      + 选择 __生成键对__ 并点按 __生成密钥对__ 按钮，并保存下载的内容 `config.zip` 安全地点 [稍后使用](#private-key)
      + 点按 __下一个__
      + 选择产品配置文件 __集成 — Cloud Service__ 点按 __保存配置的API__
   + __Adobe Services > I/O事件__ 点按 __保存配置的API__
   + __Adobe Services > I/O管理API__ 点按 __保存配置的API__

## 访问private.key{#private-key}

设置 [asset computeAPI集成](#set-up) 生成了新的密钥对，并且 `config.zip` 文件。 此 `config.zip` 包含生成的公共证书和匹配项 `private.key` 文件。

1. 解压缩 `config.zip` 将文件系统上的安全位置作为 `private.key` is [稍后使用](../develop/environment-variables.md)
   + 绝不应将密钥和私钥作为安全事项添加到Git中。

## 查看服务帐户(JWT)凭据

本地Adobe I/O使用此项目的凭据 [asset compute开发工具](../develop/development-tool.md) 与Adobe I/O Runtime进行交互，并且需要纳入Asset compute项目。 熟悉服务帐户(JWT)凭据。

![Adobe Developer服务帐户凭据](./assets/app-builder/service-account.png)

1. 从Adobe I/O项目应用程序生成器项目中，确保 `Development` 工作区被选中
1. 点按 __服务帐户(JWT)__ 在 __凭据__
1. 查看显示的Adobe I/O凭据
   + 的 __公钥__ 在底部列出的 __private.key__ 与 `config.zip` 下载时间 __asset computeAPI__ 已添加到此项目。
      + 如果私钥丢失或受损，则可以删除匹配的公钥，并使用此界面在中生成新的密钥对或将其上传到Adobe I/O。
