---
title: 为Asset compute可扩展性创建Asset compute项目
description: asset compute项目是使用Adobe I/O CLI生成的Node.js项目，符合特定结构，允许将它们部署到Adobe I/O Runtime并作为Cloud Service与AEM集成。
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
topic: 集成、开发
role: 开发人员
level: 中级，经验丰富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 1%

---


# 创建Asset compute项目

asset compute项目是使用Adobe I/O CLI生成的Node.js项目，符合允许将它们部署到Adobe I/O Runtime并作为Cloud Service与AEM集成的特定结构。 单个Asset compute项目可以包含一个或多个Asset compute工作器，每个工作器都具有可从AEM作为Cloud Service处理用户档案的离散HTTP端点引用。

## 生成项目

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_生成Asset compute项目的点进（无音频）_

使用[Adobe I/O CLIAsset compute插件](../set-up/development-environment.md#aio-cli)生成新的空Asset compute项目。

1. 在命令行中，导航到要包含项目的文件夹。
1. 在命令行中，执行`aio app init`以开始交互式项目生成CLI。
   + 此命令可能生成Web浏览器，提示进行身份验证以Adobe I/O。如果需要，请提供与[所需的Adobe服务和产品](../set-up/accounts-and-services.md)关联的Adobe凭据。 如果无法登录，请按照以下说明操作，生成项目](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user)。[
1. __选择组织__
   + 选择将AEM作为Cloud Service的Adobe组织，Project Firefly已注册
1. __选择项目__
   + 找到并选择项目。 这是从Firefly项目模板创建的[项目标题](../set-up/firefly.md)，在此示例中为`WKND AEM Asset Compute`
1. __选择工作区__
   + 选择`Development`工作区
1. __您要为此项目启用哪些Adobe I/O应用程序功能？选择要包括的组件__
   + 选择 `Actions: Deploy runtime actions`
   + 使用箭头键进行选择，使用空格键取消选择/选择，使用Enter键确认选择
1. __选择要生成的操作类型__
   + 选择 `Adobe Asset Compute Worker`
   + 使用箭头键进行选择，使用空格键取消选择/选择，使用Enter键确认选择
1. __要如何命名此操作？__
   + 使用默认名称`worker`。
   + 如果您的项目包含多个执行不同资产计算的工作线程，请根据语义命名这些工作线程

## 生成console.json

开发人员工具需要一个名为`console.json`的文件，该文件包含连接到Adobe I/O所需的凭据。此文件将从Adobe I/O控制台下载。

1. 打开Asset compute工作人员的[Adobe I/O](https://console.adobe.io)项目
1. 选择项目工作区以下载`console.json`凭据，在此示例中，选择`Development`
1. 转到Adobe I/O项目的根目录，然后点按右上角的&#x200B;__下载全部__。
1. 文件下载为带有项目和工作区前缀的`.json`文件，例如：`wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 将文件重命名为`config.json`，并将其移到Asset computeworker项目的根目录中。 这是本教程中的方法。
   + 将其移入任意文件夹，并从具有配置项`ASSET_COMPUTE_INTEGRATION_FILE_PATH`的`.env`文件中引用该文件夹。 文件路径可以是绝对路径，也可以是相对于项目的根路径。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或者
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 文件包含凭据。 如果将文件存储在项目中，请确保将其添加到`.gitignore`文件以防止共享。 `.env`文件也是如此 — 这些凭据文件不得共享或存储在Git中。

## 审查项目剖析

生成的Asset compute项目是Node.js项目，用作专用的Adobe项目Firefly项目。 以下结构元素对Asset compute项目特有：

+ `/actions` 包含子文件夹，每个子文件夹都定义一个Asset compute工作者。
   + `/actions/<worker-name>/index.js` 定义用于执行此worker工作的JavaScript。
      + 文件夹名`worker`是默认名称，只要它已注册到`manifest.yml`中，就可以是任何内容。
      + 可以根据需要在`/actions`下定义多个worker文件夹，但必须在`manifest.yml`中注册。
+ `/test/asset-compute` 包含每个worker的测试套件。与`/actions`文件夹类似，`/test/asset-compute`可以包含多个子文件夹，每个子文件夹都对应于它测试的工作程序。
   + `/test/asset-compute/worker`，表示特定worker的测试套件，包含表示特定测试用例的子文件夹，以及测试输入、参数和预期输出。
+ `/build` 包含Asset compute测试用例执行的输出、日志和伪像。
+ `/manifest.yml` 定义项目提供的Asset compute工作者。必须在此文件中枚举每个工作器实现，以使它们作为Cloud Service可供AEM使用。
+ `/console.json` 定义Adobe I/O配置
   + 可以使用`aio app use`命令生成/更新此文件。
+ `/.aio` 包含aio CLI工具使用的配置。
   + 可以使用`aio app use`命令生成/更新此文件。
+ `/.env` 在语法中定 `key=value` 义环境变量，并包含不应共享的机密。要保护这些机密，不应将此文件签入Git，并通过项目的默认`.gitignore`文件忽略此文件。
   + 可以使用`aio app use`命令生成/更新此文件。
   + 在此文件中定义的变量可以由命令行上的[导出变量](../deploy/runtime.md)覆盖。

有关项目结构审阅的更多详细信息，请查阅[《AdobeProject Firefly项目](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)剖析》。

大部分开发发生在`/actions`文件夹中，开发worker实现，在`/test/asset-compute`中为自定义Asset computeworker编写测试。

## asset computeGitHub上的项目

最终的Asset compute项目可在GitHub上获得，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含项目的最终状态，已完全填充worker和测试案例，但不包含任何凭据，即、 `.env`或 `console.json` 凭据 `.aio`。_

