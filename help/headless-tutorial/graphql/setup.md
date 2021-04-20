---
title: 快速设置 — AEM无外设快速入门 — GraphQL
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 安装AEM SDK，添加示例内容并部署使用AEM GraphQL API读取内容的应用程序。 了解AEM如何为全渠道体验提供支持。
sub-product: 站点
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 1%

---


# 快速设置{#setup}

本章优惠本地环境的快速设置，以查看外部应用程序使用AEM GraphQL API从AEM中使用内容。 本教程的后几章将基于此设置。

## 前提条件 {#prerequisites}

以下工具应安装在本地：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3E&amp;orderby%3content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=列表&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 目标{#objectives}

1. 下载并安装AEM SDK。
1. 从WKND参考站点下载并安装示例内容。
1. 下载并安装一个范例应用程序以使用GraphQL API使用内容。

## 安装AEM SDK {#aem-sdk}

本教程使用[AEM作为Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk)来浏览AEM GraphQL API。 本节提供有关安装AEM SDK并在创作模式下运行它的快速指南。 有关设置本地开发环境[的更详细指南，请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)。

>[!NOTE]
>
> 还可以通过AEM作为Cloud Service环境来学习教程。 本教程中包含有关使用云环境的其他说明。

1. 导航到&#x200B;**[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM作为Cloud Service**，并下载最新版的&#x200B;**AEM SDK**。

   ![软件分发门户](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > 默认情况下，仅在AEM SDK上启用GraphQL功能(2021-02-04或更高版本)。

1. 解压缩下载并将快速启动程序jar(`aem-sdk-quickstart-XXX.jar`)复制到专用文件夹，即`~/aem-sdk/author`。
1. 将jar文件重命名为`aem-author-p4502.jar`。

   `author`名称指定快速启动程序jar将在“作者”模式下开始。 `p4502`指定Quickstart服务器将在端口4502上运行。

1. 打开新的终端窗口，并导航到包含jar文件的文件夹。 运行以下命令以安装和开始AEM实例：

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 请提供管理员密码作为`admin`。 任何管理员密码都是可以接受的，但是，它建议使用本地开发的默认密码来减少重新配置的需要。
1. 几分钟后，AEM实例将完成安装，并将在[http://localhost:4502](http://localhost:4502)打开一个新的浏览器窗口。
1. 使用用户名`admin`和密码`admin`登录。

## 安装示例内容和GraphQL端点{#wknd-site-content-endpoints}

将安装&#x200B;**WKND参考站点**&#x200B;中的示例内容以加速教程。 WKND是一个虚构的生活风格品牌，经常与AEM培训结合使用。

WKND参考站点包括公开[GraphQL端点](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint)所需的配置。 在实际实施中，请按照文档中的步骤操作，[在客户项目中包含GraphQL端点](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint)。 [CORS](#cors-config)也已作为WKND站点的一部分打包。 需要CORS配置才能授予对外部应用程序的访问权，有关[CORS](#cors-config)的更多信息可在下面找到。

1. 下载WKND站点的最新编译的AEM包：[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 确保下载与AEM兼容的标准版作为Cloud Service,**不**`classic`版本。

1. 从&#x200B;**AEM 开始**&#x200B;菜单导航到&#x200B;**工具** > **部署** > **软件包**。

   ![导航到包](assets/setup/navigate-to-packages.png)

1. 单击&#x200B;**上载包**&#x200B;并选择在上一步中下载的WKND包。 单击&#x200B;**安装**&#x200B;以安装软件包。

1. 从&#x200B;**AEM 开始**&#x200B;菜单导航到&#x200B;**资产** > **文件**。
1. 单击文件夹可导航到&#x200B;**WKND Site** > **English** > **Adventures**。

   ![文件夹视图](assets/setup/folder-view-adventures.png)

   这是由WKND品牌推广的各种Adventures组成的所有资产的文件夹。 这包括图像和视频等传统媒体类型，以及&#x200B;**内容片段**&#x200B;等特定于AEM的媒体。

1. 单击&#x200B;**Showning Wyoming**&#x200B;文件夹，然后单击&#x200B;**Showning Skiing Wyoming内容片段**&#x200B;卡：

   ![下游滑雪内容片段卡](assets/setup/downhill-skiing-cntent-fragment.png)

1. 内容片段编辑器UI将打开，用于下山滑雪怀俄明探险。

   ![速降滑雪内容片段](assets/setup/down-hillskiing-fragment.png)

   请注意，**Title**、**Description**&#x200B;和&#x200B;**活动**&#x200B;等各种字段定义片段。

   **内** 容片段是在AEM中管理内容的一种方式。内容片段是可重用的、不可知表示的内容，由结构化数据元素（如文本、富文本、日期或对其他内容片段的引用）组成。 在教程的稍后部分将进一步探讨内容片段。

1. 单击&#x200B;**取消**&#x200B;以关闭片段。 您可以随意浏览到其他文件夹，并浏览其他Adventure内容。

>[!NOTE]
>
> 如果使用Cloud Service环境，请参阅有关如何[将代码库（如WKND引用站点）部署到Cloud Service环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#deploying)的文档。

## 安装示例应用程序{#sample-app}

本教程的目标之一是说明如何使用GraphQL API从外部应用程序使用AEM内容。 本教程使用已部分完成的React App示例来加速教程。 同样的经验和概念也适用于使用iOS、Android或任何其他平台构建的应用程序。 React应用程序有意地很简单，以避免不必要的复杂性；它不是一个参考实施。

1. 使用Git打开新的终端窗口和克隆教程启动器分支：

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在您选择的IDE中，打开位于`aem-guides-wknd-graphql/react-app/.env.development`的文件`.env.development`。 验证`REACT_APP_AUTHORIZATION`行是否未加注释，且文件如下所示：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   确保`React_APP_HOST_URI`与本地AEM实例匹配。 在本章中，我们将直接将React App连接到AEM **Author**&#x200B;环境。 **默** 认情况下，授权需要身份验证，因此我们的应用程序将以用户身份 `admin` 连接。这是开发过程中的常见做法，因为它允许我们快速更改AEM环境并立即在应用程序中看到它们。

   >[!NOTE]
   >
   > 在生产方案中，应用程序将连接到AEM **Publish**&#x200B;环境。 这在[生产部署](production-deployment.md)一章中有更详细的介绍。

1. 导览至`aem-guides-wknd-graphql/react-app`文件夹。 安装和开始应用程序：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新的浏览器窗口应自动启动位于[http://localhost:3000](http://localhost:3000)的应用程序。

   ![React starter app](assets/setup/react-starter-app.png)

   应当显示AEM中当前Adventure内容的列表。

1. 单击其中一张冒险图像，视图冒险精神。 会向AEM发出请求，要求其返回冒险的详细信息。

   ![冒险详细信息视图](assets/setup/adventure-details-view.png)

1. 使用浏览器的开发人员工具检查&#x200B;**Network**&#x200B;请求。 视图&#x200B;**XHR**&#x200B;请求并观察对为AEM配置的GraphQL端点`/content/graphql/global/endpoint.json`的多个POST请求。

   ![GraphQL端点XHR请求](assets/setup/endpoint-gql.png)

1. 您还可以通过检查网络请求来视图参数和JSON响应。 为Chrome安装浏览器扩展（如[GraphQL Network](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm)），以更好地了解查询和响应可能会有所帮助。

   ![GraphQL网络扩展](assets/setup/GraphQL-extension.png)

   *使用Chrome扩展图QL网络*

## 修改内容片段

现在React应用程序正在运行，请更新AEM中的内容，并查看应用程序中反映的更改。

1. 导航到AEM [http://localhost:4502](http://localhost:4502)。
1. 导航至&#x200B;**资产**>**文件**>**WKND站点**>**英语****冒险**>**[巴厘岛冲浪营地](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**。

   ![巴厘岛冲浪营文件夹](assets/setup/bali-surf-camp-folder.png)

1. 单击&#x200B;**Bali Surf Camp**&#x200B;内容片段以打开内容片段编辑器。
1. 修改冒险的&#x200B;**标题**&#x200B;和&#x200B;**说明**

   ![修改内容片段](assets/setup/modify-content-fragment-bali.png)

1. 单击&#x200B;**保存**&#x200B;以保存更改。
1. 导航回位于[http://localhost:3000](http://localhost:3000)的React应用程序并刷新以查看您的更改：

   ![更新的巴厘岛冲浪夏令营冒险](assets/setup/overnight-bali-surf-camp-changes.png)

## 安装GraphiQL工具{#install-graphiql}

[GraphiQL](https://github.com/graphql/graphiql) 是一款开发工具，仅在开发或本地实例等低级环境上需要。GraphiQL IDE允许您快速测试和优化返回的查询和数据。 GraphiQL还提供了对文档的轻松访问，使您能轻松了解和了解可用的方法。

1. 导航到&#x200B;**[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM作为Cloud Service**。
1. 搜索“GraphiQL”(请确保在&#x200B;**GraphiQL**&#x200B;中包含&#x200B;**i**。
1. 下载最新的&#x200B;**GraphiQL内容包v.x.x.x**

   ![下载GraphiQL包](assets/explore-graphql-api/software-distribution.png)

   zip文件是可直接安装的AEM包。

1. 从&#x200B;**AEM 开始**&#x200B;菜单导航到&#x200B;**工具** > **部署** > **软件包**。
1. 单击&#x200B;**上载包**，然后选择在上一步中下载的包。 单击&#x200B;**安装**&#x200B;以安装软件包。

   ![安装GraphiQL包](assets/explore-graphql-api/install-graphiql-package.png)
1. 导航到位于[http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)的GraphiQL IDE，然后开始探索GraphQL API。

   >[!NOTE]
   >
   > 在教程](./explore-graphql-api.md)的后面，将更详细地探讨GraphiQL工具和GraphQL API。[

## 恭喜！{#congratulations}

恭喜您，您现在有一个外部应用程序使用GraphQL的AEM内容。 您可以检查React应用程序中的代码，并继续尝试修改现有内容片段。

## 后续步骤{#next-steps}

在下一章[定义内容片段模型](content-fragment-models.md)中，了解如何使用&#x200B;**内容片段模型**&#x200B;建模内容和构建模式。 您将检查现有模型并创建新模型。 您还将了解可用于定义模式作为模型一部分的不同数据类型。

## （额外）CORS配置{#cors-config}

AEM在默认情况下是安全的，它阻止跨来源请求，防止未经授权的应用程序连接到其内容并呈现其内容。

为了允许本教程的React应用程序与AEM GraphQL API端点进行交互，已在WKND站点参考项目中定义了跨来源资源共享配置。

![跨来源资源共享配置](assets/setup/cross-origin-resource-sharing-configuration.png)

要视图已部署的配置：

1. 导航到AEM SDK的Web控制台，网址为[http://localhost:4502/system/console](http://localhost:4502/system/console)。

   >[!NOTE]
   >
   > Web控制台仅在SDK上可用。 在AEM作为Cloud Service环境中，可以通过[开发人员控制台](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html)查看此信息。

1. 在顶部菜单中，单击&#x200B;**OSGI** > **Configuration**&#x200B;以调出所有[OSGi Configurations](http://localhost:4502/system/console/configMgr)。
1. 向下滚动页面&#x200B;**AdobeGranite跨来源资源共享**。
1. 单击`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`的配置。
1. 以下字段已更新：
   * 允许的来源（正则表达式）：`http://localhost:.*`
      * 允许所有本地主机连接。
   * 允许的路径: `/content/graphql/global/endpoint.json`
      * 这是当前配置的唯一GraphQL终结点。 作为最佳做法，核心组织的配置应尽可能限制。
   * 允许的方法：`GET`、`HEAD`、`POST`
      * GraphQL只需要`POST`，但是，当以无头方式与AEM交互时，其他方法可能很有用。
   * 支持的标题：已添加&#x200B;**authorization**&#x200B;以在创作环境上传递基本身份验证。
   * 支持凭据：`Yes`
      * 这是必需的，因为我们的React应用程序将与AEM作者服务上受保护的GraphQL端点进行通信。

此配置和GraphQL端点是AEM WKND项目的一部分。 您可以在此处视图所有[OSGi配置](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)。
