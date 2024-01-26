---
title: 为Asset compute可扩展性创建Asset compute项目
description: asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合特定结构，允许将它们部署到Adobe I/O Runtime并与AEMas a Cloud Service集成。
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 212
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# 创建Asset compute项目

asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合特定结构，允许将它们部署到Adobe I/O Runtime并与AEMas a Cloud Service集成。 单个Asset compute项目可以包含一个或多个Asset compute工作程序，每个工作程序都有一个可从AEMas a Cloud Service处理配置文件引用的离散HTTP端点。

## 生成项目

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_生成Asset compute项目的点进（无音频）_

使用 [Adobe I/OCLIAsset compute插件](../set-up/development-environment.md#aio-cli) 以生成新的空Asset compute项目。

1. 从命令行中，导航到包含项目的文件夹。
1. 从命令行中，执行 `aio app init` 开始交互式项目生成CLI。
   + 此命令可能会派生一个Web浏览器，提示进行Adobe I/O验证。如果是，请提供与关联的Adobe凭据 [必需的Adobe服务和产品](../set-up/accounts-and-services.md). 如果您无法登录，请关注 [以下有关如何生成项目的说明](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __选择组织__
   + 选择具有AEMas a Cloud Service的Adobe组织，App Builder将在其中注册
1. __选择项目__
   + 找到并选择项目。 这是 [项目标题](../set-up/app-builder.md) 在本例中，是从App Builder项目模板创建的 `WKND AEM Asset Compute`
1. __选择工作区__
   + 选择 `Development` 工作区
1. __您要为此项目启用哪些Adobe I/O应用程序功能？ 选择要包含的组件__
   + 选择 `Actions: Deploy runtime actions`
   + 使用箭头键进行选择，空格键取消选择/选择，Enter键确认选择
1. __选择要生成的操作类型__
   + 选择 `DX Asset Compute Worker v1`
   + 使用箭头键选择，空格键取消选择/选择，Enter键确认选择
1. __您希望如何命名此操作？__
   + 使用默认名称 `worker`.
   + 如果项目包含多个执行不同资产计算的辅助进程，请以语义方式命名它们

## 生成console.json

开发人员工具需要一个名为的文件 `console.json` 包含连接到Adobe I/O所需的凭据。此文件将从Adobe I/O控制台下载。

1. 打开Asset compute工作人员的 [Adobe I/O](https://console.adobe.io) 项目
1. 选择要下载的项目工作区 `console.json` 的凭据，在本例中，选择 `Development`
1. 转到Adobe I/O项目的根并点击 __全部下载__ 在右上角。
1. 文件下载为 `.json` 前缀为项目和工作区的文件，例如： `wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 将文件重命名为 `console.json` 并将其移动到Asset compute工作人员项目的根目录下。 这就是本教程中的方法。
   + 将其移动到任意文件夹并从您的中引用该文件夹 `.env` 包含配置条目的文件 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. 文件路径可以是绝对路径，也可以是相对于项目根目录的路径。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     或者
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> 注意
> 该文件包含凭据。 如果将该文件存储在您的项目中，请确保将其添加到您的 `.gitignore` 阻止共享的文件。 这同样适用于 `.env` 文件 — 这些凭据文件不得共享或存储在Git中。

## 在GitHub上Asset compute项目

GitHub上提供了最终Asset compute项目，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含项目的最终状态，其中已完全填充工作程序和测试案例，但不包含任何凭据，即 `.env`， `console.json` 或 `.aio`._
