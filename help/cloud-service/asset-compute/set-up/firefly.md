---
title: 设置Adobe项目Firefly以实现Asset compute可扩展性
description: asset compute项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发者控制台中的Adobe项目Firefly才能设置和部署它们。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# 设置Adobe项目Firefly

asset compute项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发者控制台中的Adobe项目Firefly才能设置和部署它们。

## 在Adobe开发者控制台{#set-up}中创建和设置Adobe项目Firefly

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_设置Adobe项目Firefly的点进（无音频）_

1. 使用与所设置的[帐户和服务](./accounts-and-services.md)关联的Adobe ID登录到[Adobe开发者控制台](https://console.adobe.io)。 确保您是&#x200B;__系统管理员__&#x200B;或&#x200B;__开发人员角色__&#x200B;中正确的Adobe组织。
1. 通过点按&#x200B;__“新建项目”>“模板中的项目”>“项目Firefly__”，创建Firefly项目

   _如果“__&#x200B;创建新&#x200B;__项目”按钮或“项__&#x200B;目&#x200B;__类型”不可用，则表 [示您的Adobe组织未配](#request-adobe-project-firefly)置Project Firefly。_

   + __项目标题__:  `WKND AEM Asset Compute`
   + __应用程序名称__:  `wkndAemAssetCompute<YourName>`
      + __应用程序名称__&#x200B;在所有Firefly项目中必须是唯一的，以后不可修改。 为公司或组织的名称加上前缀后添加有意义的后缀是一种不错的方法，例如：`wkndAemAssetCompute`。
      + 为了自我启用，最好将您的名称后缀到&#x200B;__应用程序名称__，如`wkndAemAssetComputeJaneDoe`，以避免与其他Project Firefly项目发生冲突。
   + 在&#x200B;__Workspaces__&#x200B;下，添加一个名为`Development`的新环境
   + 在&#x200B;__Adobe I/O Runtime__&#x200B;下，确保选择“包含运行时”（每个工作区&#x200B;__）__
   + 点按&#x200B;__保存__&#x200B;以保存项目
1. 在AdobeFirefly项目中，从工作区选择器中选择`Development`
1. 点按&#x200B;__+添加服务> API__&#x200B;以打开&#x200B;__添加API__&#x200B;向导，使用此方法添加以下API:

   + __Experience Cloud>Asset compute__
      + 选择&#x200B;__生成密钥对__&#x200B;并点按&#x200B;__生成密钥对__&#x200B;按钮，将下载的`config.zip`保存到安全位置以供以后使用[](#private-key)
      + 点按&#x200B;__下一步__
      + 选择产品用户档案&#x200B;__集成-Cloud Service__&#x200B;并点按&#x200B;__保存配置的API__
   + __Adobe服务> I/O事__ 件并点 __按保存配置的API__
   + __Adobe服务> I/O管理__ API并点 __按保存配置API__

## 访问private.key{#private-key}

在设置[Asset computeAPI集成](#set-up)时，将生成新的密钥对并自动下载`config.zip`文件。 此`config.zip`包含生成的公共证书和匹配的`private.key`文件。

1. 将`config.zip`解压缩到文件系统上的安全位置，因为`private.key`是[稍后使用](../develop/environment-variables.md)
   + 机密和私钥永远不应作为安全问题添加到Git。

## 查看服务帐户(JWT)凭据

本Adobe I/O项目的证书由本地[Asset compute开发工具](../develop/development-tool.md)使用，以与Adobe I/O Runtime进行交互，需要将其纳入Asset compute项目。 熟悉服务帐户(JWT)凭据。

![Adobe开发人员服务帐户凭据](./assets/firefly/service-account.png)

1. 从Adobe I/OProject Firefly项目中，确保选择`Development`工作区
1. 点按&#x200B;__凭据__&#x200B;下的&#x200B;__服务帐户(JWT)__
1. 查看显示的Adobe I/O凭据
   + 底部列出的&#x200B;__公钥__&#x200B;在将&#x200B;__Asset computeAPI__&#x200B;添加到此项目时，其与下载的`config.zip`中的&#x200B;__private.key__&#x200B;对应。
      + 如果私钥丢失或泄露，则可以删除匹配的公钥，并使用此接口在Adobe I/O中生成或上传新的密钥对。
