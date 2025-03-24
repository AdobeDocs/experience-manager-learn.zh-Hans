---
title: 将Asset Compute工作人员与AEM处理用户档案集成
description: AEM as a Cloud Service通过Asset Compute处理配置文件与部署到Adobe I/O Runtime的AEM Assets工作人员集成。 处理用户档案在创作服务中配置为使用自定义工作程序处理特定资产，并将工作程序生成的文件存储为资产演绎版。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# 与AEM处理用户档案集成

要让Asset Compute Worker在AEM as a Cloud Service中生成自定义演绎版，必须通过“处理配置文件”在AEM as a Cloud Service Author服务中注册它们。 受该处理配置文件约束的所有资产将在上传或重新处理时调用工作进程，并生成自定义演绎版，然后通过资产的演绎版提供该演绎版。

## 定义处理配置文件

首先创建新的处理配置文件，该配置文件将使用可配置的参数调用工作进程。

![正在处理配置文件](./assets/processing-profiles/new-processing-profile.png)

1. 以&#x200B;__AEM as a Cloud Service管理员__&#x200B;身份登录AEM创作服务。 由于这是一个教程，我们建议在沙盒中使用开发环境或环境。
1. 导航到&#x200B;__工具> Assets >处理配置文件__
1. 点按&#x200B;__创建__&#x200B;按钮
1. 命名处理配置文件，`WKND Asset Renditions`
1. 点按&#x200B;__自定义__&#x200B;选项卡，然后点按&#x200B;__新增__
1. 定义新服务
   + __演绎版名称：__ `Circle`
      + 用于在AEM Assets中标识此演绎版的文件名
   + __扩展名：__`png`
      + 生成的演绎版的扩展。 设置为`png`，因为这是辅助进程的Web服务支持的输出格式，并且会在圆切出后产生透明背景。
   + __终结点：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 这是通过`aio app get-url`获取的辅助进程的URL。 根据AEM as a Cloud Service环境，确保URL指向正确的工作区。
      + 确保Worker URL指向正确的工作区。 AEM as a Cloud Service Stage应使用Stage Workspace URL，而AEM as a Cloud Service Production应使用Production Workspace URL。
   + __服务参数__
      + 点按&#x200B;__添加参数__
         + 键： `size`
         + 值： `1000`
      + 点按&#x200B;__添加参数__
         + 键： `contrast`
         + 值： `0.25`
      + 点按&#x200B;__添加参数__
         + 键： `brightness`
         + 值： `0.10`
      + 这些键/值对将传递到Asset Compute工作进程，并可通过`rendition.instructions` JavaScript对象使用。
   + __Mime类型__
      + __包括：__ `image/jpeg`，`image/png`，`image/gif`，`image/bmp`，`image/tiff`
         + 这些MIME类型是工作人员的npm模块中仅有的类型。 此列表限制由自定义工作进程处理的工作。
      + __排除：__ `Leave blank`
         + 切勿使用此服务配置处理具有这些MIME类型的资产。 在这种情况下，我们仅使用允许列表。
1. 点按右上方的&#x200B;__保存__

## 应用和调用处理配置文件

1. 选择新创建的处理配置文件，`WKND Asset Renditions`
1. 点按顶部操作栏中的&#x200B;__将配置文件应用到文件夹__
1. 选择要将处理配置文件应用到的文件夹，如`WKND`，然后点按&#x200B;__应用__
1. 通过&#x200B;__AEM > Assets >文件__&#x200B;导航到未应用处理配置文件的文件夹，然后点按`WKND`。
1. 在应用了处理配置文件的文件夹下的任意文件夹中上传一些新图像资产（[sample-1.jpg](../assets/samples/sample-1.jpg)、[sample-2.jpg](../assets/samples/sample-2.jpg)和[sample-3.jpg](../assets/samples/sample-3.jpg)），并等待处理上传的资产。
1. 点按资产以打开其详细信息
   + 在AEM中，默认演绎版的生成和显示速度可能比自定义演绎版更快。
1. 从左侧边栏打开&#x200B;__呈现版本__&#x200B;视图
1. 点按名为`Circle.png`的资源并查看生成的演绎版

   ![生成的演绎版](./assets/processing-profiles/rendition.png)

## 已完成！

恭喜！您已完成有关如何扩展AEM as a Cloud Service Asset Compute微服务的[教程](../overview.md)！ 您现在应该能够设置、开发、测试、调试和部署自定义Asset Compute工作程序，以供AEM as a Cloud Service Author服务使用。

### 在Github上查看完整项目源代码

Github上提供了最终的Asset Compute项目，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github包含项目的最终状态，已使用辅助进程和测试用例完全填充，但不包含任何凭据，即 `.env`、`.config.json`或`.aio`._

## 疑难解答

+ [AEM中的资源缺少自定义演绎版](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的资源处理失败](../troubleshooting.md#asset-processing-fails)
