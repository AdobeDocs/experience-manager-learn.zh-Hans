---
title: 为Asset Compute可扩展性设置本地开发环境
description: 开发Asset Compute工作程序(即Node.js JavaScript应用程序)需要与传统AEM开发不同的特定开发工具，这些工具从Node.js和各种npm模块到Docker Desktop和Microsoft Visual Studio Code，不一而足。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# 设置本地开发环境

Adobe Asset Compute项目无法与AEM SDK提供的本地AEM运行时集成，而是使用自己的工具链进行开发，这与基于AEM Maven项目原型的AEM应用程序所需的工具链不同。

要扩展Asset Compute微服务，必须在本地开发人员计算机上安装以下工具。

## 简略的设置说明

以下是节略设置说明。 有关这些开发工具的详细信息，请参阅下面各个部分。

1. [安装Docker Desktop](https://www.docker.com/products/docker-desktop)并提取所需的Docker映像：

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [安装Visual Studio代码](https://code.visualstudio.com/download)
1. [安装Node.js 10及更高版本](../../local-development-environment/development-tools.md#node-js)
1. 从命令行安装所需的npm模块和Adobe I/O CLI插件：

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

有关简化的安装说明的更多信息，请阅读以下部分。

## 安装Visual Studio代码{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download)用于开发和调试Asset Compute工作程序。 虽然可以使用其他[与JavaScript兼容的IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide)来开发工作程序，但只有Visual Studio代码可以集成到[debug](../test-debug/debug.md) Asset Compute工作程序。

本教程假定使用Visual Studio代码，因为它为扩展Asset Compute提供了最佳的开发人员体验。

## 安装Docker Desktop{#docker}

下载并安装最新的稳定[Docker Desktop](https://www.docker.com/products/docker-desktop)，因为这是本地[测试](../test-debug/test.md)和[调试](../test-debug/debug.md)Asset Compute项目所必需的。

安装Docker Desktop后，启动该程序，并从命令行安装以下Docker映像：

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Windows计算机上的开发人员应确保对上述图像使用Linux容器。 [Docker for Windows文档](https://docs.docker.com/docker-for-windows/)中介绍了切换到Linux容器的步骤。

## 安装Node.js（和npm）{#node-js}

Asset Compute工作程序基于[Node.js](https://nodejs.org/)，因此需要Node.js 10+（和npm）才能开发和构建。

+ [以与传统AEM开发相同的方式安装Node.js（和npm）](../../local-development-environment/development-tools.md#node-js)。

## 安装Adobe I/O CLI{#aio}

[安装Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli)，或&#x200B;__aio__&#x200B;是一个命令行(CLI) npm模块，便于使用Adobe I/O技术并与之交互，用于生成和本地开发自定义Asset Compute工作程序。

```
$ npm install -g @adobe/aio-cli
```

## 安装Adobe I/O CLI Asset Compute插件{#aio-asset-compute}

[Adobe I/O CLI Asset Compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## 安装wskdebug{#wskdebug}

下载并安装[Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模块，以便于Asset Compute工作程序的本地调试。

_需要Visual Studio Code 1.48.x+才能使[wskdebug](#wskdebug)正常工作。_

```
$ npm install -g @openwhisk/wskdebug
```

## 安装密钥{#ngrok}

下载并安装[ngrok](https://www.npmjs.com/package/ngrok) npm模块，该模块可提供对本地开发计算机的公共访问权限，以便Asset Compute工作程序的本地调试。

```
$ npm install -g ngrok --unsafe-perm=true
```
