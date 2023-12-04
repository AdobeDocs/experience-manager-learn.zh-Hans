---
title: 为AEMas a Cloud Service开发设置开发工具
description: 设置本地开发计算机，并提供在本地针对AEM进行开发所需的所有基线工具。
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3592
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1278'
ht-degree: 6%

---

# 设置开发工具 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="设置开发工具"
>abstract="Adobe Experience Manager (AEM) 开发需要在开发人员计算机上，安装和设置一组必不可少的开发工具。这些工具包括 Java、Maven、Adobe I/O CLI、开发 IDE 等。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=zh-Hans" text="开发准则"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hans" text="开发基础"

Adobe Experience Manager (AEM) 开发需要在开发人员计算机上，安装和设置一组必不可少的开发工具。这些工具支持AEM项目的开发和构建。

请注意 `~` 用作用户目录的简写。 在Windows中，这等同于 `%HOMEPATH%`.

## 安装Java

Experience Manager是一种Java应用程序，因此需要Java SDK支持开发和AEMas a Cloud ServiceSDK。

1. [下载并安装最新版本的Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14444)
1. 通过运行以下命令验证是否已安装OracleJava 11 SDK：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## 安装Homebrew

_使用Homebrew是可选的，但建议使用。_

Homebrew是适用于macOS、Windows和Linux的开源包管理器。 所有的支持工具都可以单独安装，Homebrew为安装和更新Experience Manager开发所需的各种开发工具提供了便捷的方式。

1. 打开终端
1. 通过运行以下命令检查是否已安装Homebrew： `brew --version`.
1. 如果未安装Homebrew，请安装Homebrew

>[!BEGINTABS]

>[!TAB macOS]

