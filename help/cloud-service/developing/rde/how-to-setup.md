---
title: 如何建立快速开发环境
description: 了解如何为AEMas a Cloud Service设置快速开发环境。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: f714adaa9bb637c0c7b17837c1d4b9f2be737c5c
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 0%

---

# 如何建立快速开发环境

学习 **如何设置** AEMas a Cloud Service中的快速开发环境(RDE)。

本视频说明：

- 使用Cloud Manager将RDE添加到程序
- 使用Adobe IMS的RDE登录流程，它如何与任何其他AEMas a Cloud Service环境相似
- 设置 [Adobe I/O Runtime可扩展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也称为 `aio CLI`
- AEM RDE和Cloud Manager的设置和配置 `aio CLI` 插件。 有关交互模式，请参见 [设置说明](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 先决条件

下列内容应本地安装：

- [Node.js](https://nodejs.org/en/) （LTS — 长期支持）
- [npm 8+](https://docs.npmjs.com/)

## 本地设置

要部署 [WKND站点项目的](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 代码和内容从本地计算机进入RDE，请完成以下步骤。

### Adobe I/O Runtime可扩展CLI

安装Adobe I/O Runtime可扩展CLI，也称为 `aio CLI` 从命令行运行以下命令。

```shell
$ npm install -g @adobe/aio-cli
```

### 安装和设置aio CLI插件

aio CLI必须安装插件，并使用组织、项目和RDE环境ID进行设置，才能与RDE交互。 可使用更简单的交互模式或非交互模式通过aio CLI执行安装。

>[!BEGINTABS]

>[!TAB 交互模式]

使用安装和设置AEM RDE插件 `aio cli`的 `plugins:install` 命令。

1. 使用安装aio CLI的AEM RDE插件 `aio cli`的 `plugins:install` 命令。

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

   - 选择 __否__  如果您只使用单个RDE，并且希望将RDE配置全局存储在本地计算机上。

   - 选择 __是__ 如果您正在使用多个RDE，或者希望将RDE配置本地存储在当前文件夹的 `.aio` 文件（每个项目）。

5. 从可用选项列表中选择组织ID、项目ID和RDE环境ID。

6. 通过运行以下命令验证是否设置了正确的组织、程序和环境。

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB 非交互模式]

使用安装和设置Cloud Manager和AEM RDE插件 `aio cli`的 `plugins:install` 命令。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

cloud Manager插件，允许开发人员从命令行与Cloud Manager交互。

AEM RDE插件允许开发人员从本地计算机部署代码和内容。

必须配置aio CLI插件才能与RDE交互。

1. 首先，使用Cloud Manager复制组织、项目和环境ID的值。

   - 组织ID：复制值 **配置文件图片>帐户信息（内部）>模式窗口>当前组织ID**

   ![组织ID](./assets/Org-ID.png)

   - 项目ID：复制值 **项目概述>环境> {ProgramName}-rde >浏览器URI >之间的数字 `program/` 和`/environment`**

   ![项目和环境ID](./assets/Program-Environment-Id.png)

   - 环境ID：复制值 **项目概述>环境> {ProgramName}-rde >浏览器URI >之后的数字`environment/`**

   ![项目和环境ID](./assets/Program-Environment-Id.png)

1. 使用 `aio cli`的 `config:set` 命令通过运行以下命令来设置这些值。

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

RDE状态信息的显示方式与以下内容类似： _您的AEM项目_ 创作和发布服务的包和配置。

## 后续步骤

学习 [使用方法](./how-to-use.md) 一个RDE，用于从您喜爱的集成开发环境(IDE)中部署代码和内容以加快开发周期。


## 其他资源

[在程序文档中启用RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

设置 [Adobe I/O Runtime可扩展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也称为 `aio CLI`

[aio CLI用法和命令](https://github.com/adobe/aio-cli#usage)

[用于与AEM快速开发环境交互的Adobe I/O Runtime CLI插件](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager aio CLI插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)
