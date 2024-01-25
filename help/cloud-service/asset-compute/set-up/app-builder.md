---
title: 设置App Builder以实现Asset compute可扩展性
description: asset compute项目是特别定义的App Builder项目，因此，需要访问Adobe Developer控制台中的App Builder才能设置和部署这些项目。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 216
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# 设置应用程序生成器

asset compute项目是特别定义的App Builder项目，因此，需要访问Adobe Developer控制台中的App Builder才能设置和部署这些项目。

## 在Adobe Developer控制台中创建并设置应用程序生成器{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_设置App Builder的点进（无音频）_

1. 登录 [Adobe Developer控制台](https://console.adobe.io) 使用与配置的关联的Adobe ID [帐户和服务](./accounts-and-services.md). 确保您是 __系统管理员__ 或在 __开发人员角色__ 以获取正确的Adobe组织。
1. 通过点按创建App Builder项目 __通过模板> App Builder创建新项目>项目__

   _如果出现以下任一情况__&#x200B;创建新项目&#x200B;__按钮或__ App Builder __类型不可用，这意味着您的Adobe组织不可用 [已设置应用程序生成器](#request-adobe-project-app-builder)._

   + __项目标题__： `WKND AEM Asset Compute`
   + __应用程序名称__： `wkndAemAssetCompute<YourName>`
      + 此 __应用程序名称__ 在所有Builderirefly项目中必须是唯一的，并且以后不能修改。 在您的公司或组织的名称前添加前缀，并在后缀中使用有意义的后缀是一种好方法，例如： `wkndAemAssetCompute`.
      + 对于自我启用，通常最好将您的姓名后缀到 __应用程序名称__，例如 `wkndAemAssetComputeJaneDoe` 以避免与其他App Builder项目发生冲突。
   + 下 __工作区__ 添加名为的新环境 `Development`
   + 下 __Adobe I/O Runtime__ 确保 __包含每个工作区的运行时__ 已选定
   + 点按 __保存__ 以保存项目
1. 在App Builder项目中，选择 `Development` 从工作区选择器中
1. 点按 __+添加服务> API__ 以打开 __添加API__ 向导，请使用此方法添加以下API：

   + __Experience Cloud>Asset compute__
      + 选择 __生成密钥对__ 然后点击 __生成密钥对__ 按钮，并保存下载的 `config.zip` 到安全位置 [稍后使用](#private-key)
      + 点按 __下一个__
      + 选择产品配置文件 __集成 — Cloud Service__ 并点击 __保存配置的API__
   + __Adobe服务> I/O事件__ 并点击 __保存配置的API__
   + __Adobe服务> I/O管理API__ 并点击 __保存配置的API__

## 访问private.key{#private-key}

设置时 [asset computeAPI集成](#set-up) 生成了一个新的密钥对，并且 `config.zip` 文件已自动下载。 此 `config.zip` 包含生成的公共证书和匹配 `private.key` 文件。

1. 解压缩 `config.zip` 到文件系统的安全位置，作为 `private.key` 是 [稍后使用](../develop/environment-variables.md)
   + 绝不应该出于安全原因将密钥和私钥添加到Git中。

## 查看服务帐户(JWT)凭据

此Adobe I/O项目的凭据由本地使用 [asset compute开发工具](../develop/development-tool.md) 要与Adobe I/O Runtime交互，则需要将和纳入Asset compute项目。 熟悉“服务帐户” (JWT)凭证。

![Adobe Developer服务帐户凭据](./assets/app-builder/service-account.png)

1. 在Adobe I/O项目App Builder项目中，确保 `Development` 已选择工作区
1. 点击 __服务帐户(JWT)__ 下 __凭据__
1. 查看显示的Adobe I/O身份证明
   + 此 __公钥__ 在底部列出 __私钥__ 中的对应项 `config.zip` 下载时间 __ASSET COMPUTEAPI__ 已添加到此项目。
      + 如果私钥丢失或泄漏，可以删除匹配的公钥，并使用此界面在中生成或上传到Adobe I/O的新密钥对。
