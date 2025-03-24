---
title: 如何建立快速开发环境
description: 了解如何为AEM as a Cloud Service设置快速开发环境。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 0%

---

# 如何建立快速开发环境

了解&#x200B;**如何在AEM as a Cloud Service中设置**&#x200B;快速开发环境(RDE)。

本视频说明：

- 使用Cloud Manager将RDE添加到您的程序
- 使用Adobe IMS的RDE登录流程，它如何与任何其他AEM as a Cloud Service环境相似
- [Adobe I/O Runtime可扩展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)的设置，也称为`aio CLI`
- 使用非交互模式设置和配置AEM RDE和Cloud Manager `aio CLI`插件。 有关交互模式，请参阅[设置说明](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 先决条件

下列内容应本地安装：

- [Node.js](https://nodejs.org/en/) （LTS — 长期支持）
- [npm 8+](https://docs.npmjs.com/)

## 本地设置

若要从本地计算机将[WKND Sites项目的](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)代码和内容部署到RDE上，请完成以下步骤。

### Adobe I/O Runtime可扩展CLI

通过从命令行运行以下命令，安装Adobe I/O Runtime可扩展CLI（也称为`aio CLI`）。

```shell
$ npm install -g @adobe/aio-cli
```

### 安装和设置aio CLI插件

aio CLI必须安装插件，并使用组织、项目和RDE环境ID进行设置，才能与RDE交互。 可使用更简单的交互模式或非交互模式通过aio CLI执行安装。

>[!BEGINTABS]

>[!TAB 交互模式]

使用`aio cli`的`plugins:install`命令安装和设置AEM RDE插件。

1. 使用`aio cli`的`plugins:install`命令安装aio CLI的AEM RDE插件。

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   AEM RDE插件允许开发人员从本地计算机部署代码和内容。

2. 通过运行以下命令登录Adobe I/O Runtime可扩展CLI以获取访问令牌。 确保登录到与Cloud Manager相同的Adobe组织。

   ```shell
   $ aio login
   ```

3. 运行以下命令以使用交互模式设置RDE。

   ```shell
   $ aio aem:rde:setup
   ```

4. CLI会提示您输入组织ID、项目ID和环境ID。

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - 如果您只使用单个RDE，并且希望将RDE配置全局存储到本地计算机上，请选择&#x200B;__否__。

   - 如果您正在使用多个RDE，或者希望将RDE配置存储到每个项目的当前文件夹的`.aio`文件中，请选择&#x200B;__是__。

5. 从可用选项列表中选择组织ID、项目ID和RDE环境ID。

6. 通过运行以下命令验证是否设置了正确的组织、程序和环境。

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB 非交互模式]

使用`aio cli`的`plugins:install`命令安装和设置Cloud Manager和AEM RDE插件。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Cloud Manager插件允许开发人员从命令行与Cloud Manager交互。

AEM RDE插件允许开发人员从本地计算机部署代码和内容。

必须配置aio CLI插件才能与RDE交互。

1. 首先，使用Cloud Manager复制“组织”、“项目”和“环境ID”的值。

   - 组织ID：从&#x200B;**配置文件图片>帐户信息（内部）>模式窗口>当前组织ID**&#x200B;中复制值

   ![组织ID](./assets/Org-ID.png)

   - 项目ID：从&#x200B;**项目概述>环境> {ProgramName}-rde >浏览器URI >介于`program/`和`/environment`**&#x200B;之间的数字中复制值

   ![项目和环境ID](./assets/Program-Environment-Id.png)

   - 环境ID：从&#x200B;**程序概述>环境> {ProgramName}-rde >浏览器URI >位于`environment/`**&#x200B;之后的数字中复制值

   ![项目和环境ID](./assets/Program-Environment-Id.png)

1. 使用`aio cli`的`config:set`命令通过运行以下命令来设置这些值。

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. 通过运行以下命令验证当前配置值。

   ```shell
   $ aio config:list
   ```

1. 切换或查看您当前登录到的组织：

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## 验证RDE访问权限

通过运行以下命令验证AEM RDE插件的安装和配置。

```shell
$ aio aem:rde:status
```

RDE状态信息显示为：环境状态、_您的AEM项目_&#x200B;包以及创作和发布服务上的配置的列表。

## 后续步骤

了解[如何使用](./how-to-use.md) RDE从您喜爱的集成开发环境(IDE)中部署代码和内容，以加快开发周期。


## 其他资源

[在程序文档中启用RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

[Adobe I/O Runtime可扩展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)的设置，也称为`aio CLI`

[aio CLI用法和命令](https://github.com/adobe/aio-cli#usage)

用于与Adobe I/O Runtime快速开发环境交互的[AEM CLI插件](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager aio CLI插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)
