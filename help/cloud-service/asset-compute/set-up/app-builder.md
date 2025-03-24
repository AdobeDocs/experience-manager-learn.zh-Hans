---
title: 为Asset Compute可扩展性设置App Builder
description: Asset Compute项目是特别定义的App Builder项目，因此，需要访问Adobe Developer Console中的App Builder才能设置和部署这些项目。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# 设置App Builder

Asset Compute项目是特别定义的App Builder项目，因此，需要访问Adobe Developer Console中的App Builder才能设置和部署这些项目。

## 在Adobe Developer Console中创建和设置App Builder{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_设置App Builder的点进（无音频）_

1. 使用与已设置的[帐户和服务](./accounts-and-services.md)关联的Adobe ID登录到[Adobe Developer Console](https://console.adobe.io)。 确保您是&#x200B;__系统管理员__&#x200B;或处于正确Adobe组织的&#x200B;__开发人员角色__&#x200B;中。
1. 通过点按&#x200B;__新建项目>从模板创建项目> App Builder__&#x200B;创建App Builder项目

   _如果__&#x200B;新建项目&#x200B;__按钮或__ App Builder __类型不可用，这意味着您的Adobe组织未[配置了App Builder](#request-adobe-project-app-builder)。_

   + __项目标题__： `WKND AEM Asset Compute`
   + __应用程序名称__： `wkndAemAssetCompute<YourName>`
      + __应用程序名称__&#x200B;在所有FApp Builderirefly项目中必须是唯一的，并且以后不能修改。 在公司或组织的名称前面加上前缀，并在后缀中加上有意义的后缀是一种好方法，例如： `wkndAemAssetCompute`。
      + 对于自我启用，通常最好将您的名称后缀到&#x200B;__应用程序名称__，如`wkndAemAssetComputeJaneDoe`，以避免与其他App Builder项目发生冲突。
   + 在&#x200B;__工作区__&#x200B;下，添加名为`Development`的新环境
   + 在&#x200B;__Adobe I/O Runtime__&#x200B;下，确保选中&#x200B;__包含每个工作区的运行时__
   + 点按&#x200B;__保存__&#x200B;以保存项目
1. 在App Builder项目中，从工作区选择器中选择`Development`
1. 点按&#x200B;__+添加服务> API__&#x200B;以打开&#x200B;__添加API__&#x200B;向导，使用此方法添加以下API：

   + __Experience Cloud > Asset Compute__
      + 选择&#x200B;__生成密钥对__&#x200B;并点按&#x200B;__生成密钥对__&#x200B;按钮，然后将下载的`config.zip`保存到安全位置以供[以后使用](#private-key)
      + 点按&#x200B;__下一步__
      + 选择产品配置文件&#x200B;__集成 — Cloud Service__，然后点按&#x200B;__保存配置的API__
   + __Adobe服务> I/O事件__&#x200B;并点按&#x200B;__保存配置的API__
   + __Adobe服务> I/O管理API__，然后点按&#x200B;__保存配置的API__

## 访问private.key{#private-key}

在设置[Asset Compute API集成](#set-up)时，生成了一个新的密钥对，并自动下载了`config.zip`文件。 此`config.zip`包含生成的公共证书和匹配的`private.key`文件。

1. 将`config.zip`解压缩到文件系统中的安全位置，因为`private.key`稍后将被[使用](../develop/environment-variables.md)
   + 绝不应该出于安全原因将密钥和私钥添加到Git中。

## 查看服务帐户(JWT)凭据

此Adobe I/O项目的凭据由本地[Asset Compute开发工具](../develop/development-tool.md)用于与Adobe I/O Runtime交互，需要将其纳入Asset Compute项目。 熟悉“服务帐户” (JWT)凭证。

![Adobe Developer服务帐户凭据](./assets/app-builder/service-account.png)

1. 从Adobe I/O项目App Builder项目中，确保选择`Development`工作区
1. 点按&#x200B;__凭据__&#x200B;下的&#x200B;__服务帐户(JWT)__
1. 查看显示的Adobe I/O凭据
   + 在底部列出的&#x200B;__公钥__&#x200B;具有在将&#x200B;__Asset Compute API__&#x200B;添加到此项目时下载的`config.zip`中的&#x200B;__private.key__&#x200B;对应项。
      + 如果私钥丢失或受损，可以删除匹配的公钥，并使用此界面在Adobe I/O中生成或上传新的密钥对。
