---
title: AEM Headless快速设置AEMas a Cloud Service
description: AEM Headless快速设置可让您使用WKND Site示例项目中的内容，以及通过AEM Headless GraphQL API使用内容的React应用程序，来实践AEM Headless。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1075'
ht-degree: 0%

---


# AEM Headless快速设置AEMas a Cloud Service

AEM无头快速设置可让您使用WKND网站示例项目中的内容，以及通过AEM无头GraphQL API使用内容的示例React应用程序(SPA)，来实践AEM无头。

## 前提条件

要执行此快速设置，需要满足以下条件：

+ AEMas a Cloud Service沙盒环境（最好是开发）
+ 访问AEMas a Cloud Service和Cloud Manager
   + `AEM Administrator` 访问AEMas a Cloud Service
   + `Cloud Manager - Deployment Manager` 对Cloud Manager的访问权限
+ 必须在本地安装以下工具：
   + [Node.js v10+](https://nodejs.org/en/)
   + [npm 6+](https://www.npmjs.com/)
   + [Git](https://git-scm.com/)
   + IDE(例如， [Microsoft® Visual Studio代码](https://code.visualstudio.com/)

## 1.创建Cloud Manager Git存储库

首先，创建用于部署WKND站点的Cloud Manager Git存储库。 WKND网站是一个AEM网站项目示例，其中包含快速设置的React应用程序使用的内容（内容片段）和GraphQL AEM端点。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339073/?quality=12&learn=on)

1. 导航到 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. 选择Cloud Manager __项目__ 其中包含用于此快速设置的AEMas a Cloud Service环境
1. 为WKND站点项目创建Git存储库
   1. 选择 __存储库__ 在顶部导航中
   1. 选择 __添加存储库__ 在顶部操作栏中
   1. 将新的Git存储库命名为： `aem-headless-quick-setup`
   1. 选择 __保存__，然后等待Git存储库初始化

## 2.将示例WKND站点项目推送到Cloud Manager Git存储库

创建Cloud Manager Git存储库后，从GitHub克隆WKND站点项目的源代码，并将其推送到Cloud Manager Git存储库。 现在，Cloud Manager可以访问WKND Site项目并将其部署到AEMas a Cloud Service环境。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339074/?quality=12&learn=on)

1. 从命令行中，从GitHub中克隆示例WKND站点项目的源代码

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 将Cloud Manager Git存储库添加为远程存储库
   1. 选择 __存储库__ 在顶部导航中
   1. 选择 __访问存储库信息__ 从顶部操作栏
   1. 在中找到执行命令 __将远程存储库添加到Git存储库__ 从命令行

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup/
      ```

1. 将示例项目的源代码推送到Cloud Manager Git存储库

   1. 将代码从本地Git存储库推送到Cloud Manager Git存储库

      ```shell
      $ git push adobe master:main
      ```

      提示输入凭据时，请提供 __用户名__ 和 __密码__ 从Cloud Manager的 __存储库信息__ 模式窗口。

## 3.将WKND站点部署到AEMas a Cloud Service

将WKND Site项目推送到Cloud Manager Git存储库后，无法使用Cloud Manager管道将其部署到AEMas a Cloud Service。

请记住，WKND Site项目提供了React应用程序通过AEM无头GraphQL API使用的示例内容。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339075/?quality=12&learn=on)

1. 附加 __非生产部署管道__ 到新的Git存储库
   1. 选择 __管道__ 在顶部导航中
   1. 选择 __添加管道__ 从顶部操作栏
   1. 在 __配置__ 选项卡
      1. 选择 __部署管道__ 选项
      1. 设置 __非生产管道名称__ to `Dev Deployment pipeline`
      1. 选择 __部署触发器> On Git更改__
      1. 选择 __重要量度失败行为>立即继续__
      1. 选择 __继续__
   1. 在 __源代码__ 选项卡
      1. 选择 __完整堆栈代码__ 选项
      1. 选择 __AEMas a Cloud Service开发环境__ 从 __符合条件的部署环境__ 选择框
      1. 选择 `aem-headless-quick-setup` 在 __存储库__ 选择框
      1. 选择 `main` 从 __Git分支__ 选择框
      1. 选择 __保存__
1. 运行 __开发部署管道__
   1. 选择 __概述__ 在顶部导航中
   1. 找到新创建的 __开发部署管道__ 在 __管道__ 部分
   1. 选择 __...__ 到管道入口的右侧
   1. 选择 __运行__，并在模式窗口中确认
   1. 选择 __...__ 在正在运行的管道的右侧
   1. 选择 __查看详细信息__
1. 从管道执行的详细信息中，监控进度，直到成功完成。 管道执行需要45到60分钟。

## 4.下载并运行WKND React应用程序

通过使用WKND站点项目中的内容引导AEMas a Cloud Service，下载并启动示例WKND React应用程序，该应用程序会通过AEM无头GraphQL API使用WKND站点的内容。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339076/?quality=12&learn=on)

1. 从命令行中，从GitHub中克隆React应用程序的源代码。

   ```shell
   $ cd ~/Code
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 打开文件夹 `~/Code/aem-guides-wknd-graphql` 在IDE中。
1. 在IDE中，打开文件 `react-app/.env.development`.
1. 指向AEMas a Cloud Service __发布__ 服务的主机URI(来自  `REACT_APP_HOST_URI` 属性。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com/
   ...
   ```

   要查找AEMas a Cloud Service发布服务的主机URI，请执行以下操作：

   1. 在Cloud Manager中，选择 __环境__ 在顶部导航中
   1. 选择 __开发__ 环境
   1. 找到 __发布服务(AEM和Dispatcher)__ 链接 __环境区段__ 表
   1. 复制链接的地址，并将其用作AEMas a Cloud Service发布服务的URI

1. 在IDE中，将更改保存到 `.env.development`
1. 从命令行中，运行React App

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 在本地运行的React应用程序从 [http://localhost:3000](http://localhost:3000) 和显示冒险列表，这些冒险源自使用AEM Headless&#39; GraphQL API的AEMas a Cloud Service。

## 5.在AEM中编辑内容

当示例WKND React应用程序连接到AEM Headless GraphQL API并使用其中的内容时，在AEM创作服务中创作内容，并查看React应用程序的体验如何协同更新。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339077/?quality=12&learn=on)

1. 登录到AEMas a Cloud Service创作服务
1. 导航到 __资产>文件> WKND >英语>冒险__
1. 打开 __南犹他州自行车队__ 文件夹
1. 选择 __南犹他州自行车队__ 内容片段，然后选择 __编辑__ 从顶部操作栏
1. 更新内容片段的一些字段，例如：
   + 标题: `Cycling Utah's National Parks`
   + 行程时长： `6 Days`
   + 困难： `Intermediate`
   + 价格: `$3500`
   + 主映像： `/content/dam/wknd/en/activities/cycling/mountain-biking.jpg`
1. 选择 __保存__ 在顶部操作栏中
1. 选择 __快速发布__ 从顶部操作栏的 __...__
1. 刷新运行在上的React应用程序 [http://localhost:3000](http://localhost:3000).
1. 在React应用程序中，选择当前已更新，并验证对内容片段所做的内容更改。

1. 在AEM创作服务中，使用相同的方法：
   1. 取消发布现有的Adventure内容片段，并验证是否已从React应用程序体验中将其删除
   1. 创建并发布新的Adventure内容片段，然后验证它是否显示在React应用程序体验中

   >[!TIP]
   >
   > 如果您不熟悉如何创建和发布新内容或取消发布现有内容片段，请观看上面的屏幕截图。

## 恭喜！

恭喜！ 您已成功使用AEM Headless来为React应用程序提供支持！

要详细了解React应用程序如何使用AEMas a Cloud Service中的内容，请参阅 [AEM Headless教程](../multi-step/overview.md). 本教程探讨了创建时AEM中的内容片段，以及此React应用程序如何将其内容用作JSON。

### 后续步骤

+ [启动AEM Headless教程](../multi-step/overview.md)
