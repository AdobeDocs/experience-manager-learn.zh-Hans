---
title: 快速设置SPA Editor和远程SPA
description: 了解如何在15分钟内启动和运行远程SPA和AEM SPA编辑器！
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 3%

---

# 快速设置

快速设置是一个快速的演练步骤，用于说明如何安装和运行WKND应用程序并作为远程SPA，以及如何使用AEM SPA Editor创作它。

快速设置会直接将您转到本教程的结束状态。

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_快速设置的视频演示_

## 前提条件

本教程需要满足以下条件：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 仅限macOS先决条件
   + [Xcode](https://developer.apple.com/xcode/) 或 [Xcode命令行工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码(分支：feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


本教程假定：

+ [Microsoft® Visual Studio代码](https://visualstudio.microsoft.com/) 作为IDE
+ 工作目录 `~/Code/wknd-app`
+ 在上运行AEM SDK作为创作服务 `http://localhost:4502`
+ 使用本地运行AEM SDK `admin` 密码帐户 `admin`
+ 在上运行SPA `http://localhost:3000`

## 启动AEM SDK快速启动

在端口4502上下载并安装AEM SDK快速启动，默认情况下为 `admin/admin` 凭据。

1. [下载最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 将AEM SDK解压缩到 `~/aem-sdk`
1. 运行AEM SDK快速入门Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK启动，并在 [http://localhost:4502](http://localhost:4502). 使用以下凭据登录：

+ 用户名: `admin`
+ 密码: `admin`

## 下载并安装WKND站点包

本教程依赖于 __WKND 2.1.0+秒__ 项目（用于内容）。

1. [下载最新版本的 `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 登录到AEM SDK的包管理器，网址为 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 和 `admin` 凭据。
1. __上传__ the `aem-guides-wknd.all.x.x.x.zip` 在步骤1中下载
1. 点按 __安装__ 按钮 `aem-guides-wknd.all-x.x.x.zip`

## 下载并安装WKND应用程序SPA包

要执行快速设置，此处提供了AEM包，其中包含教程的最终AEM配置和内容。

1. [下载 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下载 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. 登录到AEM SDK的包管理器，网址为 [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) 和 `admin` 凭据。
1. __上传__ the `wknd-app.all.x.x.x.zip` 在步骤1中下载
1. 点按 __安装__ 按钮 `wknd-app.all.x.x.x.zip`
1. __上传__ the `wknd-app.ui.content.sample.x.x.x.zip` 在步骤2中下载
1. 点按 __安装__ 按钮 `wknd-app.ui.content.sample.x.x.x.zip`

## 下载WKND应用程序源

从Github.com下载WKND应用程序的源代码，然后将包含对本教程中所执行的SPA所做更改的分支切换为。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## 启动SPA应用程序

从项目的根中，安装SPA项目npm依赖项并运行应用程序。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

如果运行时出错 `npm install` 尝试执行以下步骤：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

验证SPA是否在 [http://localhost:3000](http://localhost:3000).

## 在AEM SPA编辑器中创作内容

在创作内容之前，请安排浏览器窗口，以便AEM创作(`http://localhost:4502`)，并且远程SPA(`http://localhost:3000`)在右侧运行。 此安排允许您查看AEM源内容的更改如何立即反映在SPA中。

1. 登录到 [AEM SDK创作服务](http://localhost:4502) as `admin`
1. 导航到 __站点> WKND应用程序>美国> en__
1. 编辑 __WKND应用程序主页__
1. 切换到 __编辑__ 模式

### 创作主页视图的固定组件

1. 点按文本 __WKND冒险__ 激活固定标题组件(在SPA主页视图中硬编码)
1. 点按 __扳手__ 图标
1. 更改标题组件的内容并保存
1. 刷新运行的SPA `http://localhost:3000` 并查看所反映的更改

### 创作主页视图的容器组件

1. 仍在编辑 __WKND应用程序主页__...
1. 展开 __SPA编辑器的侧栏__ （左）
1. 点按 __组件__ 图标
1. 在WKND徽标下方和固定标题组件上方的容器组件中添加、更改或删除组件
1. 刷新运行的SPA `http://localhost:3000` 并查看所反映的更改

### 在动态路由上创作容器组件

1. 切换到 __预览__ 模式(在SPA编辑器中)
1. 点按 __巴厘岛冲浪营__ 卡片，导航到其动态路线
1. 在位于 __行程__ 标题
1. 刷新运行的SPA `http://localhost:3000` 并查看所反映的更改

新的AEM页面 __WKND应用程序主页> Adventure__ _必须_ 具有与相应冒险的内容片段名称匹配的AEM页面名称。 这是因为SPA到AEM页面的路由映射基于路由的最后一段，即内容片段的名称。

## 恭喜！

您只需快速了解AEM SPA Editor如何通过可控的可编辑区域来增强您的SPA! 如果您感兴趣 — 请查看本教程的其余部分，但请确保重新开始，因为在此快速设置中，您的本地开发环境现在处于教程的结束状态！
