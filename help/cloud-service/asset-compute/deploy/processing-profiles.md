---
title: 将Asset compute工作程序与AEM处理配置文件集成
description: AEM as a Cloud Service通过AEM Assets处理配置文件与部署到Adobe I/O Runtime的Asset compute工作程序集成。 处理配置文件在创作服务中进行了配置，以使用自定义工作程序处理特定资产，并将工作程序生成的文件存储为资产演绎版。
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# 与AEM处理配置文件集成

为了使Asset compute工作程序在AEMas a Cloud Service中生成自定义演绎版，必须通过处理用户档案在AEMas a Cloud Service作者服务中进行注册。 受该处理配置文件约束的所有资产都将在上传或重新处理时调用工作程序，并通过资产的演绎版生成并提供自定义演绎版。

## 定义处理配置文件

首先，创建一个新的处理配置文件，该配置文件将使用可配置参数调用工作程序。

![处理配置文件](./assets/processing-profiles/new-processing-profile.png)

1. 登录AEMas a Cloud Service创作服务作为 __AEM管理员__. 由于这是一个教程，因此我们建议您使用开发环境或沙盒中的环境。
1. 导航到 __工具>资产>处理配置文件__
1. 点按 __创建__ 按钮
1. 命名处理配置文件， `WKND Asset Renditions`
1. 点按 __自定义__ 选项卡，然后点按 __新增__
1. 定义新服务
   + __演绎版名称：__ `Circle`
      + 用于在AEM Assets中标识此演绎版的演绎版的文件名
   + __扩展：__ `png`
      + 生成的演绎版的扩展。 设置为 `png` 因为这是工作人员web服务支持的支持输出格式，因此在圆切出后会产生透明背景。
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 这是通过获取的工作人员的URL `aio app get-url`. 根据AEMas a Cloud Service环境，确保URL指向正确的工作区。
      + 确保工作URL指向正确的工作区。 AEMas a Cloud ServiceStage应使用Stage工作区URL，而AEMas a Cloud Service生产应使用生产工作区URL。
   + __服务参数__
      + 点按 __添加参数__
         + 键: `size`
         + 值: `1000`
      + 点按 __添加参数__
         + 键: `contrast`
         + 值: `0.25`
      + 点按 __添加参数__
         + 键: `brightness`
         + 值: `0.10`
      + 这些键/值对将传递到Asset compute工作器，并可通过 `rendition.instructions` JavaScript对象。
   + __Mime 类型__
      + __包括：__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + 这些MIME类型是工作程序npm模块中的唯一类型。 此列表限制了自定义工作程序处理的内容。
      + __不包括：__ `Leave blank`
         + 切勿使用此服务配置处理具有这些MIME类型的资产。 在这种情况下，我们只使用允许列表。
1. 点按 __保存__ 在右上方

## 应用并调用处理配置文件

1. 选择新创建的处理配置文件， `WKND Asset Renditions`
1. 点按 __将配置文件应用到文件夹__ 在顶部操作栏中
1. 选择要将处理配置文件应用到的文件夹，例如 `WKND` 点按 __应用__
1. 导航到处理配置文件未通过应用到的文件夹 __AEM > Assets >文件__ 并点按 `WKND`.
1. 上传一些新图像资产([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg)和 [sample-3.jpg](../assets/samples/sample-3.jpg))，然后等待处理上传的资产。
1. 点按资产以打开其详细信息
   + 默认演绎版在AEM中的生成和显示速度可能比自定义演绎版快。
1. 打开 __演绎版__ 从左侧边栏查看
1. 点按名为的资产 `Circle.png` 并查看生成的演绎版

   ![生成的演绎版](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！您已完成 [教程](../overview.md) 关于如何扩展AEMas a Cloud ServiceAsset compute微服务！ 现在，您应该能够设置、开发、测试、调试和部署自定义Asset compute工作程序，以供AEMas a Cloud Service创作服务使用。

### 在Github上查看完整的项目源代码

最终Asset compute项目可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github包含是项目的最终状态，已完全填充工作程序和测试用例，但不包含任何凭据，即。 `.env`, `.config.json` 或 `.aio`._

## 疑难解答

+ [AEM资产中缺少自定义演绎版](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [资产处理在AEM中失败](../troubleshooting.md#asset-processing-fails)
