---
title: 为资产计算可扩展性设置本地开发环境
description: 开发资产计算工作程序（即Node.js JavaScript应用程序）需要与传统AEM开发不同的特定开发工具，这些工具从Node.js和各种npm模块到Docker Desktop和Microsoft Visual Studio代码。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# 设置本地开发环境

Adobe资产计算项目无法与AEM SDK提供的本地AEM运行时集成，并使用其自己的工具链进行开发，这与AEM Maven项目原型上的AEM应用程序要求的工具链不同。

要扩展资产计算微服务，必须在本地开发人员机器上安装以下工具。

## 简略设置说明

下面是设置说明的桥。 有关这些开发工具的详细信息，请参见下面的各个部分。

1. [安装Docker Desktop](https://www.docker.com/products/docker-desktop) ，并拉取所需的Docker图像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [安装Visual Studio代码](https://code.visualstudio.com/download)
1. [安装Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 从命令行安装所需的npm模块和AdobeI/O CLI插件：

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有关简略安装说明的详细信息，请阅读以下各节。

## 安装Visual Studio代码{#vscode}

[Microsoft Visual Studio代码](https://code.visualstudio.com/download) ，用于开发和调试资产计算工作程序。 虽然可 [以使用其他JavaScript兼容](../../local-development-environment/development-tools.md#set-up-the-development-ide) IDE来开发该工作器，但只能集成Visual Studio代码来调 [试Asset](../test-debug/debug.md) Compute Worker。

_Visual Studio代码1.48.x+是wskdebug工作所 [必需的](#wskdebug) 。_

本教程假定使用Visual Studio代码，因为它为扩展资产计算提供了最佳开发人员体验。

## 安装Docker Desktop{#docker}

下载并安装最新、稳 [定的Docker](https://www.docker.com/products/docker-desktop)Desktop，这是在本地测试和调 [试Asset](../test-debug/test.md) Compute [项目](../test-debug/debug.md) 所必需的。

安装Docker Desktop后，请开始它并从命令行安装以下Docker图像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows计算机上的开发人员应确保他们正在对上述图像使用Linux容器。 切换到Linux容器的步骤在Docker for Windows文 [档中有介绍](https://docs.docker.com/docker-for-windows/)。

## 安装Node.js（和npm）{#node-js}

资产计算工 [作程序基于Node](https://nodejs.org/).js，因此需要Node.js 10+（和npm）进行开发和构建。

+ [以与传统AEM开发相同的方式安装](../../local-development-environment/development-tools.md#node-js) Node.js（和npm）。

## 安装AdobeI/O CLI{#aio}

[安装AdobeI/O](../../local-development-environment/development-tools.md#aio-cli)CLI ____ ，或aio是一个命令行(CLI)npm模块，它简化了AdobeI/O技术的使用和交互，并用于生成和本地开发自定义资产计算工作程序。

```
$ npm install -g @adobe/aio-cli
```

## 安装AdobeI/O CLI Asset Compute插件{#aio-asset-compute}

Adobe [I/O CLI资产计算插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安装wskdebug{#wskdebug}

下载并安装 [Apache OpenWhisk调试npm](https://www.npmjs.com/package/@openwhisk/wskdebug) 模块，以便于本地调试资产计算工作器。

_Visual Studio代码1.48.x+是wskdebug工作所 [必需的](#wskdebug) 。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安装ngrok{#ngrok}

下载并安装 [ngrok](https://www.npmjs.com/package/ngrok) npm模块，它提供对本地开发机器的公共访问，以便于本地调试资产计算工作程序。

```
$ npm install -g ngrok --unsafe-perm=true
```
