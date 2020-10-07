---
title: 为资产计算可扩展性设置Adobe项目Firefly
description: 资产计算项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发者控制台中的Adobe项目Firefly才能设置和部署它们。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# 设置Adobe项目Firefly

资产计算项目是特别定义的Adobe项目Firefly项目，因此，需要访问Adobe开发者控制台中的Adobe项目Firefly才能设置和部署它们。

## 在Adobe开发人员控制台中创建和设置Adobe项目Firefly{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)
_设置Adobe项目Firefly的点进（无音频）_

1. 使用与所配 [置的帐户和服务](https://console.adobe.io) 关联的Adobe ID登录 [到Adobe开发人员](./accounts-and-services.md)控制台。 确保您是系 __统管理员__ ，或是正 __确的Adobe组__ 织的开发人员。
1. 通过点按“新建项目”>“ __模板中的项目”>“项目Firefly”，创建Firefly项目__

   _如果“__&#x200B;创建新项目&#x200B;__”按钮或“__&#x200B;项目Firefly __”类型不可用，则表示您的Adobe组织未配[置Project Firefly](#request-adobe-project-firefly)。_

   + __项目标题__: `WKND AEM Asset Compute`
   + __应用程序名称__: `wkndAemAssetCompute<YourName>`
      + 应用 __程序名称__ 在所有Firefly项目中必须唯一，以后不可修改。 为公司或组织的名称加上前缀后添加有意义的后缀是一种不错的方法，例如： `wkndAemAssetCompute`.
      + 为了实现自我启用，最好将您的姓名后缀 __到应用程序名__，如 `wkndAemAssetComputeJaneDoe` 避免与其他Project Firefly项目发生冲突。
   + 在“工 __作区__ ”下，添加一个名为 `Development`
   + 在 __Adobe I/O Runtime____下__ ，确保选择“包含运行时”(Include Runtime with each workspace)
   + 点按 __保存__ ，以保存项目
1. 在AdobeFirefly项目中，从工作区 `Development` 选择器中进行选择
1. 点 __按+添加服务__ > API以打 __开添加API向导__ ，使用此方法添加以下API:

   + __Experience Cloud>资产计算__
      + 选 __择“生成密钥对__ ”并点按“ __生成密钥对__ ”按钮，将下载的内容保 `config.zip` 存到安全位置供 [以后使用](#private-key)
      + 点按下 __一步__
      + 选择产品用户档案集 __成-Cloud Service__ ，然 __后点按保存配置的API__
   + __Adobe服务> I/O事件__ ，然 __后点按保存配置的API__
   + __Adobe服务> I/O管理API__ ，然后 __点按保存配置API__

## 访问private.key{#private-key}

设置Asset Compute API [集成时](#set-up) ，将生成新的密钥对，并 `config.zip` 自动下载文件。 它包 `config.zip` 含生成的公共证书和匹配 `private.key` 文件。

1. 解压缩 `config.zip` 到文件系统上的安全位置，以 `private.key` 后 [使用](../develop/environment-variables.md)
   + 机密和私钥永远不应作为安全问题添加到Git。

## 查看服务帐户(JWT)凭据

本地Asset Compute Development Tool使用此AdobeI/ [O项目的凭据与Adobe I/O Runtime](../develop/development-tool.md) ，并且需要将其并入Asset Compute项目。 熟悉服务帐户(JWT)凭据。

![Adobe开发人员服务帐户凭据](./assets/firefly/service-account.png)

1. 从AdobeI/O项目Firefly项目中，确保选 `Development` 择工作区
1. 点按凭 __据下的服务帐户__ (JWT) __。__
1. 查看显示的AdobeI/O凭据
   + 底部 __列出__ 的公钥在将Asset Compute API添加到此项目时，其下 __载的密钥是private__`config.zip`____ .key对应项。
      + 如果私钥丢失或泄露，则可以删除匹配的公钥，并使用此接口在AdobeI/O中生成或上传新的密钥对。
