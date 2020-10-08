---
title: 为资产计算可扩展性创建资产计算项目
description: 资产计算项目是使用AdobeI/O CLI生成的Node.js项目，符合特定结构，可将其部署到Adobe I/O Runtime并与AEM集成为Cloud Service。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 0%

---


# 创建资产计算项目

资产计算项目是使用AdobeI/O CLI生成的Node.js项目，符合允许将其部署到Adobe I/O Runtime并与AEM集成为Cloud Service的特定结构。 单个资产计算项目可以包含一个或多个资产计算工作程序，每个工作程序都具有可以从AEM作为Cloud Service处理用户档案的离散HTTP端点引用。

## 生成项目

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_生成资产计算项目的点进（无音频）_


使用 [AdobeI/O CLI资产计算插件](../set-up/development-environment.md#aio-cli) ，生成新的空资产计算项目。

1. 在命令行中，导航到要包含项目的文件夹。
1. 从命令行中，执 `aio app init` 行以开始交互式项目生成CLI。
   + 这会生成Web浏览器，提示对AdobeI/O进行身份验证。如果需要，请提供与所需Adobe服务和产 [品关联的Adobe凭据](../set-up/accounts-and-services.md)。 如果您无法登录，请按照以下说明操作，生成项目。
1. __选择组织__
   + 选择具有AEM的Adobe组织作为Cloud Service,Project Firefly将注册到
1. __选择项目__
   + 找到并选择项目。 这是从Firefly [项目模板](../set-up/firefly.md) 创建的项目标题，在此例中 `WKND AEM Asset Compute`
1. __选择工作区__
   + 选择工作 `Development` 区
1. __您要为此项目启用哪些AdobeI/O应用程序功能？ 选择要包括的组件__
   + 选择 `Actions: Deploy runtime actions`
   + 使用箭头键进行选择，使用空格键进行取消选择／选择，使用Enter键确认选择
1. __选择要生成的操作类型__
   + 选择 `Adobe Asset Compute Worker`
   + 使用箭头键进行选择，使用空格键取消选择／选择，使用Enter键确认选择
1. __您要如何命名此操作？__
   + 使用默认名称 `worker`。
   + 如果您的项目包含多个执行不同资产计算的工作线程，则从语义上对它们进行命名

## 审查项目剖析

生成的资产计算项目是专用的Adobe项目Firefly项目的Node.js项目，以下是Asset Compute项目的特有项：

+ `/actions` 包含子文件夹，每个子文件夹都定义一个资产计算工作人员。
   + `/actions/<worker-name>/index.js` 定义执行JavaScript以执行此工作者的工作。
      + 文件夹名 `worker` 称是默认名称，只要已在中注册，就可以是任何名称 `manifest.yml`。
      + 可以根据需要在下定义多个工作 `/actions` 者文件夹，但必须在中注册 `manifest.yml`。
+ `/test/asset-compute` 包含每个工作者的测试套件。 与文件夹 `/actions` 类似， `/test/asset-compute` 可以包含多个子文件夹，每个子文件夹都与它测试的工作器相对应。
   + `/test/asset-compute/worker`表示特定工作者的测试套件的子文件夹包含表示特定测试用例的子文件夹以及测试输入、参数和预期输出。
+ `/build` 包含资产计算测试用例执行的输出、日志和工件。
+ `/manifest.yml` 定义项目提供的资产计算工作程序。 必须在此文件中枚举每个工作器实现，以使它们作为Cloud Service提供给AEM。
+ `/.aio` 包含aio CLI工具使用的配置。 可以通过命令配置此 `aio config` 文件。
+ `/.env` 在语法中定义环境 `key=value` 变量，并包含不应共享的机密。 要保护这些机密，不应将此文件签入Git，并通过项目的默认文件忽略 `.gitignore` 此文件。
   + 在该文件中定义的变量可通过在命 [令行上导出](../deploy/runtime.md) 变量来覆盖。

有关项目结构审阅的更多详细信息，请 [查看Adobe项目Firefly项目的剖析](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)。

开发的大部分内容发生在开发工 `/actions` 作者实施的文件夹中，以及编写自 `/test/asset-compute` 定义资产计算工作者的测试中。

## Github上的资产计算项目

Github上提供最终的资产计算项目：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是项目的最终状态，它完全填充了工作者和测试用例，但不包含任何凭据，如`.env`,`.config.json`or`.aio`._