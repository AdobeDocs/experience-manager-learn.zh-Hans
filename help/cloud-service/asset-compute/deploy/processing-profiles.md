---
title: 将资产计算工作者与AEM处理用户档案集成
description: AEM作为Cloud Service，通过AEM Assets处理用户档案与部署到Adobe I/O Runtime的资产计算工作人员集成。 处理用户档案在作者服务中进行了配置，以使用自定义工作线程处理特定资产，并将工作线程生成的文件存储为资产演绎版。
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

要使资产计算工作器在AEM中作为Cloud Service生成自定义演绎版，必须通过处理用户档案在AEM中将其注册为Cloud Service作者服务。 所有受此处理用户档案约束的资产将在上传或重新处理时调用该工作人员，并通过资产的演绎版生成并提供自定义演绎版。

## 定义处理用户档案

首先创建一个新的处理用户档案，它将使用可配置的参数调用工作器。

![处理用户档案](./assets/processing-profiles/new-processing-profile.png)

1. 以AEM管理员身份以Cloud Service作者服务的身份登 __录AEM__。 因为这是教程，我们建议在沙箱中使用开发环境或环境。
1. 导航到工 __具>资产>处理用户档案__
1. 点按 __创建__ 按钮
1. 命名处理用户档案, `WKND Asset Renditions`
1. 点按自定 __义选__ 项卡，然后点 __按添加新__
1. 定义新服务
   + __再现名称：__ `Circle`
      + 用于在AEM Assets标识此再现的文件名再现
   + __扩展：__ `png`
      + 将生成的再现的扩展。 设置为 `png` 因为这是工作者的Web服务支持的支持的输出格式，并在圆形剪切后面产生透明背景。
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 这是通过获取的工作者的URL `aio app get-url`。 根据AEM作为Cloud Service环境，确保URL指向正确的工作区。
      + 确保工作器URL指向正确的工作区。 AEM作为Cloud Service级，应使用舞台工作区URL，而AEM作为Cloud Service生产应使用生产工作区URL。
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
      + 这些键／值对会传递到资产计算工作器，并可通过JavaScript对 `rendition.instructions` 象使用。
   + __Mime 类型__
      + __包括：__`image/jpeg`, `image/png`, `image/gif`, `image/bmp``image/tiff`
         + 这些MIME类型是工作者npm模块中唯一的类型。 此列表限制自定义工作者将处理哪些资产。
      + __不包括：__ `Leave blank`
         + 切勿使用此服务配置使用这些MIME类型处理资产。 在这种情况下，我们只使用允许列表。
1. 点按 __右上__ 方的“保存”

## 应用和调用处理用户档案

1. 选择新建的处理用户档案, `WKND Asset Renditions`
1. 点 __按顶部操作栏中的__ “将用户档案应用到文件夹”
1. 选择要将处理用户档案应用到的文件夹，如并点 `WKND` 按应 __用__
1. 导航到未通过AEM >资产>文件应用处理用户档案 __的文件夹__ ，然后点按 `WKND`。
1. 在应用了处理[用户档案后，上传文件夹下任意文件夹中的某些新图像资产(](../assets/samples/sample-1.jpg)sample-1.jpg [、](../assets/samples/sample-2.jpg)sample-2.jpg和 [sample-3.jpg](../assets/samples/sample-3.jpg))，然后等待上传的资产得到处理。
1. 点按资产以打开其详细信息
   + 与自定义演绎版相比，默认演绎版在AEM中生成和显示速度更快。
1. 从左侧提 __要栏打开__ “演绎版”视图
1. 点按名为的资产并 `Circle.png` 复查生成的演绎版

   ![生成的演绎版](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！ 您已完成了如 [何将AEM](../overview.md) 扩展为Cloud Service资产计算微服务的教程！ 您现在应该能够设置、开发、测试、调试和部署自定义资产计算工作器，以便AEM作为Cloud Service作者服务使用。

### 在Github上查看完整的项目源代码

Github上提供最终的资产计算项目：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是项目的最终状态，它完全填充了工作者和测试用例，但不包含任何凭据，如 `.env`、 `.config.json` 或 `.aio`。_

## 疑难解答

+ [AEM中资产中缺少自定义再现](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的资产处理失败](../troubleshooting.md#asset-processing-fails)
