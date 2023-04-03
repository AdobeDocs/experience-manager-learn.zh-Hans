---
title: 如何建立快速发展环境
description: 了解如何为AEM as a Cloud Service设置快速开发环境。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 1%

---


# 如何建立快速发展环境

学习 **如何设置** AEMas a Cloud Service中的快速开发环境(RDE)。

此视频显示：

- 使用Cloud Manager向程序添加RDE
- 使用Adobe IMS的RDE登录流程，它与任何其他AEMas a Cloud Service环境类似
- 设置 [Adobe I/O Runtime可扩展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也称为 `aio CLI`
- AEM RDE和Cloud Manager的设置和配置 `aio CLI` 插件

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 先决条件

应在本地安装以下内容：

- [Node.js](https://nodejs.org/en/) （LTS — 长期支持）
- [npm 8+](https://docs.npmjs.com/)

## 本地设置

部署 [WKND Sites项目的](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 将代码和内容从本地计算机上传到RDE，请完成以下步骤。

### Adobe I/O Runtime可扩展CLI

安装Adobe I/O Runtime Extensible CLI，也称为 `aio CLI` 从命令行中运行以下命令。

```shell
$ npm install -g @adobe/aio-cli
```

### AEM插件

使用 `aio cli`&#39;s `plugins:install` 命令。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Cloud Manager插件允许开发人员通过命令行与Cloud Manager交互。

AEM RDE插件允许开发人员从本地计算机部署代码和内容。

此外，要更新插件，请使用 `aio plugins:update` 命令。

## 配置AEM插件

必须将AEM插件配置为与RDE交互。 首先，使用Cloud Manager UI复制组织、项目和环境ID的值。

1. 组织ID:从 **配置文件图片>帐户信息（内部）>模式窗口>当前组织ID**

   ![组织 ID](./assets/Org-ID.png)

1. 程序ID:从 **项目概述>环境> {ProgramName}-rde >浏览器URI >之间的数字 `program/` 和`/environment`**

1. 环境ID:从 **项目概述>环境> {ProgramName}-rde >浏览器URI >后面的数字`environment/`**

   ![程序和环境ID](./assets/Program-Environment-Id.png)

1. 然后，通过使用 `aio cli`&#39;s `config:set` 命令通过运行以下命令来设置这些值。

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

您可以运行以下命令来验证当前配置值。

```shell
$ aio config:list
```

此外，要切换或了解您当前登录的组织，可以使用以下命令。

```shell
$ aio where
```

## 验证RDE访问权限

通过运行以下命令验证AEM RDE插件的安装和配置。

```shell
$ aio aem:rde:status
```

RDE状态信息的显示方式与环境状态、 _您的AEM项目_ 创作和发布服务上的包和配置。

## 后续步骤

学习 [如何使用](./how-to-use.md) 用于从您喜爱的集成开发环境(IDE)中部署代码和内容以缩短开发周期的RDE。


## 其他资源

[在项目文档中启用RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

设置 [Adobe I/O Runtime可扩展CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 也称为 `aio CLI`

[AIO CLI使用和命令](https://github.com/adobe/aio-cli#usage)

[Adobe I/O Runtime CLI插件，用于与AEM快速开发环境进行交互](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)
