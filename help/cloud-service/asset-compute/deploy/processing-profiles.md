---
title: 将Asset compute工作人员与AEM处理用户档案集成
description: AEM作为Cloud Service，与通过AEM Assets处理用户档案部署到Adobe I/O Runtime的Asset compute工作人员集成。 处理用户档案在作者服务中进行了配置，以使用自定义工作线程处理特定资产，并将工作线程生成的文件存储为资产演绎版。
feature: asset-compute, processing-profiles
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 1%

---


# 与AEM处理用户档案集成

要使Asset compute工作者在AEM中以Cloud Service形式生成自定义再现，必须通过处理用户档案在AEM中以Cloud Service作者服务形式进行注册。 所有受此处理用户档案约束的资产将在上传或重新处理时调用该工作人员，并通过资产的演绎版生成并提供自定义演绎版。

## 定义处理用户档案

首先创建一个新的处理用户档案，它将使用可配置的参数调用工作器。

![处理用户档案](./assets/processing-profiles/new-processing-profile.png)

1. 以&#x200B;__AEM管理员__&#x200B;身份以Cloud Service作者服务身份登录AEM。 因为这是教程，我们建议在沙箱中使用开发环境或环境。
1. 导航到&#x200B;__工具>资产>处理用户档案__
1. 点按&#x200B;__创建__&#x200B;按钮
1. 将处理用户档案命名为`WKND Asset Renditions`
1. 点按&#x200B;__Custom__&#x200B;选项卡，然后点按&#x200B;__添加新__
1. 定义新服务
   + __再现名称：__ `Circle`
      + 用于在AEM Assets标识此再现的文件名再现
   + __扩展：__ `png`
      + 将生成的再现的扩展。 设置为`png`，因为这是工作者的Web服务支持的支持输出格式，并导致在圆切出后产生透明背景。
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 这是通过`aio app get-url`获取的工作者的URL。 根据AEM作为Cloud Service环境，确保URL指向正确的工作区。
      + 确保工作器URL指向正确的工作区。 AEM作为Cloud Service级，应使用舞台工作区URL，而AEM作为Cloud Service生产应使用生产工作区URL。
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
      + 这些键／值对被传递到Asset compute工作器中，并可通过`rendition.instructions` JavaScript对象使用。
   + __Mime 类型__
      + __包括__ `image/jpeg`: `image/png`、 `image/gif`、 `image/bmp`、  `image/tiff`
         + 这些MIME类型是工作者npm模块中唯一的类型。 此列表限制自定义工作者将处理哪些资产。
      + __不包括：__ `Leave blank`
         + 切勿使用此服务配置使用这些MIME类型处理资产。 在这种情况下，我们只使用允许列表。
1. 点按右上方的&#x200B;__保存__

## 应用和调用处理用户档案

1. 选择新创建的处理用户档案`WKND Asset Renditions`
1. 点按顶部操作栏中的&#x200B;__将用户档案应用到文件夹__
1. 选择要将处理用户档案应用到的文件夹，如`WKND`，然后点按&#x200B;__应用__
1. 导航到未通过&#x200B;__AEM >资产>文件__&#x200B;应用处理用户档案的文件夹，然后点按`WKND`。
1. 在应用了处理用户档案的文件夹下的任意文件夹中上传一些新图像资产（[sample-1.jpg](../assets/samples/sample-1.jpg)、[sample-2.jpg](../assets/samples/sample-2.jpg)和[sample-3.jpg](../assets/samples/sample-3.jpg)），然后等待上传的资产得到处理。
1. 点按资产以打开其详细信息
   + 与自定义演绎版相比，默认演绎版在AEM中生成和显示速度更快。
1. 从左侧边栏打开&#x200B;__演绎版__&#x200B;视图
1. 点按名为`Circle.png`的资产，并查看生成的演绎版

   ![生成的演绎版](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！ 您已完成了有关如何将AEM扩展为Cloud ServiceAsset compute微服务的[教程](../overview.md)! 您现在应该能够设置、开发、测试、调试和部署自定义Asset compute工作器，以便AEM作为Cloud Service作者服务使用。

### 在Github上查看完整的项目源代码

Github上提供最终Asset compute项目：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是项目的最终状态，它完全填充了工作者和测试用例，但不包含任何凭据，如`.env`、 `.config.json` 或 `.aio`。_

## 疑难解答

+ [AEM中资产中缺少自定义再现](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的资产处理失败](../troubleshooting.md#asset-processing-fails)
