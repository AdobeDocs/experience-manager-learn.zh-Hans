---
title: 快速设置SPA编辑器和远程SPA
description: 了解如何在15分钟内启动并运行远程SPA和AEM SPA Editor！
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 10%

---

# 快速设置

{{spa-editor-deprecation}}

快速设置是一个加速完成的演练，说明如何安装和运行WKND应用程序并作为远程SPA，以及使用AEM SPA编辑器创作它。

快速设置会将您直接引导至本教程的结束状态。

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_快速设置的视频演练_

## 先决条件

本教程需要以下内容：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hans)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 仅macOS先决条件
   + [Xcode](https://developer.apple.com/xcode/)或[Xcode命令行工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip 或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码(branch： feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


本教程假设：

+ 使用 [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) 作为 IDE
+ 工作目录为 `~/Code/wknd-app`
+ 在 `http://localhost:4502` 上以 Author 服务运行 AEM SDK
+ 使用本地 `admin` 账户（密码为 `admin`）运行 AEM SDK
+ 在 `http://localhost:3000` 上运行 SPA

## 启动AEM SDK快速入门

使用默认`admin/admin`凭据在端口4502上下载并安装AEM SDK快速入门。

1. [下载最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. 将AEM SDK解压缩到`~/aem-sdk`
1. 运行AEM SDK快速入门Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK将在[http://localhost:4502](http://localhost:4502)上启动并自动启动。 使用以下凭据登录：

+ 用户名： `admin`
+ 密码： `admin`

## 下载并安装WKND站点包

本教程依赖于&#x200B;__WKND 2.1.0+的__&#x200B;项目（内容）。

1. [下载最新版本的`aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 使用[凭据在:4502http://localhost](http://localhost:4502/crx/packmgr)/crx/packmgr`admin`登录AEM SDK的包管理器。
1. __上传__&#x200B;在步骤1中下载的`aem-guides-wknd.all.x.x.x.zip`
1. 点按条目&#x200B;__的__&#x200B;安装`aem-guides-wknd.all-x.x.x.zip`按钮

## 下载并安装WKND应用程序SPA包

为了执行快速设置，此处提供了AEM包，其中包含教程的最终AEM配置和内容。

1. [下载 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下载 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. 使用[凭据在:4502http://localhost](http://localhost:4502/crx/packmgr)/crx/packmgr`admin`登录AEM SDK的包管理器。
1. __上传__&#x200B;在步骤1中下载的`wknd-app.all.x.x.x.zip`
1. 点按条目&#x200B;__的__&#x200B;安装`wknd-app.all.x.x.x.zip`按钮
1. __上传__&#x200B;在步骤2中下载的`wknd-app.ui.content.sample.x.x.x.zip`
1. 点按条目&#x200B;__的__&#x200B;安装`wknd-app.ui.content.sample.x.x.x.zip`按钮

## 下载WKND应用程序源

从Github.com下载WKND应用程序的源代码，并切换包含在本教程中执行的对SPA的更改的分支。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## 启动SPA应用程序

从项目的根目录中，安装SPA项目npm依赖项并运行应用程序。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

如果在运行`npm install`时出现错误，请尝试以下步骤：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

验证SPA是否在[http://localhost:3000](http://localhost:3000)上运行。

## 在AEM SPA编辑器中创作内容

在创作内容之前，请排列浏览器窗口，使AEM Author (`http://localhost:4502`)位于左侧，远程SPA (`http://localhost:3000`)在右侧运行。 通过这种安排，您可以看到对AEM源内容的更改如何立即反映在SPA中。

1. 以[身份登录到](http://localhost:4502)AEM SDK创作服务`admin`
1. 导航到&#x200B;__站点> WKND应用程序>我们> en__
1. 编辑&#x200B;__WKND应用程序主页__
1. 切换到&#x200B;__编辑__&#x200B;模式

### 创作主页视图的固定组件

1. 点按文本&#x200B;__WKND Adventures__&#x200B;以激活固定标题组件（硬编码到SPA的“主页”视图中）
1. 点按标题组件操作栏上的&#x200B;__扳手__&#x200B;图标
1. 更改标题组件的内容并保存
1. 刷新`http://localhost:3000`上运行的SPA并查看更改是否反映出来

### 创作主页视图的容器组件

1. 仍在编辑&#x200B;__WKND应用程序主页__&#x200B;时……
1. 展开&#x200B;__SPA编辑器的侧栏__ （在左侧）
1. 点按&#x200B;__组件__&#x200B;图标
1. 添加、更改或删除容器组件（位于WKND徽标下方和固定标题组件上方）中的组件
1. 刷新`http://localhost:3000`上运行的SPA并查看更改是否反映出来

### 在动态路由上创作容器组件

1. 在SPA编辑器中切换到&#x200B;__预览__&#x200B;模式
1. 点按&#x200B;__巴厘岛冲浪营__&#x200B;卡并导航到其动态路径
1. 添加、更改或移除位于&#x200B;__行程__&#x200B;标题上方的容器组件
1. 刷新`http://localhost:3000`上运行的SPA并查看更改是否反映出来

__WKND应用程序主页> Adventure__ _下的新AEM页面必须_&#x200B;具有与相应冒险的内容片段名称匹配的AEM页面名称。 这是因为SPA到AEM页面映射的路由基于路由的最后一个区段，即内容片段的名称。

## 恭喜！

您刚刚快速了解了AEM SPA Editor如何通过受控、可编辑的区域来增强SPA！ 如果您感兴趣，请查看本教程的其余部分，但请确保重新开始，因为在本快速设置中，您的本地开发环境现在已处于本教程的结束状态！
