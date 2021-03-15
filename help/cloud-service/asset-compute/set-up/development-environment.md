---
title: 设置本地开发环境以实现Asset compute可扩展性
description: 开发Asset computeWorker（即Node.js JavaScript应用程序）需要与传统AEM开发不同的特定开发工具，这些工具包括Node.js和各种npm模块，Docker桌面和Microsoft Visual Studio代码。
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: 集成、开发
role: 开发人员
level: 中级，经验丰富的
translation-type: tm+mt
source-git-commit: 53c20b9774c15b04a1c78c7c0c7b61a60996bf60
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---


# 设置本地开发环境

Adobe Asset compute项目无法与AEM SDK提供的本地AEM运行时集成，并且是使用它们自己的工具链开发的，这与基于AEM Maven项目原型的AEM应用程序所需的工具链不同。

要扩展Asset compute microservices，必须在本地开发人员计算机上安装以下工具。

## 删节设置说明

下面是设置说明的桥。 有关这些开发工具的详细信息将在以下各节中介绍。

1. [安装Docker ](https://www.docker.com/products/docker-desktop) Desktop并拉取所需的Docker图像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [安装Visual Studio代码](https://code.visualstudio.com/download)
1. [安装Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 从命令行安装所需的npm模块和Adobe I/O CLI插件：

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有关简略安装说明的详细信息，请阅读以下各节。

## 安装Visual Studio代码{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) 用于开发和调试Asset computeWorker。其它[兼容JavaScript的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)可用于开发该worker，但只有Visual Studio代码可集成到[debug](../test-debug/debug.md)Asset computeworker。

_Visual Studio代码1.48.x+是进行Wskdebuto工作 [](#wskdebug) 的必需。_

本教程假定使用Visual Studio代码，因为它为扩展Asset compute提供了最佳的开发人员体验。

## 安装Docker Desktop{#docker}

下载并安装最新、稳定的[Docker Desktop](https://www.docker.com/products/docker-desktop)，因为在本地安装[test](../test-debug/test.md)和[debug](../test-debug/debug.md)Asset compute项目时需要安装。

安装Docker Desktop后，请开始它并从命令行安装以下Docker图像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows计算机上的开发人员应确保他们正在使用Linux容器处理上述图像。 [Docker for Windows文档](https://docs.docker.com/docker-for-windows/)中介绍了切换到Linux容器的步骤。

## 安装Node.js（和npm）{#node-js}

asset computeWorker基于[Node.js](https://nodejs.org/)，因此需要Node.js 10+（和npm）来开发和构建。

+ [以与传统AEM开发相同](../../local-development-environment/development-tools.md#node-js) 的方式安装Node.js（和npm）。

## 安装Adobe I/O CLI{#aio}

[安装Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli)，或aiois ____ 命令行(CLI)npm模块，该模块简化了Adobe I/O技术的使用和交互，用于生成自定义Asset compute工作线程并在本地进行开发。

```
$ npm install -g @adobe/aio-cli
```

## 安装Adobe I/O CLIAsset compute插件{#aio-asset-compute}

[Adobe I/OCLIAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安装wskdebug{#wskdebug}

下载并安装[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模块，以便于本地调试Asset computeWorker。

_Visual Studio代码1.48.x+是进行Wskdebuto工作 [](#wskdebug) 的必需。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安装ngrok{#ngrok}

下载并安装[nrok](https://www.npmjs.com/package/ngrok) npm模块，它提供对本地开发机器的公共访问，以便于本地调试Asset compute工作程序。

```
$ npm install -g ngrok --unsafe-perm=true
```
