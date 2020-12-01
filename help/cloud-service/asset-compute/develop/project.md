---
title: 创建Asset compute项目以实现Asset compute可扩展性
description: asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合特定结构，允许将它们部署到Adobe I/O Runtime并与AEM集成为Cloud Service。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# 创建Asset compute项目

asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合允许将其部署到Adobe I/O Runtime并与AEM集成为Cloud Service的特定结构。 单个Asset compute项目可以包含一个或多个Asset compute工作器，每个工作器都具有可从AEM作为Cloud Service处理用户档案引用的离散HTTP端点。

## 生成项目

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_生成Asset compute项目的点进（无音频）_


使用[Adobe I/OCLIAsset compute插件](../set-up/development-environment.md#aio-cli)生成新的空Asset compute项目。

1. 在命令行中，导航到要包含项目的文件夹。
1. 在命令行中，执行`aio app init`以开始交互式项目生成CLI。
   + 这可能会生成Web浏览器，提示Adobe I/O进行身份验证。如果需要，请提供与[所需的Adobe服务和产品](../set-up/accounts-and-services.md)关联的Adobe凭据。 如果您无法登录，请按照[这些说明操作，了解如何生成项目](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)。
1. __选择组织__
   + 选择具有AEM的Adobe组织作为Cloud Service,Project Firefly将注册到
1. __选择项目__
   + 找到并选择项目。 这是从Firefly项目模板创建的[项目标题](../set-up/firefly.md)，在本例中为`WKND AEM Asset Compute`
1. __选择工作区__
   + 选择`Development`工作区
1. __您要为此项目启用哪些Adobe I/O应用程序功能？选择要包括的组件__
   + 选择 `Actions: Deploy runtime actions`
   + 使用箭头键进行选择，使用空格键进行取消选择／选择，使用Enter键确认选择
1. __选择要生成的操作类型__
   + 选择 `Adobe Asset Compute Worker`
   + 使用箭头键进行选择，使用空格键取消选择／选择，使用Enter键确认选择
1. __您要如何命名此操作？__
   + 使用默认名称`worker`。
   + 如果您的项目包含多个执行不同资产计算的工作线程，则从语义上对它们进行命名

## 生成console.json

从新建Asset compute项目的根目录中，运行以下命令以生成`console.json`。

```
$ aio app use
```

验证当前工作区详细信息是否正确，并且漂亮地`Y`或进入以生成`console.json`。 如果检测到`.env`和`.aio`已存在，请点按`x`以跳过其创建。

## 审查项目剖析

生成的Asset compute项目是专门的Adobe项目Firefly项目的Node.js项目，以下项目是Asset compute项目的特有项目：

+ `/actions` 包含子文件夹，每个子文件夹都定义一个Asset compute工作者。
   + `/actions/<worker-name>/index.js` 定义执行JavaScript以执行此工作者的工作。
      + 文件夹名称`worker`是默认名称，只要它已在`manifest.yml`中注册，就可以是任何内容。
      + 可以根据需要在`/actions`下定义多个工作文件夹，但必须在`manifest.yml`中注册这些文件夹。
+ `/test/asset-compute` 包含每个工作者的测试套件。与`/actions`文件夹类似，`/test/asset-compute`可以包含多个子文件夹，每个子文件夹都与它测试的工作器相对应。
   + `/test/asset-compute/worker`表示特定工作者的测试套件的子文件夹包含表示特定测试用例的子文件夹以及测试输入、参数和预期输出。
+ `/build` 包含Asset compute测试用例执行的输出、日志和伪像。
+ `/manifest.yml` 定义项目提供的Asset compute工作者。必须在此文件中枚举每个工作器实现，以使它们作为Cloud Service提供给AEM。
+ `/console.json` 定义Adobe I/O配置
   + 可以使用`aio app use`命令生成／更新此文件。
+ `/.aio` 包含aio CLI工具使用的配置。
   + 可以使用`aio app use`命令生成／更新此文件。
+ `/.env` 在语法中定义环境 `key=value` 变量，并包含不应共享的机密。可以生成此项，或者要保护这些机密，不应将此文件签入Git，并通过项目的默认`.gitignore`文件忽略此文件。
   + 可以使用`aio app use`命令生成／更新此文件。
   + 在此文件中定义的变量可以由命令行上的[导出变量](../deploy/runtime.md)覆盖。

有关项目结构审核的更多详细信息，请查看[Adobe项目Firefly项目的剖析](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)。

大部分开发发生在`/actions`文件夹中开发工作者实现以及在`/test/asset-compute`中为自定义Asset compute工作者编写测试。

## Githubasset compute项目

Github上提供最终Asset compute项目：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是项目的最终状态，它完全填充了工作者和测试用例，但不包含任何凭据，如`.env`, `console.json` 或 `.aio`。_

