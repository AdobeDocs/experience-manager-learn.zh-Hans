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
source-git-commit: 59bfc9ae08acca6c41234f23eaa60f56e2eda890
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 2%

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
      + 这是通过获取的工作者的URL `aio app get-url`。 确保URL点位于正确的工作区，该工作区基于AEM作为正在配置处理用户档案的Cloud Service环境。 请注意，此子域与工作区 `development` 匹配。
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
         + 这些MIME类型是工作人员的Web服务所支持的唯一类型，这限制了自定义工作人员可以处理哪些资产。
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

## 疑难解答

### 资产中缺少自定义再现

+ __错误：__ 已成功处理新资产和重新处理的资产，但缺少自定义演绎版

#### 处理用户档案未应用于上级文件夹

+ __原因：__ 资产在具有使用自定义工作器的处理用户档案的文件夹下不存在
+ __解决方案：__ 将处理用户档案应用于资产的上级文件夹

#### 处理用户档案由较低的处理用户档案替代

+ __原因：__ 资产存在于应用了自定义工作用户档案的文件夹下，但在该文件夹和资产之间已应用了不使用客户工作人员的其他处理用户档案。
+ __解决方案：__ 合并或以其他方式协调两个处理用户档案并删除中间处理用户档案

### 资产处理失败

+ __错误：__ 资产处理失败标记显示在资产上
+ __原因：__ 执行自定义工作器时出错
+ __解决方案：__ 按照使用调试 [Adobe I/O Runtime激活](../test-debug/debug.md#aio-app-logs) 的说 `aio app logs`明。