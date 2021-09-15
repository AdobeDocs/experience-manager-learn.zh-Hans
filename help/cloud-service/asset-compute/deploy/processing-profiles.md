---
title: 将Asset compute工作程序与AEM处理配置文件集成
description: AEM as aCloud Service通过AEM Assets处理用户档案与部署到Adobe I/O Runtime的Asset compute工作程序集成。 处理配置文件在创作服务中进行了配置，以使用自定义工作程序处理特定资产，并将工作程序生成的文件存储为资产演绎版。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 2%

---

# 与AEM处理配置文件集成

为了使Asset compute工作程序在AEM as a Cloud Service中生成自定义演绎版，必须通过处理用户档案在AEM中注册为Cloud Service作者服务。 受该处理配置文件约束的所有资产都将在上传或重新处理时调用工作程序，并通过资产的演绎版生成并提供自定义演绎版。

## 定义处理配置文件

首先，创建一个新的处理配置文件，该配置文件将使用可配置参数调用工作程序。

![处理配置文件](./assets/processing-profiles/new-processing-profile.png)

1. 以&#x200B;__AEM Administrator__&#x200B;的身份登录AEM as a Cloud Service创作服务。 由于这是一个教程，因此我们建议您使用开发环境或沙盒中的环境。
1. 导航到&#x200B;__工具>资产>处理配置文件__
1. 点按&#x200B;__创建__&#x200B;按钮
1. 将处理配置文件命名为`WKND Asset Renditions`
1. 点按&#x200B;__Custom__&#x200B;选项卡，然后点按&#x200B;__Add New__
1. 定义新服务
   + __演绎版名称：__ `Circle`
      + 用于在AEM Assets中标识此演绎版的文件名演绎版
   + __扩展：__ `png`
      + 将生成的演绎版的扩展。 设置为`png`，因为这是工作人员Web服务支持的支持输出格式，并导致在圆切出后出现透明背景。
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 这是通过`aio app get-url`获取的工作器的URL。 根据AEM as a Cloud Service环境，确保URL指向正确的工作区。
      + 确保工作URL指向正确的工作区。 AEM as a Production Stage应使用Stage工作区URL， AEM as a Production应使用Production Workspace URL。
   + __服务参数__
      + 点按&#x200B;__添加参数__
         + 键: `size`
         + 值: `1000`
      + 点按&#x200B;__添加参数__
         + 键: `contrast`
         + 值: `0.25`
      + 点按&#x200B;__添加参数__
         + 键: `brightness`
         + 值: `0.10`
      + 这些键/值对将传递到Asset compute工作器，并可通过`rendition.instructions` JavaScript对象使用。
   + __Mime 类型__
      + __包括：__ `image/jpeg`、 `image/png`、 `image/gif`、 `image/bmp`、  `image/tiff`
         + 这些MIME类型是工作程序npm模块中的唯一类型。 此列表限制自定义工作程序将处理哪些资产。
      + __不包括：__ `Leave blank`
         + 切勿使用此服务配置处理具有这些MIME类型的资产。 在这种情况下，我们只使用允许列表。
1. 点按右上方的&#x200B;__Save__

## 应用并调用处理配置文件

1. 选择新创建的处理配置文件`WKND Asset Renditions`
1. 点按顶部操作栏中的&#x200B;__将配置文件应用到文件夹__
1. 选择要将处理配置文件应用到的文件夹，如`WKND`，然后点按&#x200B;__应用__
1. 通过&#x200B;__AEM > Assets > Files__&#x200B;导航到处理配置文件未应用到的文件夹，然后点按`WKND`。
1. 在应用了处理配置文件的文件夹下的任意文件夹中，上传一些新的图像资产（[sample-1.jpg](../assets/samples/sample-1.jpg)、[sample-2.jpg](../assets/samples/sample-2.jpg)和[sample-3.jpg](../assets/samples/sample-3.jpg)），然后等待处理上传的资产。
1. 点按资产以打开其详细信息
   + 默认演绎版在AEM中的生成和显示速度可能比自定义演绎版快。
1. 从左侧边栏中打开&#x200B;__演绎版__&#x200B;视图
1. 点按名为`Circle.png`的资产，然后查看生成的演绎版

   ![生成的演绎版](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！ 您已完成关于如何将AEM扩展为Cloud ServiceAsset compute微服务的[教程](../overview.md)! 现在，您应该能够设置、开发、测试、调试和部署自定义Asset compute工作程序，以供AEM作为Cloud Service创作服务使用。

### 在Github上查看完整的项目源代码

最终Asset compute项目可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github包含是项目的最终状态，已完全填充工作程序和测试用例，但不包含任何凭据，即。`.env`, `.config.json` 或 `.aio`._

## 疑难解答

+ [AEM资产中缺少自定义演绎版](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [资产处理在AEM中失败](../troubleshooting.md#asset-processing-fails)
