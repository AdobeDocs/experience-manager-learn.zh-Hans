---
title: 设置AEMas a Cloud Service开发的开发工具
description: 设置本地开发机器，其中包含针对AEM进行本地开发所需的所有基线工具。
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '1498'
ht-degree: 7%

---

# 设置开发工具 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="设置开发工具"
>abstract="Adobe Experience Manager (AEM) 开发需要在开发人员计算机上，安装和设置一组必不可少的开发工具。这些工具包括 Java、Maven、Adobe I/O CLI、开发 IDE 等。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="开发准则"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hans" text="开发基础"

Adobe Experience Manager (AEM) 开发需要在开发人员计算机上，安装和设置一组必不可少的开发工具。这些工具支持AEM项目的开发和构建。

请注意 `~` 用作用户目录的简写形式。 在Windows中，这等同于 `%HOMEPATH%`.

## 安装Java

Experience Manager是一个Java应用程序，因此需要Java SDK才能支持开发和AEMas a Cloud ServiceSDK。

1. [下载并安装最新版本的Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;by.sort=desc&amp;list=0&amp;p.offset=14)
1. 通过运行以下命令验证是否已安装Java 11 SDK:
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## 安装Homebrew

_可以选择使用Homebrew，但建议使用。_

Homebrew是一个适用于macOS、Windows和Linux的开源包管理器。 所有支持工具都可以单独安装，Homebrew为安装和更新Experience Manager开发所需的各种开发工具提供了一种方便的方法。

