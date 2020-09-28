---
title: 为AEM设置开发工具作为Cloud Service开发
description: 设置一个本地开发机器，它具有针对AEM本地开发所需的所有基准工具。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 0%

---


# 设置开发工具

Adobe Experience Manager(AEM)开发需要在开发者机器上安装和设置最少的开发工具集。 这些工具支持AEM项目的开发和构建。

请注 `~` 意，它用作用户目录的简写。 在Windows中，这相当于 `%HOMEPATH%`。

## 安装Java

Experience Manager是Java应用程序，因此需要Java SDK支持开发，而AEM则是Cloud ServiceSDK。

1. [下载并安装最新版Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+11%7E&amp;orderby=%40jcr%3content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=列表&amp;p.offset=0&amp;p.limit=14)
1. 通过运行以下命令验证Java 11 SDK是否已安装：
   + Windows: `java -version`
   + macOS/Linux: `java --version`

![Java](./assets/development-tools/java.png)

## 安装Homebrew

_使用Homebrew是可选的，但建议使用。_

Homebrew是适用于macOS、Windows和Linux的开放源代码包管理器。 所有支持工具都可以单独安装，Homebrew为安装和更新Experience Manager开发所需的各种开发工具提供了一种方便的方法。

1. 打开终端
1. 通过运行以下命令检查Homebrew是否已安装： `brew --version`.
1. 如果未安装Homebrew，请安装Homebrew
   + [在macOS上安装Homebrew](https://brew.sh/)
      + macOS上的Homebrew [需要](https://apps.apple.com/us/app/xcode/id497799835) Xcode [或命令行工具](https://developer.apple.com/download/more/)，可通过命令进行安装：
         + `xcode-select --install`
   + [在Linux上安装Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [在Windows 10上安装Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 通过运行以下命令验证Homebrew是否已安装： `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

如果您使用Homebrew，请按照以 __下各节中的__ “使用Homebrew安装”说明进行操作。 如果您不 __使用__ Homebrew，请使用操作系统特定链接安装工具。

## 安装Git

[Git](https://git-scm.com/) 是Adobe云管理器使用的 [源控制管理系统](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html)，是开发的必备工具。

+ 使用Homebrew安装Git
   1. 打开终端／命令提示符
   1. 执行命令： `brew install git`
   1. 使用以下命令验证Git是否已安装： `git --version`
+ 或者，下载并安装Git（macOS、Linux或Windows）
   1. [下载并安装Git](https://git-scm.com/downloads)
   1. 打开终端／命令提示符
   1. 使用以下命令验证Git是否已安装： `git --version`

![Git](./assets/development-tools/git.png)

## 安装Node.js（和npm）{#node-js}

[Node.js是](https://nodejs.org) 一个JavaScript运行时环境，用于处理AEM项目的ui.frontend子项 __目的前__ 端资产。 Node.js与npm一起 [分发](https://www.npmjs.com/)，它是事实上的Node.js包管理器，用于管理JavaScript依赖关系。

+ 使用Homebrew安装Node.js
   1. 打开终端／命令提示符
   1. 执行命令： `brew install node`
   1. 使用以下命令验证Node.js是否已安装： `node -v`
   1. 使用以下命令验证npm是否已安装： `npm -v`
+ 或者，下载并安装Node.js（macOS、Linux或Windows）
   1. [下载并安装Node.js](https://nodejs.org/en/download/)
   1. 打开终端／命令提示符
   1. 使用以下命令验证Node.js是否已安装： `node -v`
   1. 使用以下命令验证npm是否已安装： `npm -v`

![Node.js和npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
> [AEM基于Project](https://github.com/adobe/aem-project-archetype)Archetype的AEM Projects在构建时安装Node.js的隔离版本。 使本地开发系统的版本与AEM Maven项目的Reactor pom.xml中指定的Node.js和npm版本保持同步（或接近）是件好事。
请参阅此 [示例AEM Project Recator pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) ，了解在何处找到Node.js和npm构建版本。

## 安装Maven

Apache Maven是开放源代码Java命令行工具，用于构建从AEM Project Maven Archetype生成的AEM项目。 所有主要IDE([IntelliJ IDEA](https://www.jetbrains.com/idea/)[、Visual Studio Code](https://code.visualstudio.com/)、 [Eclipse](https://www.eclipse.org/)等) 集成了Maven支持。

+ 使用Homebrew安装Maven
   1. 打开终端／命令提示符
   1. 执行命令： `brew install maven`
   1. 使用以下命令验证Maven是否已安装： `mvn -v`
+ 或者，下载并安装Maven（macOS、Linux或Windows）
   1. [下载Maven](https://maven.apache.org/download.cgi)
   1. [安装Maven](https://maven.apache.org/install.html)
   1. 打开终端／命令提示符
   1. 使用以下命令验证Maven是否已安装： `mvn -v`

![马文](./assets/development-tools/maven.png)

## 设置AdobeI/O CLI{#aio-cli}

Adobe [I](https://github.com/adobe/aio-cli)/O CLI `aio`（或）提 [供对各种Adobe服务（包括Cloud Manager和Asset Compute）的命](https://github.com/adobe/aio-cli-plugin-cloudmanager) 令行访问 [](https://github.com/adobe/aio-cli-plugin-asset-compute)。 AdobeI/O CLI作为Cloud Service在AEM的开发中起着不可或缺的作用，因为它使开发人员能够：

+ 作为Cloud Services服务从AEM尾日志
+ 通过CLI管理Cloud Manager管道

### 安装AdobeI/O CLI

1. 确 [保Node.js已安装](#node-js) ，因为AdobeI/O CLI是npm模块
   + 运行 `node --version` 以确认
1. 执 `npm install -g @adobe/aio-cli` 行以全局安 `aio` 装npm模块

### 设置AdobeI/O CLI云管理器插件{#aio-cloud-manager}

AdobeI/O云管理器插件允许aio CLI通过命令与Adobe云管理器 `aio cloudmanager` 交互。

1. 执 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 行以安 [装aio Cloud Manager插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

### 设置AdobeI/O CLI资产计算插件{#aio-asset-compute}

AdobeI/O云管理器插件允许aio CLI通过命令生成和运行资产计算工 `aio asset-compute` 作器。

1. 执 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 行以安 [装aio Asset Compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)。

### 设置AdobeI/O CLI身份验证

要使AdobeI/O CLI与Cloud Manager通信，必须在AdobeI/O控制台中创建Cloud Manager集成，并获取凭据才能成功进行身份验证。

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. 登录到 [console.adobe.io](https://console.adobe.io)
1. 确保包含要连接到的云管理器产品的组织在Adobe组织切换程序中处于活动状态
1. 新建或打开现有 [AdobeI/O项目](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + AdobeI/O控制台项目只是组织内的集成组，根据您希望如何管理集成，创建或使用现有项目
   + 如果创建新项目，则在出现提示时选择“空项目”（与从模板创建）
   + AdobeI/O控制台项目是Cloud Manager项目的不同概念
1. 创建与“开发人员-Cloud Service”用户档案集成的新Cloud Manager API
1. 获取需要填充AdobeI/O CLI的config.json的服务帐户(JWT) [凭据](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 将文 `config.json` 件加载到AdobeI/O CLI
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. 将文 `private.key` 件加载到AdobeI/O CLI
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

通 [过Adobe](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) I/O CLI开始执行Cloud Manager命令。

## 设置开发IDE

AEM开发主要由Java和前端（JavaScript、CSS等）开发和XML管理组成。 以下是AEM开发中最流行的IDE。

### IntelliJ IDEA

__[IntelliJ IDEA是用于](https://www.jetbrains.com/idea/)__ Java开发的功能强大的IDE。 IntelliJ IDEA有两种版本：免费的社区版和商业版（付费）终极版。 免费的社区版本足以用于AEM开发，但Ultimate扩展 [了其功能集](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [下载IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下载回购工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio代码

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)是一款面向前端开发人员的免费开放源代码工具。 可以设置Visual Studio代码，以在Adobe工具repo的帮助下与AEM集成内容同 __[步](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__。

Visual Studio Code是主要创建前端代码的前端开发人员的理想选择；JavaScript、CSS和HTML。 虽然VS Code通过扩展支持 [Java](https://code.visualstudio.com/docs/java/java-tutorial)，但它可能缺乏更多特定于Java的功能所提供的一些高级功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下载Visual Studio代码](https://code.visualstudio.com/Download)
+ [下载回购工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下载Aemfed VS代码扩展](https://aemfed.io/)
+ [下载AEM Sync VS代码扩展](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE是](https://www.eclipse.org/ide/)__ 用于Java开发的流行IDE，它支持Adobe提供的 __[AEM Developer Tools](https://eclipse.adobe.com/aem/dev-tools/)__ 插件，它提供一个用于创作IDE内容并将JCR内容与本地AEM实例同步的IDE内GUI。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下载Eclipse](https://www.eclipse.org/ide/)
+ [下载Eclipse Dev工具](https://eclipse.adobe.com/aem/dev-tools/)