[macOS上的自述](https://brew.sh/) 需要 [Xcode](https://apps.apple.com/us/app/xcode/id497799835) 或 [命令行工具](https://developer.apple.com/download/more/)，可通过命令安装：

```shell
$ xcode-select --install
```

>[!TAB Windows]

[在Windows 10上安装Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[在Linux上安装Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. 通过运行以下命令验证是否已安装Homebrew： `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

如果您使用的是Homebrew，请按照 __使用Homebrew安装__ 以下各节中的说明。 如果您是 __非__ 使用Homebrew，使用特定于操作系统的链接安装工具。

## 安装Git

[Git](https://git-scm.com/) 是使用的源代码控制管理系统 [AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)因此，是开发所必需的。

>[!BEGINTABS]

>[!TAB 使用Homebrew安装Git]

1. 打开终端/命令提示符
1. 执行命令： `$ brew install git`
1. 使用命令验证Git是否已安装： `$ git --version`

>[!TAB 下载并安装Git]

1. [下载并安装Git](https://git-scm.com/downloads)
1. 打开终端/命令提示符
1. 使用命令验证Git是否已安装： `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## 安装Node.js（和npm）{#node-js}

[Node.js](https://nodejs.org) 是一个用于处理AEM项目的前端资源的JavaScript运行时环境。 __ui.frontend__ 子项目。 Node.js的分布有 [npm](https://www.npmjs.com/)，是用于管理JavaScript依赖项的实际Node.js包管理器。

>[!BEGINTABS]

>[!TAB 使用Homebrew安装Node.js]

1. 打开终端/命令提示符
1. 执行命令： `$ brew install node`
1. 使用命令验证是否已安装Node.js： `$ node -v`
1. 使用命令验证是否已安装npm： `$ npm -v`

>[!TAB 下载并安装Node.js]

1. [下载并安装Node.js](https://nodejs.org/en/download/)
2. 打开终端/命令提示符
3. 使用命令验证是否已安装Node.js： `$ node -v`
4. 使用命令验证是否已安装npm： `$ npm -v`

>[!ENDTABS]

![Node.js和npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM项目原型](https://github.com/adobe/aem-project-archetype)基于的AEM项目在构建时安装隔离版本的Node.js。 最好使本地开发系统的版本与在AEM Maven项目的Reactor pom.xml中指定的Node.js和npm版本保持同步（或接近）。
>
>查看此示例 [AEM项目Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) 用于找到Node.js和npm内部版本的位置。

## 安装Maven

Apache Maven是一个开源Java命令行工具，用于构建从AEM项目Maven原型生成的AEM项目。 所有主要IDE ([IntelliJ IDEA](https://www.jetbrains.com/idea/)， [Visual Studio代码](https://code.visualstudio.com/)， [Eclipse](https://www.eclipse.org/)、等) 集成了Maven支持。


>[!BEGINTABS]

>[!TAB 使用Homebrew安装Maven]

1. 打开终端/命令提示符
1. 执行命令： `$ brew install maven`
1. 使用命令验证是否已安装Maven： `$ mvn -v`

>[!TAB 下载并安装Maven]

1. [下载Maven](https://maven.apache.org/download.cgi)
1. [安装Maven](https://maven.apache.org/install.html)
1. 打开终端/命令提示符
1. 使用命令验证是否已安装Maven： `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## 设置Adobe I/OCLI{#aio-cli}

此 [ADOBE I/OCLI](https://github.com/adobe/aio-cli)，或 `aio`，提供对各种Adobe服务的命令行访问，包括 [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) 和 [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/OCLI在AEMas a Cloud Service的开发中起着不可或缺的作用，因为它使开发人员能够：

+ AEM as aCloud Service服务中的尾日志
+ 从CLI管理Cloud Manager管道
+ 部署到 [AEM快速开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### 安装Adobe I/OCLI

1. 确保 [Node.js已安装](#node-js) 因为Adobe I/OCLI是npm模块
   + 运行 `node --version` 确认
1. 执行 `npm install -g @adobe/aio-cli` 安装 `aio` npm模块（全局）

### 设置Adobe I/OCLI Cloud Manager插件{#aio-cloud-manager}

Adobe I/OAdobe Cloud Manager插件允许aio CLI通过 `aio cloudmanager` 命令。

1. 执行 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 安装 [aio Cloud Manager插件](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### 设置Adobe I/OCLI身份验证

为了使Adobe I/OCLI与Cloud Manager通信， [必须在Adobe I/O控制台中创建Cloud Manager集成](https://github.com/adobe/aio-cli-plugin-cloudmanager)必须获取和凭据才能成功进行身份验证。

1. 登录 [console.adobe.io](https://console.adobe.io)
1. 确保包含要连接到的Cloud Manager产品的组织在Adobe组织切换器中处于活动状态
1. 创建新或打开现有 [Adobe I/O计划](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O控制台程序只是由集成、创建或使用以及现有程序组成的组织分组，具体取决于您希望如何管理集成
   + 如果创建新项目，则在出现提示时选择“空项目”（与“从模板创建”）
   + Adobe I/O控制台程序是与Cloud Manager程序不同的概念
1. 创建与“开发人员 — Cloud Service”配置文件的新Cloud Manager API集成
1. 获取服务帐户(JWT)凭据需要填充Adobe I/OCLI的 [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 加载 `config.json` 文件导入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. 加载 `private.key` 文件导入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

开始 [正在执行命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) 适用于Cloud Manager，通过Adobe I/OCLI。

### 设置AEM快速开发环境插件{#rde}

AEM Rapid Development Environment插件允许aio CLI与AEMas a Cloud Service交互 [快速开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) 通过 `aio aem:rde` 命令。

1. 执行 `aio plugins:install @adobe/aio-cli-plugin-aem-rde` 安装 [AEM快速开发环境插件](https://github.com/adobe/aio-cli-plugin-aem-rde).

### 设置Adobe I/OCLIAsset compute插件{#aio-asset-compute}

Adobe I/OCloud Manager插件允许aio CLI通过生成和运行Asset compute工作程序 `aio asset-compute` 命令。

1. 执行 `aio plugins:install @adobe/aio-cli-plugin-asset-compute` 安装 [aioAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute).

## 设置开发IDE

AEM开发主要包括了Java和前端（JavaScript、CSS等）开发以及XML管理。 以下是AEM开发中最常用的IDE。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ 是一个用于Java开发的强大IDE。 IntelliJ IDEA有两种风格：免费社区版和商业（付费）Ultimate版。 免费社区版本足以用于AEM开发，但最终版本是 [扩展其功能集](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [下载IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下载Repo工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio代码

__[Visual Studio代码](https://code.visualstudio.com/)__ (VS Code)是面向前端开发人员的免费开源工具。 Visual Studio代码可以设置为在Adobe工具的帮助下与AEM集成content sync， __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio代码是前端开发人员（主要创建前端代码）的理想选择；JavaScript、CSS和HTML。 而VS Code通过以下方式支持Java [扩展](https://code.visualstudio.com/docs/java/java-tutorial)中，它可能缺少由更特定于Java提供的某些高级功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下载Visual Studio代码](https://code.visualstudio.com/Download)
+ [下载Repo工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下载嵌入的VS代码扩展](https://aemfed.io/)
+ [下载AEM Sync VS代码扩展](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ 是用于Java开发的常用IDE，并且支持  __[AEM Developer Tools](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)__ Adobe插件，提供IDE中的GUI用于创作和将JCR内容与本地AEM实例同步。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下载Eclipse](https://www.eclipse.org/ide/)
+ [下载Eclipse开发工具](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)
