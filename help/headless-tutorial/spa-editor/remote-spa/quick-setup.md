---
title: 快速设置SPA Editor和远程SPA
description: 了解如何在15分钟内启动和运行远程SPA和AEM SPA编辑器！
topic: 无头、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
source-git-commit: 73c75f8dac85615f4ed2dfdcc2ee4d0e9e5d161a
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 3%

---


# 快速设置

快速设置是一个快速的演练步骤，用于说明如何安装和运行WKND应用程序并作为远程SPA，以及如何使用AEM SPA Editor创作它。

快速设置会直接将您转到本教程的结束状态。

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_快速设置的视频演示_

## 前提条件

本教程需要满足以下条件：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ 仅限macOS先决条件
   + [](https://developer.apple.com/xcode/) Xcodeor Xcode [命令行工具](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all.0.3.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql源代码](https://github.com/adobe/aem-guides-wknd-graphql)


本教程假定：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 码作为IDE
+ `~/Code/wknd-app`的工作目录
+ 在`http://localhost:4502`上作为创作服务运行AEM SDK
+ 使用本地`admin`帐户运行密码为`admin`的AEM SDK
+ 在`http://localhost:3000`上运行SPA

## 启动AEM SDK快速启动

在端口4502上下载并安装AEM SDK快速启动，默认为`admin/admin`凭据。

1. [下载最新的AEM SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. 将AEM SDK解压缩到`~/aem-sdk`
1. 运行AEM SDK快速入门Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK将在[http://localhost:4502](http://localhost:4502)上启动并自动启动。 使用以下凭据登录：

+ 用户名: `admin`
+ 密码: `admin`

## 下载并安装WKND站点包

本教程依赖于&#x200B;__WKND 0.3.0+的__&#x200B;项目（对于内容）。

1. [下载最新版本的  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. 使用`admin`凭据在[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)登录AEM SDK的包管理器。
1. ____ 上载 `aem-guides-wknd.all.x.x.x.zip` 步骤1中下载的
1. 点按&#x200B;__Install__&#x200B;按钮，查看`aem-guides-wknd.all-x.x.x.zip`条目

## 下载并安装WKND应用程序SPA包

要执行快速设置，将提供AEM包，其中包含教程的最终AEM配置和内容。

1. [下载 ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [下载 ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. 使用`admin`凭据在[http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr)登录AEM SDK的包管理器。
1. ____ 上载 `wknd-app.all.x.x.x.zip` 步骤1中下载的
1. 点按&#x200B;__Install__&#x200B;按钮，查看`wknd-app.all.x.x.x.zip`条目
1. ____ 上载 `wknd-app.ui.content.sample.x.x.x.zip` 步骤2中下载的
1. 点按&#x200B;__Install__&#x200B;按钮，查看`wknd-app.ui.content.sample.x.x.x.zip`条目

## 下载WKND应用程序源

从Github.com下载WKND应用程序的源代码，然后将包含对本教程中所执行的SPA所做更改的分支切换为。

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## 启动SPA应用程序

从项目的根中，安装SPA项目npm依赖项并运行应用程序。

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

## 在AEM SPA编辑器中创作内容

创作内容之前，请先排列浏览器窗口，使AEM创作(`http://localhost:4502`)位于左侧，远程SPA(`http://localhost:3000`)在右侧运行。 此安排允许您查看AEM源内容的更改如何立即反映在SPA中。

1. 登录到[AEM SDK创作服务](http://localhost:4502)作为`admin`
1. 导航至&#x200B;__站点> WKND应用程序>使用> en__
1. 编辑&#x200B;__WKND应用程序主页__
1. 切换到&#x200B;__编辑__&#x200B;模式

### 创作主页视图的固定组件

1. 点按文本&#x200B;__WKND Adventures__&#x200B;以激活固定的标题组件(硬编码到SPA主页视图中)
1. 点按标题组件操作栏上的&#x200B;__扳手__&#x200B;图标
1. 更改标题组件的内容并保存
1. 刷新`http://localhost:3000`上运行的SPA，并查看所反映的更改

### 创作主页视图的容器组件

1. 仍在编辑&#x200B;__WKND应用程序主页__&#x200B;时……
1. 展开&#x200B;__SPA Editor的侧栏__（左侧）
1. 点按&#x200B;__组件__&#x200B;图标
1. 在WKND徽标下方和固定标题组件上方的容器组件中添加、更改或删除组件
1. 刷新`http://localhost:3000`上运行的SPA，并查看所反映的更改

### 在动态路由上创作容器组件

1. 在SPA编辑器中切换到&#x200B;__预览__&#x200B;模式
1. 点按&#x200B;__Bali Surf Camp__&#x200B;卡，然后导航到其动态路由
1. 在位于&#x200B;__Itrinal__&#x200B;标题上方的容器组件中添加、更改或删除组件
1. 刷新`http://localhost:3000`上运行的SPA，并查看所反映的更改

__WKND应用程序主页> Adventure__ _下的新AEM页面必须_&#x200B;具有与相应冒险内容片段名称匹配的AEM页面名称。 这是因为SPA到AEM页面的路由映射基于路由的最后一段，即内容片段的名称。

## 恭喜！

您只需快速了解AEM SPA Editor如何通过可控的可编辑区域来增强您的SPA! 如果您感兴趣 — 请查看本教程的其余部分，但请确保重新开始，因为在此快速设置中，您的本地开发环境现在处于教程的结束状态！
