---
title: 设置本地开发环境以实现Asset compute可扩展性
description: 开发Asset compute工作程序（即Node.js JavaScript应用程序）需要与传统AEM开发不同的特定开发工具，这些工具从Node.js和各种npm模块到Docker Desktop和Microsoft Visual Studio代码。
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

AdobeAsset compute项目不能与AEM SDK提供的本地AEM运行时集成，而是使用其自己的工具链进行开发，这与基于AEM Maven项目原型的AEM应用程序要求的工具链不同。

要扩展Asset compute微服务，必须在本地开发者机器上安装以下工具。

## 简略设置说明

下面是设置说明的桥。 有关这些开发工具的详细信息，请参见下面的各个部分。

1. [安装Docker ](https://www.docker.com/products/docker-desktop) Desktop并拉取所需的Docker图像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [安装Visual Studio代码](https://code.visualstudio.com/download)
1. [安装Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 从命令行安装所需的npm模块和Adobe I/OCLI插件：

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有关简略安装说明的详细信息，请阅读以下各节。

## 安装Visual Studio代码{#vscode}

[Microsoft Visual Studio ](https://code.visualstudio.com/download) Code用于开发和调试Asset computeWorker。虽然可以使用其他与[JavaScript兼容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)来开发该工作器，但只有Visual Studio代码可以集成到[debug](../test-debug/debug.md)Asset compute工作器。

_Visual Studio代码1.48.x+是进行网络调试 [](#wskdebug) 的必需工具。_

本教程假定使用Visual Studio代码，因为它为扩展Asset compute提供了最佳开发人员体验。

## 安装Docker Desktop{#docker}

下载并安装最新、稳定的[Docker Desktop](https://www.docker.com/products/docker-desktop)，这是本地[test](../test-debug/test.md)和[debug](../test-debug/debug.md)Asset compute项目所必需的。

安装Docker Desktop后，请开始它并从命令行安装以下Docker图像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows计算机上的开发人员应确保他们正在对上述图像使用Linux容器。 [Docker for Windows文档](https://docs.docker.com/docker-for-windows/)中介绍了切换到Linux容器的步骤。

## 安装Node.js（和npm）{#node-js}

asset computeWorker基于[Node.js](https://nodejs.org/)，因此需要Node.js 10+（和npm）进行开发和构建。

+ [以与传统AEM开发相](../../local-development-environment/development-tools.md#node-js) 同的方式安装Node.js（和npm）。

## 安装Adobe I/OCLI{#aio}

[安装Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)，或 ____ aiois命令行(CLI)npm模块，该模块简化了Adobe I/O技术的使用和交互，用于生成和本地开发自定义Asset compute工作器。

```
$ npm install -g @adobe/aio-cli
```

## 安装Adobe I/OCLIAsset compute插件{#aio-asset-compute}

[Adobe I/OCLIAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安装wskdebug{#wskdebug}

下载并安装[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模块，以便于本地调试Asset compute工作器。

_Visual Studio代码1.48.x+是进行网络调试 [](#wskdebug) 的必需工具。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安装ngrok{#ngrok}

下载并安装[nrok](https://www.npmjs.com/package/ngrok) npm模块，它提供对本地开发机器的公共访问，以便于本地调试Asset compute工作程序。

```
$ npm install -g ngrok --unsafe-perm=true
```
