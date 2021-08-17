---
title: 为Asset compute可扩展性设置本地开发环境
description: 开发Asset compute工作程序（即Node.js JavaScript应用程序）需要与传统AEM开发不同的特定开发工具，这些工具从Node.js和各种npm模块到Docker Desktop和Microsoft Visual Studio代码，不一而足。
feature: asset compute微服务
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: 集成、开发
role: Developer
level: Intermediate, Experienced
source-git-commit: fd72f3c85db8a56ec8abfd1609da53492ee54be2
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 0%

---


# 设置本地开发环境

AdobeAsset compute项目无法与AEM SDK提供的本地AEM运行时集成，并且是使用其自己的工具链开发的，与AEM应用程序基于AEM Maven项目原型所需的工具链不同。

要扩展Asset compute微服务，必须在本地开发人员计算机上安装以下工具。

## 简略设置说明

下面是一个库设置说明。 有关这些开发工具的详细信息，请参见下文的各节。

1. [安装Docker Desktop](https://www.docker.com/products/docker-desktop) 并提取所需的Docker映像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [安装Visual Studio代码](https://code.visualstudio.com/download)
1. [安装Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. 从命令行安装所需的npm模块和Adobe I/OCLI插件：

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有关简略安装说明的更多信息，请阅读以下部分。

## 安装Visual Studio代码{#vscode}

[Microsoft Visual Studio代码](https://code.visualstudio.com/download) 用于开发和调试Asset compute工作程序。虽然可以使用其他与JavaScript兼容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)来开发该工作程序，但只有Visual Studio代码可以集成到[debug](../test-debug/debug.md)Asset compute工作程序。[

本教程假定使用Visual Studio代码，因为它为扩展Asset compute提供了最佳开发人员体验。

## 安装Docker Desktop{#docker}

下载并安装最新、稳定的[Docker Desktop](https://www.docker.com/products/docker-desktop)，因为在本地Asset compute项目[test](../test-debug/test.md)和[debug](../test-debug/debug.md)时需要此功能。

安装Docker Desktop后，启动它并从命令行安装以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows计算机上的开发人员应确保他们正在对上述图像使用Linux容器。 [Docker for Windows文档](https://docs.docker.com/docker-for-windows/)中介绍了切换到Linux容器的步骤。

## 安装Node.js（和npm）{#node-js}

asset compute工作程序基于[Node.js](https://nodejs.org/)，因此需要Node.js 10+（和npm）来开发和构建。

+ [以与传统AEM开发相同的方式安装Node.js（和npm）](../../local-development-environment/development-tools.md#node-js) 。

## 安装Adobe I/OCLI{#aio}

[安装Adobe I/OCLI](../../local-development-environment/development-tools.md#aio-cli)，或 ____ 安装命令行(CLI)npm模块，该模块便于使用和与Adobe I/O技术交互，并用于生成和本地开发自定义Asset compute工作程序。

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_Adobe I/OCLI版本7.1.0是必需的。目前不支持更高版本的Adobe I/OCLI。_


## 安装Adobe I/OCLIAsset compute插件{#aio-asset-compute}

[Adobe I/OCLIAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安装wskdebug{#wskdebug}

下载并安装[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模块，以便于本地调试Asset compute工作程序。

_Visual Studio代码1.48.x+是进行wskdebugto工作 [](#wskdebug) 所必需的。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安装ngrok{#ngrok}

下载并安装[ngrok](https://www.npmjs.com/package/ngrok) npm模块，该模块提供对本地开发机器的公共访问，以便于对Asset compute工作程序进行本地调试。

```
$ npm install -g ngrok --unsafe-perm=true
```
