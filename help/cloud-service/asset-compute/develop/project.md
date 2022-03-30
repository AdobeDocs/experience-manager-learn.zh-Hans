---
title: 创建Asset compute项目以实现Asset compute可扩展性
description: asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合特定结构，允许将这些项目部署到Adobe I/O Runtime并与AEMas a Cloud Service集成。
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# 创建Asset compute项目

asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合允许将这些项目部署到Adobe I/O Runtime并与AEM as a Cloud Service集成的特定结构。 单个Asset compute项目可以包含一个或多个Asset compute工作程序，每个工作程序都具有可从AEMas a Cloud Service处理配置文件引用的离散HTTP端点。

## 生成项目

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_点进以生成Asset compute项目（无音频）_

使用 [Adobe I/OCLIAsset compute插件](../set-up/development-environment.md#aio-cli) 生成新的空Asset compute项目。

1. 在命令行中，导航到要包含项目的文件夹。
1. 从命令行中执行 `aio app init` 开始交互式项目生成CLI。
   + 此命令可能会生成Web浏览器，提示进行身份验证以Adobe I/O。如果存在，请提供与 [所需的Adobe服务和产品](../set-up/accounts-and-services.md). 如果您无法登录，请遵循 [有关如何生成项目的说明](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __选择组织__
   + 选择具有AEMas a Cloud Service且已在中注册应用程序生成器的Adobe组织
1. __选择项目__
   + 找到并选择项目。 这是 [项目标题](../set-up/app-builder.md) 从应用程序生成器项目模板创建，在此例中为 `WKND AEM Asset Compute`
1. __选择工作区__
   + 选择 `Development` 工作区
1. __您希望为此项目启用哪些Adobe I/O应用程序功能？ 选择要包含的组件__
   + 选择 `Actions: Deploy runtime actions`
   + 使用箭头键选择和空格键取消选择/选择，然后按Enter键确认选择
1. __选择要生成的操作类型__
   + 选择 `DX Asset Compute Worker v1`
   + 使用箭头键进行选择，空格键进行取消选择/选择，Enter键进行确认选择
1. __要如何命名此操作？__
   + 使用默认名称 `worker`.
   + 如果您的项目包含多个执行不同资产计算的工作程序，请在语义上为它们命名

## 生成console.json

开发人员工具需要一个名为 `console.json` 包含连接到Adobe I/O所需的凭据。此文件将从Adobe I/O控制台下载。

1. 打开Asset compute工的 [Adobe I/O](https://console.adobe.io) 项目
1. 选择项目工作区以下载 `console.json` 的凭据，在此例中，选择 `Development`
1. 转到Adobe I/O项目的根，然后点按 __全部下载__ 中。
1. 文件将下载为 `.json` 带有项目和工作区前缀的文件，例如： `wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 将文件重命名为 `config.json` 并将其移到Asset compute工作项目的根中。 这是本教程中的方法。
   + 将其移入任意文件夹，并从 `.env` 具有配置项的文件 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. 文件路径可以是绝对路径，也可以是相对于项目根路径的绝对路径。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或者
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 文件包含凭据。 如果将文件存储在项目中，请确保将其添加到 `.gitignore` 文件来阻止共享。 这同样适用于 `.env` 文件 — 这些凭据文件不得共享或存储在Git中。

## asset computeGitHub上的项目

最终Asset compute项目可在GitHub上获取，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含项目的最终状态，已完全填充工作程序和测试案例，但不包含任何凭据，即 `.env`, `console.json` 或 `.aio`._
