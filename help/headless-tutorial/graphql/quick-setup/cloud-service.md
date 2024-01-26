---
title: AEMas a Cloud Service的AEM Headless快速设置
description: AEM Headless快速设置允许您使用AEM Headless的实际操作，它使用WKND Site示例项目中的内容，以及一个通过AEM Headless GraphQL API使用内容的React应用程序。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 850
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# AEMas a Cloud Service的AEM Headless快速设置

AEM Headless快速设置允许您使用AEM Headless的实际操作，它使用WKND Site示例项目中的内容以及一个通过AEM Headless GraphQL API使用内容的示例React应用程序(SPA)。

## 前提条件

进行此快速设置需要以下各项：

+ AEMas a Cloud Service沙盒环境（最好是开发环境）
+ 访问AEMas a Cloud Service和Cloud Manager
   + __AEM管理员__ 访问AEMas a Cloud Service
   + __Cloud Manager — 部署管理员__ 对Cloud Manager的访问权限
+ 必须本地安装以下工具：
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + IDE(例如， [Microsoft® Visual Studio代码](https://code.visualstudio.com/))

## 1.创建Cloud Manager Git存储库

首先，创建用于部署WKND站点的Cloud Manager Git存储库。 WKND站点是一个示例AEM网站项目，其中包含内容（内容片段）和快速设置的React应用程序使用的GraphQL AEM端点。

_步骤截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. 导航到 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. 选择Cloud Manager __项目__ 包含用于此快速设置的AEMas a Cloud Service环境
1. 为WKND站点项目创建Git存储库
   1. 选择 __存储库__ 在顶部导航中
   1. 选择 __添加存储库__ 在顶部操作栏中
   1. 命名新的Git存储库： `aem-headless-quick-setup-wknd`
      + 每个Adobe组织的Git存储库名称必须是唯一的，
   1. 选择 __保存__，并等待Git存储库初始化

## 2.将示例WKND站点项目推送到Cloud Manager Git存储库

创建Cloud Manager Git存储库后，从GitHub克隆WKND站点项目的源代码，并将其推送到Cloud Manager Git存储库。 现在，Cloud Manager可访问WKND站点项目，并将其部署到AEMas a Cloud Service环境。

_步骤截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. 从命令行中，从GitHub克隆示例WKND站点项目的源代码

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 将Cloud Manager Git存储库添加为远程存储库
   1. 选择 __存储库__ 在顶部导航中
   1. 选择 __访问存储库信息__ 从顶部操作栏
   1. 执行命令位于 __向Git存储库中添加远程存储库__ 从命令行

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 将示例项目的源代码从本地Git存储库推送到Cloud Manager Git存储库

   ```shell
   $ git push adobe main:main
   ```

   在系统提示输入凭据时，提供 __用户名__ 和 __密码__ 来自Cloud Manager的 __存储库信息__ 模式。

## 3.将WKND站点部署到AEMas a Cloud Service

将WKND站点项目推送到Cloud Manager Git存储库后，无法使用Cloud Manager管道将其部署到AEMas a Cloud Service。

请记住，WKND站点项目提供了React应用程序通过AEM Headless GraphQL API使用的示例内容。

_步骤截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. 附加 __非生产部署管道__ 到新的Git存储库
   1. 选择 __管道__ 在顶部导航中
   1. 选择 __添加管道__ 从顶部操作栏
   1. 在 __配置__ 选项卡
      1. 选择 __部署管道__ option
      1. 设置 __非生产管道名称__ 到 `Dev Deployment pipeline`
      1. 选择 __部署触发器>关于Git更改__
      1. 选择 __重要量度失败行为>立即继续__
      1. 选择 __继续__
   1. 在 __源代码__ 选项卡
      1. 选择 __全栈代码__ option
      1. 选择 __AEMas a Cloud Service开发环境__ 从 __符合条件的部署环境__ 选择框
      1. 选择 `aem-headless-quick-setup-wknd` 在 __存储库__ 选择框
      1. 选择 `main` 从 __Git分支__ 选择框
      1. 选择 __保存__
1. 运行 __开发部署管道__
   1. 选择 __概述__ 在顶部导航中
   1. 找到新创建的 __开发部署管道__ 在 __管道__ 部分
   1. 选择 __...__ 管道条目的右侧
   1. 选择 __运行__，并在模式窗口中确认
   1. 选择 __...__ 位于正在运行的管道的右侧
   1. 选择 __查看详细信息__
1. 从管道执行的详细信息中，监控进度，直到成功完成为止。 管道执行应在30-40分钟之间进行。

## 4.下载并运行WKND React应用程序

使用WKND Site项目中的内容引导AEMas a Cloud Service，下载并启动示例WKND React应用程序，该应用程序通过AEM Headless GraphQL API使用WKND网站的内容。

_步骤截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. 从命令行中，从GitHub克隆React应用程序的源代码。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 打开文件夹 `~/Code/aem-guides-wknd-graphql/react-app` 在IDE中。
1. 在IDE中，打开文件 `.env.development`.
1. 指向AEMas a Cloud Service __Publish__ 服务的主机URI  `REACT_APP_HOST_URI` 属性。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   要查找AEMas a Cloud Service发布服务的主机URI，请执行以下操作：

   1. 在Cloud Manager，选择 __环境__ 在顶部导航中
   1. 选择 __开发__ 环境
   1. 找到 __发布服务(AEM和Dispatcher)__ 链接 __环境区段__ 表
   1. 复制链接的地址，并将其用作AEMas a Cloud Service发布服务的URI

1. 在IDE中，将更改保存到 `.env.development`
1. 从命令行中，运行React应用程序

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 在本地运行的React应用程序会启动： [http://localhost:3000](http://localhost:3000) 和显示冒险列表，这些冒险源自AEM使用AEM Headless的GraphQL API进行as a Cloud Service。

## 5.在AEM中编辑内容

使用示例WKND React应用程序连接到AEM Headless GraphQL API并使用其内容，在AEM Author服务中创作内容，并查看React应用程序的体验如何一致更新。

_步骤截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. 登录到AEMas a Cloud Service创作服务
1. 导航到 __资产>文件> WKND已共享>英语>冒险__
1. 打开 __骑自行车去犹他州南部__ 文件夹
1. 选择 __骑自行车去犹他州南部__ 内容片段，并选择 __编辑__ 从顶部操作栏
1. 更新内容片段的某些字段，例如：
   + 标题： `Cycling Utah's National Parks`
   + 行程长度： `6 Days`
   + 困难： `Intermediate`
   + 价格： `3500`
   + 主映像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 选择 __保存__ 在顶部操作栏中
1. 选择 __快速发布__ 从顶部操作栏的 __...__
1. 刷新在上运行的React应用程序 [http://localhost:3000](http://localhost:3000).
1. 在React应用程序中，选择现在更新的循环冒险，并验证对内容片段所做的内容更改。

1. 使用相同的方法，在AEM创作服务中：
   1. 取消发布现有的冒险内容片段，并验证是否将其从React应用程序体验中删除
   1. 创建和发布新的冒险内容片段，并验证它是否显示在React应用程序体验中

   >[!TIP]
   >
   > 如果您不熟悉如何创建和发布新内容片段，或无法取消发布现有内容片段，请观看上面的屏幕截图。

## 恭喜！

恭喜！您已成功使用AEM Headless来支持React应用程序！

要详细了解React应用程序如何使用AEMas a Cloud Service中的内容，请查看 [AEM Headless指南](../multi-step/overview.md). 本教程探讨了如何在AEM中创建内容片段，以及此React应用程序如何以JSON形式使用其内容。

### 后续步骤

+ [启动AEM Headless教程](../multi-step/overview.md)
