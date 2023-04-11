---
title: AEM Headless使用本地AEM SDK快速设置
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 安装AEM SDK、添加示例内容并部署使用GraphQL API从AEM中使用内容的应用程序。 了解AEM如何为全渠道体验提供支持。
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 2%

---

# AEM Headless使用本地AEM SDK快速设置 {#setup}

AEM Headless快速设置可让您使用WKND网站示例项目中的内容来实践AEM Headless，以及一个可通过AEM Headless GraphQL API使用内容的示例React应用程序(SPA)。 本指南使用 [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## 前提条件 {#prerequisites}

应在本地安装以下工具：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;by.sort=desc&amp;list=0&amp;p.offset=14)
* [Node.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## 1.安装AEM SDK {#aem-sdk}

此设置使用 [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) 来浏览AEM GraphQL API。 本节提供了有关安装AEM SDK并在创作模式下运行该SDK的快速指南。 有关设置本地开发环境的更详细指南 [可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> 您还可以在本教程的后面使用 [AEMas a Cloud Service环境](./cloud-service.md). 整个教程中都包含有关使用云环境的其他说明。

1. 导航到 **[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEMas a Cloud Service** 并下载 **AEM SDK**.

   ![软件分发门户](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. 解压缩下载并复制快速入门Jar(`aem-sdk-quickstart-XXX.jar`)到专用文件夹，例如 `~/aem-sdk/author`.
1. 将jar文件重命名为 `aem-author-p4502.jar`.

   的 `author` 名称指定快速入门jar以“创作”模式启动。 的 `p4502` 指定在端口4502上运行快速启动。

1. 要安装和启动AEM实例，请在包含jar文件的文件夹中打开命令提示符，然后运行以下命令：

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 提供管理员密码作为 `admin`. 可接受任何管理员密码，但建议使用 `admin` 以便减少重新配置的需要。
1. 安装完AEM服务后，应会在 [http://localhost:4502](http://localhost:4502).
1. 使用用户名登录 `admin` 和在AEM初始启动期间选择的密码(通常 `admin`)。

## 2.安装示例内容 {#install-sample-content}

示例内容 **WKND参考站点** 用于加速教程。 WKND是一个虚构的生命风格品牌，通常与AEM培训一起使用。

WKND站点包括公开 [GraphQL端点](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). 在实际实施中，按照 [包括GraphQL端点](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) 在您的客户项目中。 A [CORS](#cors-config) 也被打包为WKND站点的一部分。 需要CORS配置才能授予对外部应用程序的访问权限，有关 [CORS](#cors-config) 可在下面找到。

1. 下载适用于WKND站点的最新编译的AEM包： [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > 确保下载与AEM as a Cloud Service兼容的标准版本，以及 **not** the `classic` 版本。

1. 从 **AEM开始** 菜单，导航至 **工具** > **部署** > **包**.

   ![导航到包](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. 单击 **上传包** 并选择在上一步骤中下载的WKND包。 单击&#x200B;**安装**&#x200B;可安装软件包。

1. 从 **AEM开始** 菜单，导航至 **资产** > **文件** > **WKND共享** > **英语** > **冒险**.

   ![Adventures的文件夹视图](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   这是构成WKND品牌推广的各种Adventures的所有资产的文件夹。 这包括传统媒体类型（如图像和视频），以及特定于AEM的媒体，如 **内容片段**.

1. 单击 **怀俄明州速降滑雪** 文件夹，然后单击 **怀俄明山滑雪内容片段** 卡片：

   ![内容片段卡](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. 内容片段编辑器将打开，用于下山滑雪怀俄明探险。

   ![内容片段编辑器](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   请注意， **标题**, **描述**&#x200B;和 **活动** 定义片段。

   **内容片段** 是在AEM中管理内容的一种方式。 内容片段是可重用的、与呈现形式无关的内容，由结构化数据元素（如文本、富文本、日期或对其他内容片段的引用）组成。 稍后在快速设置中会详细探索内容片段。

1. 单击 **取消** 来关闭片段。 您可以随意导航到其他一些文件夹，并浏览其他Adventure内容。

>[!NOTE]
>
> 如果使用Cloud Service环境，请参阅有关如何 [将类似WKND引用站点的代码库部署到Cloud Service环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3.下载并运行WKND React应用程序 {#sample-app}

本教程的目标之一是演示如何使用GraphQL API从外部应用程序中使用AEM内容。 本教程使用React应用程序的示例。 React应用程序特意非常简单，只注重与AEM GraphQL API的集成。

1. 打开新的命令提示符，并从GitHub中克隆示例React应用程序：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 在中打开React应用程序 `aem-guides-wknd-graphql/react-app` 在您选择的IDE中。
1. 在IDE中，打开文件 `.env.development` at `/.env.development`. 验证 `REACT_APP_AUTHORIZATION` 行将被取消注释，并且文件声明了以下变量：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   确保 `REACT_APP_HOST_URI` 指向本地AEM SDK。 为方便起见，此快速入门可将React应用程序连接到  **AEM作者**. **作者** 服务需要进行身份验证，因此应用程序使用 `admin` 用户建立其连接。 在开发过程中，将应用程序连接到AEM作者是一种常见做法，因为这样可以帮助快速迭代内容，而无需发布更改。

   >[!NOTE]
   >
   > 在生产方案中，应用程序将连接到AEM **发布** 环境。 有关详细信息，请参阅 _生产部署_ 中。


1. 安装并启动React应用程序：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新的浏览器窗口会自动在 [http://localhost:3000](http://localhost:3000).

   ![React启动应用程序](assets/quick-setup/aem-sdk/react-app__home-view.png)

   将显示AEM中的探险内容列表。

1. 单击其中一张冒险图像可查看冒险详细信息。 系统会向AEM发出请求，以返回冒险的详细信息。

   ![“冒险详细信息”视图](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. 使用浏览器的开发人员工具检查 **网络** 请求。 查看 **XHR** 请求和观察多个GET请求 `/graphql/execute.json/...`. 此路径前缀将调用AEM持久化查询端点，从而选择要使用前缀后面的名称和编码参数执行的持久化查询。

   ![GraphQL Endpoint XHR请求](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4.在AEM中编辑内容

运行React应用程序时，更新AEM中的内容，并查看该更改是否反映在应用程序中。

1. 导航到AEM [http://localhost:4502](http://localhost:4502).
1. 导航到 **资产** > **文件** > **WKND共享** > **英语** > **冒险** > **[巴厘岛冲浪营](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![巴厘岛冲浪营文件夹](assets/setup/bali-surf-camp-folder.png)

1. 单击 **巴厘岛冲浪营** 用于打开内容片段编辑器的内容片段。
1. 修改 **标题** 和 **描述** 冒险的。

   ![修改内容片段](assets/setup/modify-content-fragment-bali.png)

1. 单击 **保存** 以保存更改。
1. 在以下位置刷新React应用程序： [http://localhost:3000](http://localhost:3000) 要查看更改：

   ![更新了巴厘岛冲浪夏令营冒险](assets/setup/overnight-bali-surf-camp-changes.png)

## 5.浏览GraphiQL {#graphiql}

1. 打开 [GraphiQL](http://localhost:4502/aem/graphiql.html) 导航至 **工具** > **常规** > **GraphQL查询编辑器**
1. 选择左侧的现有持久化查询，然后运行它们以查看结果。

   >[!NOTE]
   >
   > GraphiQL工具和GraphQL API是 [稍后在教程中详细探讨了](../multi-step/explore-graphql-api.md).

## 恭喜！{#congratulations}

恭喜，您现在有一个外部应用程序正在使用AEM内容与GraphQL。 您可以随时在React应用程序中检查代码，并继续尝试修改现有的内容片段。

### 后续步骤

* [启动AEM Headless教程](../multi-step/overview.md)
