---
title: 创建Asset compute项目以实现Asset compute可扩展性
description: asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合特定结构，允许将它们部署到Adobe I/O Runtime并与AEM作为Cloud Service集成。
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 1%

---


# 创建Asset compute项目

asset compute项目是使用Adobe I/OCLI生成的Node.js项目，符合允许将这些项目部署到Adobe I/O Runtime并与AEM作为Cloud Service集成的特定结构。 单个Asset compute项目可以包含一个或多个Asset compute工作程序，每个工作程序都具有可从AEM作为Cloud Service处理配置文件引用的离散HTTP端点。

## 生成项目

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_点进以生成Asset compute项目（无音频）_

使用[Adobe I/OCLIAsset compute插件](../set-up/development-environment.md#aio-cli)生成新的空Asset compute项目。

1. 在命令行中，导航到要包含项目的文件夹。
1. 从命令行中，执行`aio app init`以开始交互式项目生成CLI。
   + 此命令可能会生成Web浏览器，提示进行身份验证以Adobe I/O。如果是，请提供与[所需的Adobe服务和产品](../set-up/accounts-and-services.md)关联的Adobe凭据。 如果您无法登录，请按照[有关如何生成项目](https://www.adobe.io/project-firefly/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user)的说明操作。
1. __选择组织__
   + 选择将AEM作为Cloud Service的Adobe组织，Project Firefly将在中注册
1. __选择项目__
   + 找到并选择项目。 这是从Firefly项目模板创建的[项目标题](../set-up/firefly.md)，在此例中为`WKND AEM Asset Compute`
1. __选择工作区__
   + 选择`Development`工作区
1. __您希望为此项目启用哪些Adobe I/O应用程序功能？选择要包含的组件__
   + 选择 `Actions: Deploy runtime actions`
   + 使用箭头键选择和空格键取消选择/选择，然后按Enter键确认选择
1. __选择要生成的操作类型__
   + 选择 `Adobe Asset Compute Worker`
   + 使用箭头键进行选择，空格键进行取消选择/选择，Enter键进行确认选择
1. __要如何命名此操作？__
   + 使用默认名称`worker`。
   + 如果您的项目包含多个执行不同资产计算的工作程序，请在语义上为它们命名

## 生成console.json

开发人员工具需要名为`console.json`的文件，该文件包含连接到Adobe I/O所需的凭据。此文件将从Adobe I/O控制台下载。

1. 打开Asset compute工作者的[Adobe I/O](https://console.adobe.io)项目
1. 选择项目工作区以下载的`console.json`凭据，在此例中，选择`Development`
1. 转到Adobe I/O项目的根目录，然后点按右上角的&#x200B;__下载全部__。
1. 文件将下载为带有项目和工作区前缀的`.json`文件，例如：`wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 将文件重命名为`config.json`，并将其移动到Asset compute工作项目的根中。 这是本教程中的方法。
   + 将其移入任意文件夹，并从具有配置项`ASSET_COMPUTE_INTEGRATION_FILE_PATH`的`.env`文件中引用该文件夹。 文件路径可以是绝对路径，也可以是相对于项目根路径的绝对路径。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或者
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 文件包含凭据。 如果将文件存储在项目中，请确保将其添加到`.gitignore`文件中以阻止共享。 这同样适用于`.env`文件 — 这些凭据文件不得共享或存储在Git中。

## 审查项目结构

生成的Asset compute项目是一个Node.js项目，可用作专门的Adobe项目Firefly项目。 以下结构元素对Asset compute项目具有特殊性：

+ `/actions` 包含子文件夹，每个子文件夹都定义一个Asset compute工作程序。
   + `/actions/<worker-name>/index.js` 定义用于执行此工作程序的JavaScript。
      + 文件夹名称`worker`是默认名称，只要它在`manifest.yml`中注册，即可为任何内容。
      + 可根据需要在`/actions`下定义多个工作文件夹，但必须在`manifest.yml`中注册。
+ `/test/asset-compute` 包含每个工作程序的测试包。与`/actions`文件夹类似， `/test/asset-compute`可以包含多个子文件夹，每个子文件夹都与它测试的工作程序相对应。
   + `/test/asset-compute/worker`，表示特定工作人员的测试包，包含表示特定测试包的子文件夹，以及测试输入、参数和预期输出。
+ `/build` 包含执行Asset compute测试案例的输出、日志和工件。
+ `/manifest.yml` 定义项目提供的Asset compute工作程序。必须在此文件中枚举每个工作程序实施，以使其可作为Cloud Service供AEM使用。
+ `/console.json` 定义Adobe I/O配置
   + 可以使用`aio app use`命令生成/更新此文件。
+ `/.aio` 包含aio CLI工具使用的配置。
   + 可以使用`aio app use`命令生成/更新此文件。
+ `/.env` 在语法中定义环 `key=value` 境变量，并包含不应共享的密钥。要保护这些密钥，不应将此文件签入Git，而应通过项目的默认`.gitignore`文件忽略此文件。
   + 可以使用`aio app use`命令生成/更新此文件。
   + 此文件中定义的变量可由命令行上的[导出变量](../deploy/runtime.md)来覆盖。

有关项目结构审查的更多详细信息，请查看[Adobe项目Firefly项目的解剖](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)。

开发的大部分内容发生在开发工作程序实施的`/actions`文件夹中，以及为自定义Asset compute工作程序编写测试的`/test/asset-compute`中。

## asset computeGitHub上的项目

最终Asset compute项目可在GitHub上获取，网址为：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含项目的最终状态，已完全填充工作程序和测试案例，但不包含任何凭据(即 `.env`或 `console.json`  `.aio`)。_