1. 打开终端
1. 通过运行命令检查是否已安装Homebrew: `brew --version`.
1. 如果未安装Homebrew，请安装Homebrew
   + [在macOS上安装Homebrew](https://brew.sh/)
      + macOS的家酿需要 [Xcode](https://apps.apple.com/us/app/xcode/id497799835) 或 [命令行工具](https://developer.apple.com/download/more/)，通过命令安装：
         + `xcode-select --install`
   + [在Linux上安装Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [在Windows 10上安装Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 通过运行命令验证Homebrew是否已安装： `brew --version`

![家酿](./assets/development-tools/homebrew.png)

如果您使用的是Homebrew，请按照 __使用Homebrew安装__ 下面各节中的说明。 如果您 __not__ 使用Homebrew，使用特定于操作系统的链接安装工具。

## 安装Git

[Git](https://git-scm.com/) 是 [AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)，因此开发需要。

+ 使用Homebrew安装Git
   1. 打开终端/命令提示符
   1. 执行命令： `brew install git`
   1. 使用命令验证Git是否已安装： `git --version`
+ 或者，下载并安装Git(macOS、Linux或Windows)
   1. [下载并安装Git](https://git-scm.com/downloads)
   1. 打开终端/命令提示符
   1. 使用命令验证Git是否已安装： `git --version`

![Git](./assets/development-tools/git.png)

## 安装Node.js（和npm）{#node-js}

[Node.js](https://nodejs.org) 是一个JavaScript运行时环境，用于处理AEM项目的前端资产 __ui.frontend__ 子项目。 Node.js与 [npm](https://www.npmjs.com/)，是事实上的Node.js包管理器，用于管理JavaScript依赖项。

+ 使用Homebrew安装Node.js
   1. 打开终端/命令提示符
   1. 执行命令： `brew install node`
   1. 使用命令验证Node.js是否已安装： `node -v`
   1. 使用命令验证npm是否已安装： `npm -v`
+ 或者，下载并安装Node.js(macOS、Linux或Windows)
   1. [下载并安装Node.js](https://nodejs.org/en/download/)
   1. 打开终端/命令提示符
   1. 使用命令验证Node.js是否已安装： `node -v`
   1. 使用命令验证npm是否已安装： `npm -v`

![Node.js和npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM项目原型](https://github.com/adobe/aem-project-archetype)基于的AEM项目在生成时会安装一个孤立版本的Node.js。 最好将本地开发系统的版本与AEM Maven项目的Reactor pom.xml中指定的Node.js和npm版本保持同步（或接近）。
>
>请参阅此示例 [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) 在何处查找Node.js和npm内部版本。

## 安装Maven

Apache Maven是一款开源Java命令行工具，用于构建从AEM Project Maven Archetype生成的AEM项目。 所有主要IDE的([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio代码](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/)等) 集成了Maven支持。

+ 使用Homebrew安装Maven
   1. 打开终端/命令提示符
   1. 执行命令： `brew install maven`
   1. 使用命令验证Maven是否已安装： `mvn -v`
+ 或者，下载并安装Maven(macOS、Linux或Windows)
   1. [下载Maven](https://maven.apache.org/download.cgi)
   1. [安装Maven](https://maven.apache.org/install.html)
   1. 打开终端/命令提示符
   1. 使用命令验证Maven是否已安装： `mvn -v`

![Maven](./assets/development-tools/maven.png)

## 设置Adobe I/OCLI{#aio-cli}

的 [Adobe I/OCLI](https://github.com/adobe/aio-cli)或 `aio`，提供对各种Adobe服务(包括 [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) 和 [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/OCLI在AEMas a Cloud Service的开发中发挥着不可或缺的作用，因为它使开发人员能够：

+ 从AEM as a Cloud Services服务跟踪日志
+ 从CLI中管理Cloud Manager管道
+ 部署到 [AEM快速开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### 安装Adobe I/OCLI

1. 确保 [已安装Node.js](#node-js) 因为Adobe I/OCLI是npm模块
   + 运行 `node --version` 确认
1. 执行 `npm install -g @adobe/aio-cli` 安装 `aio` 全局npm模块

### 设置Adobe I/OCLI Cloud Manager插件{#aio-cloud-manager}

Adobe I/O云管理器插件允许aio CLI通过 `aio cloudmanager` 命令。

1. 执行 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 安装 [aio Cloud Manager插件](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### 设置Adobe I/OCLI身份验证

为了使Adobe I/OCLI与Cloud Manager通信， [必须在Adobe I/O控制台中创建Cloud Manager集成](https://github.com/adobe/aio-cli-plugin-cloudmanager)、和凭据才能成功进行身份验证。

1. 登录到 [console.adobe.io](https://console.adobe.io)
1. 确保包含要连接到的Cloud Manager产品的组织在Adobe组织切换器中处于活动状态
1. 创建新页面或打开现有页面 [Adobe I/O计划](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O控制台程序只是将集成分组，根据您管理集成的方式创建或使用现有程序
   + 如果创建新项目，则在出现提示时选择“空项目”(与&quot;从模板创建&quot;)
   + Adobe I/O控制台程序是Cloud Manager程序的不同概念
1. 使用“开发人员 — Cloud Service”配置文件创建新的Cloud Manager API集成
1. 获取服务帐户(JWT)凭据需要填充Adobe I/OCLI的 [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 加载 `config.json` 文件到Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. 加载 `private.key` 文件到Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

开始 [执行命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) (通过Adobe I/OCLI)。

### 设置AEM Rapid Development Environment插件{#rde}

AEM Rapid Development Environment插件允许aio CLI与AEMas a Cloud Service交互 [快速开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) 通过 `aio aem:rde` 命令。

1. 执行 `aio plugins:install @adobe/aio-cli-plugin-aem-rde` 安装 [AEM Rapid Development Environments插件](https://github.com/adobe/aio-cli-plugin-aem-rde).

### 设置Adobe I/OCLIAsset compute插件{#aio-asset-compute}

Adobe I/OCloud Manager插件允许aio CLI通过 `aio asset-compute` 命令。

1. 执行 `aio plugins:install @adobe/aio-cli-plugin-asset-compute` 安装 [aioAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute).


## 设置开发IDE

AEM开发主要由Java和前端（JavaScript、CSS等）开发和XML管理组成。 以下是用于AEM开发的最常用IDE。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ 是一款功能强大的IDE for Java开发工具。 IntelliJ IDEA有两种版本，一种是免费的社区版本，另一种是商业（付费）Ultimate版本。 免费的社区版本足以用于AEM开发，但最终版本 [扩展其功能集](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [下载IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下载Repo工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio代码

__[Visual Studio代码](https://code.visualstudio.com/)__ （VS代码）是面向前端开发人员的一款免费开源工具。 可以设置Visual Studio代码，以便借助Adobe工具将内容同步与AEM集成。 __[存储库](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code是主要创建前端代码的前端开发人员的理想选择；JavaScript、CSS和HTML。 而VS代码则通过 [扩展](https://code.visualstudio.com/docs/java/java-tutorial)，则可能缺少更特定于Java的一些高级功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下载Visual Studio代码](https://code.visualstudio.com/Download)
+ [下载Repo工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下载Aemfed VS代码扩展](https://aemfed.io/)
+ [下载AEM Sync VS代码扩展](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ 是用于Java开发的常用IDE，支持  __[AEM Developer Tools](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)__ 由Adobe提供的插件，它提供了用于创作的IDE内GUI，以及将JCR内容与本地AEM实例同步。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下载Eclipse](https://www.eclipse.org/ide/)
+ [下载Eclipse开发工具](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)
