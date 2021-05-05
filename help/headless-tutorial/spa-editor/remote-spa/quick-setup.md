---
title: 快速设置SPA Editor和远程SPA
description: 了解如何在15分钟内使用远程SPA和AEM SPA编辑器快速入门和运行！
topic: 无外设、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 3%

---


# 快速设置

快速设置是一个快速的逐步介绍，它说明了如何安装和运行WKND应用程序并作为远程SPA，以及如何使用AEM SPA Editor创作它。

“快速设置”将直接带您进入本教程的结束状态。

## 前提条件

本教程要求：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码](https://github.com/adobe/aem-guides-wknd-graphql)

本教程假定：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 码(IDE)
+ `~/Code/wknd-app`的工作目录
+ 在`http://localhost:4502`上作为作者服务运行AEM SDK
+ 使用密码为`admin`的本地`admin`帐户运行AEM SDK
+ 在`http://localhost:3000`上运行SPA

## 开始AEM SDK快速入门

在端口4502上下载并安装AEM SDK Quickstart，默认凭据为`admin/admin`。

1. [下载最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 将AEM SDK解压缩到`~/aem-sdk`
1. 运行AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK将在[http://localhost:4502](http://localhost:4502)上开始并自动启动。 使用以下凭据登录：

+ 用户名: `admin`
+ 密码: `admin`

## 下载并安装WKND站点包

本教程依赖于&#x200B;__WKND 0.3.0+的__&#x200B;项目（对于内容）。

1. [下载最新版本的  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 使用`admin`凭据登录到位于[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)的AEM SDK的包管理器。
1. __上__ 载步 `aem-guides-wknd.all.x.x.x.zip` 骤1中下载的
1. 点按条目`aem-guides-wknd.all-x.x.x.zip`的&#x200B;__安装__&#x200B;按钮

## 下载并安装WKND应用程序SPA包

要执行快速设置，会提供包含教程的最终AEM配置和内容的AEM包。

1. [下载 `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下载 `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. 使用`admin`凭据登录到位于[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)的AEM SDK的包管理器。
1. __上__ 载步 `wknd-app.all.x.x.x.zip` 骤1中下载的
1. 点按条目`wknd-app.all.x.x.x.zip`的&#x200B;__安装__&#x200B;按钮
1. __上__ 载步 `wknd-app.ui.content.sample.x.x.x.zip` 骤2中下载的
1. 点按条目`wknd-app.ui.content.sample.x.x.x.zip`的&#x200B;__安装__&#x200B;按钮

## 下载WKND应用程序源

从Github.com下载WKND应用程序的源代码，并切换包含对本教程中所执行的SPA所做更改的分支。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## 开始SPA应用程序

从项目的根目录中，安装SPA项目npm依赖项并运行应用程序。

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

如果运行`npm install`时出错，请尝试以下步骤：

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

验证SPA是否在[http://localhost:3000](http://localhost:3000)运行。

## 在AEM SPA Editor中创作内容

在创作内容之前，请排列浏览器窗口，使AEM作者(`http://localhost:4502`)位于左侧，远程SPA(`http://localhost:3000`)在右侧运行。 此安排允许您查看AEM来源内容的更改如何立即反映在SPA中。

1. 以`admin`身份登录到[AEM SDK作者服务](http://localhost:4502)
1. 导航到&#x200B;__站点> WKND应用程序>我们> en__
1. 编辑&#x200B;__WKND应用程序主页__
1. 切换到&#x200B;__编辑__&#x200B;模式

### 创作主视图的固定组件

1. 点按文本&#x200B;__WKND Adventures__&#x200B;以激活固定标题组件(硬编码到SPA主视图中)
1. 点按标题组件操作栏上的&#x200B;__扳手__&#x200B;图标
1. 更改标题组件的内容并保存
1. 刷新`http://localhost:3000`上运行的SPA，并查看所反映的更改

### 创作主视图的容器组件

1. 仍在编辑&#x200B;__WKND应用程序主页__&#x200B;时……
1. 展开&#x200B;__SPA Editor的提要栏__（左侧）
1. 点按&#x200B;__组件__&#x200B;图标
1. 从位于WKND徽标下方和固定标题组件上方的容器组件中添加、更改或删除组件
1. 刷新`http://localhost:3000`上运行的SPA，并查看所反映的更改

### 在动态路由上创作容器组件

1. 在SPA Editor中切换到&#x200B;__预览__&#x200B;模式
1. 点按&#x200B;__Bali Surf Camp__&#x200B;卡并导航至其动态路由
1. 添加、更改或删除位于&#x200B;__Itrine__&#x200B;标题上方的容器组件中的组件
1. 刷新`http://localhost:3000`上运行的SPA，并查看所反映的更改

## 恭喜！

您刚刚快速了解AEM SPA编辑器如何通过受控的可编辑区域增强您的SPA! 如果您感兴趣 — 请查看教程的其余部分，但确保开始新鲜，因为在此快速设置中，您的本地开发环境现在处于教程的结束状态！
