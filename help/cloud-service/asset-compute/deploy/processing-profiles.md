---
title: 将Asset compute工作程序与AEM处理配置文件集成
description: AEMas a Cloud Service通过AEM Assets处理配置文件与部署到Adobe I/O Runtime的Asset compute工作程序集成。 处理配置文件在创作服务中配置为使用自定义工作进程处理特定资产，并将工作进程生成的文件存储为资产演绎版。
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

要让Asset compute工作进程在AEMas a Cloud Service中生成自定义演绎版，必须通过“处理配置文件”在AEMas a Cloud Service创作服务中注册它们。 受该处理配置文件约束的所有资产将在上传或重新处理时调用工作进程，并生成自定义演绎版，并通过资产的演绎版提供该演绎版。

## 定义处理配置文件

首先，创建新的处理配置文件，该配置文件将使用可配置的参数调用工作进程。

![处理配置文件](./assets/processing-profiles/new-processing-profile.png)

1. 以AEMas a Cloud Service作者服务的身份登录 __AEM管理员__. 由于这是一个教程，我们建议在沙盒中使用开发环境或环境。
1. 导航到 __工具>资产>处理配置文件__
1. 点按 __创建__ 按钮
1. 命名处理配置文件， `WKND Asset Renditions`
1. 点按 __自定义__ 选项卡，然后点按 __新增__
1. 定义新服务
   + __演绎版名称：__ `Circle`
      + 用于在AEM Assets中标识此演绎版的文件名
   + __扩展名：__ `png`
      + 生成的演绎版的扩展。 设置为 `png` 因为这是工作程序的Web服务支持的输出格式，并且会在切掉的圆圈后面产生透明背景。
   + __端点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 这是辅助进程的URL，获取自 `aio app get-url`. 根据AEMas a Cloud Service环境，确保URL指向正确的工作区。
      + 确保工作程序URL指向正确的工作区。 AEMas a Cloud Service暂存应使用暂存工作区URL，AEMas a Cloud Service生产应使用生产工作区URL。
   + __服务参数__
      + 点按 __添加参数__
         + 键: `size`
         + 价值: `1000`
      + 点按 __添加参数__
         + 键: `contrast`
         + 价值: `0.25`
      + 点按 __添加参数__
         + 键: `brightness`
         + 价值: `0.10`
      + 这些键/值对将传递到Asset compute工作进程并可通过 `rendition.instructions` javascript对象。
   + __Mime 类型__
      + __包括：__ `image/jpeg`， `image/png`， `image/gif`， `image/bmp`， `image/tiff`
         + 这些MIME类型是工作人员的npm模块中唯一的NPM类型。 此列表限制由自定义工作进程处理的工作。
      + __不包括：__ `Leave blank`
         + 使用此服务配置时，切勿处理具有这些MIME类型的资产。 在这种情况下，我们仅使用允许列表。
1. 点按 __保存__ 右上角

## 应用和调用处理配置文件

1. 选择新创建的处理配置文件， `WKND Asset Renditions`
1. 点按 __将配置文件应用到文件夹__ 在顶部操作栏中
1. 选择要应用处理配置文件的文件夹，例如 `WKND` 并点按 __应用__
1. 通过导航到未应用处理配置文件的文件夹 __AEM >资源>文件__ 并深入了解 `WKND`.
1. 上传一些新图像资产([sample-1.jpg](../assets/samples/sample-1.jpg)， [sample-2.jpg](../assets/samples/sample-2.jpg)、和 [sample-3.jpg](../assets/samples/sample-3.jpg))，并等待处理上传的资产。
1. 点按资产以打开其详细信息
   + 与自定义演绎版相比，默认演绎版可在AEM中更快地生成和显示。
1. 打开 __演绎版__ 从左侧边栏查看
1. 点按名为的资产 `Circle.png` 并查看生成的演绎版

   ![生成的演绎版](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！您已完成 [教程](../overview.md) 如何扩展AEMas a Cloud ServiceAsset compute微服务！ 您现在应该能够设置、开发、测试、调试和部署自定义Asset compute工作程序，以供AEMas a Cloud Service创作服务使用。

### 在Github上查看完整项目源代码

Github上提供了最终Asset compute项目，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github包含是项目的最终状态，已使用工作程序和测试用例完全填充，但不包含任何凭据，即 `.env`, `.config.json` 或 `.aio`._

## 疑难解答

+ [AEM中的资源缺少自定义演绎版](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的资源处理失败](../troubleshooting.md#asset-processing-fails)
