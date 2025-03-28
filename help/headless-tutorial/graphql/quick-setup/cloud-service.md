---
title: AEM Headless的AEM as a Cloud Service快速设置
description: AEM Headless快速设置允许您使用AEM Headless进行操作，它使用WKND Site示例项目中的内容以及一个通过AEM Headless GraphQL API使用内容的React应用程序。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# AEM Headless的AEM as a Cloud Service快速设置

AEM Headless快速设置使用WKND Site示例项目中的内容以及一个通过AEM Headless GraphQL API使用内容的示例React应用程序(SPA)，帮助您实际操作AEM Headless。

## 先决条件

进行此快速设置需要以下各项：

+ AEM as a Cloud Service沙盒环境（最好是开发环境）
+ 访问AEM as a Cloud Service和Cloud Manager
   + __AEM管理员__&#x200B;访问AEM as a Cloud Service的权限
   + __Cloud Manager — 部署管理员__&#x200B;访问Cloud Manager
+ 必须本地安装以下工具：
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + IDE(例如，[Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1.创建Cloud Manager Git存储库

首先，创建用于部署WKND站点的Cloud Manager Git存储库。 WKND站点是一个示例AEM网站项目，其中包含内容（内容片段）和快速设置的React应用程序使用的GraphQL AEM端点。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. 导航到[https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. 选择包含要用于此快速设置的AEM as a Cloud Service环境的Cloud Manager __项目__
1. 为WKND站点项目创建Git存储库
   1. 在顶部导航中选择&#x200B;__存储库__
   1. 在顶部操作栏中选择&#x200B;__添加存储库__
   1. 命名新的Git存储库： `aem-headless-quick-setup-wknd`
      + 每个Adobe组织的Git存储库名称必须唯一，
   1. 选择&#x200B;__保存__，并等待Git存储库初始化

## 2.将示例WKND站点项目推送到Cloud Manager Git存储库

创建Cloud Manager Git存储库后，从GitHub克隆WKND站点项目的源代码，并将其推送到Cloud Manager Git存储库。 现在，可使用Cloud Manager访问WKND站点项目，并将其部署到AEM as a Cloud Service环境。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. 从命令行中，从GitHub克隆示例WKND站点项目的源代码

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 将Cloud Manager Git存储库添加为远程存储库
   1. 在顶部导航中选择&#x200B;__存储库__
   1. 从顶部操作栏中选择&#x200B;__访问存储库信息__
   1. 执行在&#x200B;__中找到的命令从命令行向Git存储库__&#x200B;添加远程存储库

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 将示例项目的源代码从本地Git存储库推送到Cloud Manager Git存储库

   ```shell
   $ git push adobe main:main
   ```

   在系统提示输入凭据时，从Cloud Manager的&#x200B;__存储库信息__&#x200B;模式中提供&#x200B;__用户名__&#x200B;和&#x200B;__密码__。

## 3.将WKND站点部署到AEM as a Cloud Service

将WKND站点项目推送到Cloud Manager Git存储库后，无法使用Cloud Manager管道将其部署到AEM as a Cloud Service。

请记住，WKND站点项目提供了React应用程序通过AEM Headless GraphQL API使用的示例内容。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. 将&#x200B;__非生产部署管道__&#x200B;附加到新的Git存储库
   1. 在顶部导航中选择&#x200B;__管道__
   1. 从顶部操作栏中选择&#x200B;__添加管道__
   1. 在&#x200B;__配置__&#x200B;选项卡上
      1. 选择&#x200B;__部署管道__&#x200B;选项
      1. 将&#x200B;__非生产管道名称__&#x200B;设置为`Dev Deployment pipeline`
      1. 选择&#x200B;__部署触发器>Git更改__
      1. 选择&#x200B;__重要度量失败行为>立即继续__
      1. 选择&#x200B;__继续__
   1. 在&#x200B;__Source代码__&#x200B;选项卡上
      1. 选择&#x200B;__全栈代码__&#x200B;选项
      1. 从&#x200B;__符合条件的部署环境__&#x200B;选择框中选择&#x200B;__AEM as a Cloud Service开发环境__
      1. 在&#x200B;__存储库__&#x200B;选择框中选择`aem-headless-quick-setup-wknd`
      1. 从&#x200B;__Git分支__&#x200B;选择框中选择`main`
      1. 选择&#x200B;__保存__
1. 运行&#x200B;__开发部署管道__
   1. 在顶部导航中选择&#x200B;__概述__
   1. 在&#x200B;__管道__&#x200B;部分中找到新创建的&#x200B;__开发部署管道__
   1. 选择管道条目右侧的&#x200B;__...__
   1. 选择&#x200B;__运行__，然后在模式窗口中确认
   1. 选择当前正在运行的管道右侧的&#x200B;__...__
   1. 选择&#x200B;__查看详细信息__
1. 从管道执行的详细信息中，监控进度，直到成功完成为止。 管道执行应在30-40分钟之间进行。

## 4.下载并运行WKND React应用程序

使用WKND Site项目中的内容引导AEM as a Cloud Service，下载并启动示例WKND React应用程序，该应用程序通过AEM Headless GraphQL API使用WKND网站的内容。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. 从命令行中，从GitHub克隆React应用程序的源代码。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在IDE中打开文件夹`~/Code/aem-guides-wknd-graphql/react-app`。
1. 在IDE中，打开文件`.env.development`。
1. 从`REACT_APP_HOST_URI`属性指向AEM as a Cloud Service __Publish__&#x200B;服务的主机URI。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   要查找AEM as a Cloud Service发布服务的主机URI，请执行以下操作：

   1. 在Cloud Manager的顶部导航中选择&#x200B;__环境__
   1. 选择&#x200B;__开发__&#x200B;环境
   1. 找到&#x200B;__发布服务(AEM和Dispatcher)__&#x200B;链接&#x200B;__环境区段__&#x200B;表
   1. 复制链接的地址，并将其用作AEM as a Cloud Service发布服务的URI

1. 在IDE中，将更改保存到`.env.development`
1. 从命令行中，运行React应用程序

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 在本地运行的React应用程序从[http://localhost:3000](http://localhost:3000)开始，并显示一系列冒险，这些冒险是使用AEM Headless的GraphQL API从AEM as a Cloud Service中获得的。

## 5.在AEM中编辑内容

使用示例WKND React应用程序连接到AEM Headless GraphQL API并使用其内容，在AEM Author服务中创作内容，并查看React应用程序的体验如何一致更新。

_步骤的截屏_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. 登录到AEM as a Cloud Service创作服务
1. 导航到&#x200B;__Assets >文件> WKND共享>英语>冒险__
1. 打开&#x200B;__Cycling Southern Utah__&#x200B;文件夹
1. 选择&#x200B;__Cycling Southern Utah__&#x200B;内容片段，然后从顶部操作栏中选择&#x200B;__编辑__
1. 更新内容片段的某些字段，例如：
   + 标题： `Cycling Utah's National Parks`
   + 行程长度： `6 Days`
   + 难度： `Intermediate`
   + 价格： `3500`
   + 主图像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 在顶部操作栏中选择&#x200B;__保存__
1. 从顶部操作栏的&#x200B;__中选择__&#x200B;快速发布&#x200B;__...__
1. 刷新在[http://localhost:3000](http://localhost:3000)上运行的React应用程序。
1. 在React应用程序中，选择现在更新的循环冒险，并验证对内容片段所做的内容更改。

1. 使用相同的方法，在AEM创作服务中：
   1. 取消发布现有的冒险内容片段，并验证是否将其从React应用程序体验中删除
   1. 创建和发布新的冒险内容片段，并验证它是否显示在React应用程序体验中

   >[!TIP]
   >
   > 如果您不熟悉如何创建和发布新内容片段，或无法取消发布现有内容片段，请观看上面的屏幕截图。

## 恭喜！

恭喜！您已成功使用AEM Headless来支持React应用程序！

要详细了解React应用程序如何使用AEM as a Cloud Service中的内容，请查看[AEM Headless教程](../multi-step/overview.md)。 本教程探讨了如何在AEM中创建内容片段，以及此React应用程序如何以JSON形式使用其内容。

### 后续步骤

+ [启动AEM Headless教程](../multi-step/overview.md)
