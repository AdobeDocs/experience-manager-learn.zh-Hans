---
title: 使用AEM发布服务的生产部署 — AEM无头入门 — GraphQL
description: 了解AEM创作和发布服务以及无头应用程序的推荐部署模式。 在本教程中，了解如何使用环境变量根据目标环境动态更改GraphQL端点。 了解如何正确配置AEM以进行跨域资源共享(CORS)。
version: cloud-service
feature: 内容片段， GraphQL API
topic: 无外设、内容管理
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2367'
ht-degree: 1%

---


# 使用AEM发布服务进行生产部署

在本教程中，您将设置一个本地环境，以模拟从创作实例分发到发布实例的内容。 您还将生成React应用程序的生产内部版本，该应用程序配置为使用GraphQL API从AEM发布环境中使用内容。 在此过程中，您将了解如何有效使用环境变量以及如何更新AEM CORS配置。

## 前提条件

本教程是多部分教程的一部分。 假定已完成前几部分中概述的步骤。

## 目标

了解如何：

* 了解AEM创作和发布架构。
* 了解管理环境变量的最佳实践。
* 了解如何正确配置AEM以进行跨域资源共享(CORS)。

## 创作发布部署模式 {#deployment-pattern}

完整的AEM环境由创作、发布和调度程序组成。 “创作”服务是内部用户创建、管理和预览内容的位置。 发布服务被视为“实时”环境，通常是最终用户与之交互的环境。 内容在创作服务上进行编辑和批准后，会分发到发布服务。

AEM无头应用程序的最常见部署模式是，将应用程序的生产版本连接到AEM发布服务。

![高级部署模式](assets/publish-deployment/high-level-deployment.png)

上图描述了此常见部署模式。

1. **内容作者**&#x200B;使用AEM创作服务创建、编辑和管理内容。
2. **内容作者**&#x200B;和其他内部用户可以直接在创作服务上预览内容。 可以设置应用程序的预览版本，以连接到创作服务。
3. 内容获得批准后，即可&#x200B;**已发布**&#x200B;到AEM发布服务。
4. **最终** 用户与应用程序的生产版本进行交互。生产应用程序连接到发布服务，并使用GraphQL API请求和使用内容。

本教程通过将AEM Publish实例添加到当前设置来模拟上述部署。 在前几章中，React应用程序通过直接连接到创作实例来充当预览。 React应用程序的生产内部版本将部署到连接到新发布实例的静态Node.js服务器。

最后，将运行三台本地服务器：

* http://localhost:4502 — 创作实例
* http://localhost:4503 — 发布实例
* http://localhost:5000 — 在生产模式下， React应用程序连接到发布实例。

## 安装AEM SDK — 发布模式 {#aem-sdk-publish}

当前，我们在&#x200B;**创作**&#x200B;模式下有一个正在运行的SDK实例。 也可以在&#x200B;**发布**&#x200B;模式下启动SDK，以模拟AEM发布环境。

有关设置本地开发环境[的更详细指南，请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)。

1. 在本地文件系统上，创建一个专用文件夹以安装Publish实例，即`~/aem-sdk/publish`。
1. 复制前几章中用于创作实例的快速入门Jar文件，并将其粘贴到`publish`目录中。 或者，导航到[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)并下载最新的SDK并解压快速入门Jar文件。
1. 将jar文件重命名为`aem-publish-p4503.jar`。

   `publish`字符串指定快速入门Jar以“发布”模式启动。 `p4503`指定快速启动服务器在端口4503上运行。

1. 打开新的“终端”窗口，然后导航到包含jar文件的文件夹。 安装并启动AEM实例：

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 提供管理员密码作为`admin`。 可接受任何管理员密码，但建议将默认密码用于本地开发，以避免额外配置。
1. 安装完AEM实例后，将在[http://localhost:4503/content.html](http://localhost:4503/content.html)中打开一个新的浏览器窗口

   应会返回“404未找到”页面。 这是一个全新的AEM实例，尚未安装任何内容。

## 安装示例内容和GraphQL端点 {#wknd-site-content-endpoints}

与创作实例一样，发布实例需要启用GraphQL端点，并且需要示例内容。 接下来，在发布实例上安装WKND引用站点。

1. 下载适用于WKND站点的最新编译的AEM包：[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 确保下载与AEM as a Cloud Service兼容的标准版本，而&#x200B;**不**&#x200B;的`classic`版本。

1. 通过直接导航到以下位置，登录到Publish实例：[http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html)，用户名`admin`，密码`admin`。
1. 接下来，导航到位于[http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)的包管理器。
1. 单击&#x200B;**上传包** ，然后选择在上一步骤中下载的WKND包。 单击&#x200B;**Install**&#x200B;以安装包。
1. 安装包后，现在可在[http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html)上找到WKND引用站点。
1. 单击菜单栏中的“注销”按钮，以`admin`用户身份注销。

   ![WKND注销参考站点](assets/publish-deployment/sign-out-wknd-reference-site.png)

   与AEM创作实例不同，AEM发布实例默认使用匿名只读访问。 我们希望在运行React应用程序时模拟匿名用户的体验。

## 更新环境变量以指向发布实例 {#react-app-publish}

接下来，更新React应用程序使用的环境变量以指向Publish实例。 React应用程序应该在生产模式下&#x200B;**仅**&#x200B;连接到Publish实例。

接下来，添加新文件`.env.production.local`以模拟生产体验。

1. 在IDE中打开WKND GraphQL React应用程序。

1. 在`aem-guides-wknd-graphql/react-app`下，添加一个名为`.env.production.local`的文件。
1. 使用以下内容填充`.env.production.local` :

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![添加新的环境变量文件](assets/publish-deployment/env-production-local-file.png)

   通过使用环境变量，可以轻松地在创作或发布环境之间切换GraphQL端点，而无需在应用程序代码中添加额外的逻辑。 有关[React自定义环境变量的更多信息，请参阅此处](https://create-react-app.dev/docs/adding-custom-environment-variables)。

   >[!NOTE]
   >
   > 请注意，由于发布环境默认提供对内容的匿名访问，因此未包含任何身份验证信息。

## 部署静态节点服务器 {#static-server}

React应用程序可以使用Webpack服务器启动，但仅供开发使用。 接下来，使用[serve](https://github.com/vercel/serve)模拟生产部署，以使用Node.js托管React应用程序的生产内部版本。

1. 打开新的终端窗口并导航到`aem-guides-wknd-graphql/react-app`目录

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 使用以下命令安装[serve](https://github.com/vercel/serve):

   ```shell
   $ npm install serve --save-dev
   ```

1. 在`react-app/package.json`处打开文件`package.json`。 添加名为`serve`的脚本：

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   `serve`脚本执行两个操作。 首先，生成React应用程序的生产内部版本。 其次，Node.js服务器启动并使用生产内部版本。

1. 返回到终端并输入启动静态服务器的命令：

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. 打开新浏览器并导航到[http://localhost:5000/](http://localhost:5000/)。 您应会看到正在提供React应用程序。

   ![React App Served](assets/publish-deployment/react-app-served-port5000.png)

   请注意，GraphQL查询在主页上工作。 使用开发人员工具Inspect **XHR**&#x200B;请求。 请注意，GraphQLPOST位于`http://localhost:4503/content/graphql/global/endpoint.json`的Publish实例。

   但是，主页上的所有图像都损坏了！

1. 单击其中一个“Adventure Detail”页面。

   ![冒险详细信息错误](assets/publish-deployment/adventure-detail-error.png)

   观察是否引发了`adventureContributor`的GraphQL错误。 在接下来的练习中，修复了损坏的图像和`adventureContributor`问题。

## 绝对图像引用 {#absolute-image-references}

图像显示为已损坏，因为`<img src`属性被设置为相对路径，并最终指向位于`http://localhost:5000/`的Node静态服务器。 这些图像而应该指向AEM发布实例。 这有几个潜在的解决方案。 使用WebPack开发服务器时，文件`react-app/src/setupProxy.js`会在WebPack服务器和AEM创作实例之间为对`/content`的任何请求设置代理。 代理配置可以在生产环境中使用，但必须在Web服务器级别进行配置。 例如， [Apache的代理模块](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)。

可以更新应用程序以包含使用`REACT_APP_HOST_URI`环境变量的绝对URL。 我们改用AEM GraphQL API的功能来请求图像的绝对URL。

1. 停止Node.js服务器。
1. 返回到IDE并在`react-app/src/components/Adventures.js`处打开文件`Adventures.js`。
1. 将`_publishUrl`属性添加到`allAdventuresQuery`内的`ImageRef`:

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` 和是 `_authorUrl` 内置到对象中的 `ImageRef` 值，可更轻松地包含绝对url。

1. 重复上述步骤以修改`filterQuery(activity)`函数中使用的查询，以包含`_publishUrl`属性。
1. 在`function AdventureItem(props)`修改`AdventureItem`组件，以在构建`<img src=''>`标记时引用`_publishUrl`而不是`_path`属性：

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. 在`react-app/src/components/AdventureDetail.js`处打开文件`AdventureDetail.js`。
1. 重复相同步骤以修改GraphQL查询，并为Adventure添加`_publishUrl`属性

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. 修改`AdventureDetail.js`中冒险主图像和参与者图片引用的两个`<img>`标记：

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. 返回到终端并启动静态服务器：

   ```shell
   $ npm run serve
   ```

1. 导航到[http://localhost:5000/](http://localhost:5000/)，并观察显示的图像以及`<img src''>`属性指向`http://localhost:4503`。

   ![已修复损坏的图像](assets/publish-deployment/broken-images-fixed.png)

## 模拟内容发布 {#content-publish}

请记住，在请求“探险详细信息”页面时，会为`adventureContributor`引发GraphQL错误。 Publish实例上尚不存在&#x200B;**Contributor**&#x200B;内容片段模型。 对&#x200B;**Adventure**&#x200B;内容片段模型的更新也在Publish实例中不可用。 这些更改直接发生在创作实例中，需要分发到发布实例。

对依赖于内容片段或内容片段模型更新的应用程序推出新更新时，需要考虑这一点。

接下来，允许在本地创作实例和发布实例之间模拟内容发布。

1. 启动创作实例（如果尚未启动），然后导航到位于[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)的包管理器
1. 下载包[EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip)并使用包管理器进行安装。

   此包将安装一个配置，该配置允许创作实例将内容发布到发布实例。 [此配置的手动步骤可在此处](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution)找到。

   >[!NOTE]
   >
   > 在AEM as a Cloud Service环境中，创作层会自动设置为将内容分发到发布层。

1. 从&#x200B;**AEM开始**&#x200B;菜单中，导航到&#x200B;**工具** > **资产** > **内容片段模型**。

1. 单击&#x200B;**WKND Site**&#x200B;文件夹。

1. 选择所有三个模型，然后单击&#x200B;**Publish**:

   ![发布内容片段模型](assets/publish-deployment/publish-contentfragment-models.png)

   出现确认对话框，单击&#x200B;**Publish**。

1. 导航到位于[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的Bali Surf Camp内容片段。

1. 单击顶部菜单栏中的&#x200B;**Publish**&#x200B;按钮。

   ![在内容片段编辑器中单击发布按钮](assets/publish-deployment/publish-bali-content-fragment.png)

1. “发布”向导会显示应发布的任何从属资产。 在本例中，列出了引用的片段&#x200B;**stacey-roswells**，还引用了多个图像。 引用的资产与片段一起发布。

   ![要发布的引用资产](assets/publish-deployment/referenced-assets.png)

   再次单击&#x200B;**发布**&#x200B;按钮以发布内容片段和从属资产。

1. 返回到在[http://localhost:5000/](http://localhost:5000/)运行的React应用程序。 您现在可以点击巴厘岛冲浪营地，查看冒险的详细信息。

1. 切换回位于[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的AEM创作实例，并更新片段的&#x200B;**Title**。 **保存并关** 闭片段。然后， **发布**&#x200B;片段。
1. 返回到[http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)并观察已发布的更改。

   ![巴厘岛冲浪营发布更新](assets/publish-deployment/bali-surf-camp-update.png)

## 更新COR配置

AEM默认是安全的，不允许非AEM web属性进行客户端调用。 AEM跨域资源共享(CORS)配置允许特定域调用AEM。

接下来，尝试使用AEM发布实例的CORS配置。

1. 返回到React App使用命令`npm run serve`运行的终端窗口：

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   请注意提供了两个URL。 一个使用`localhost`，另一个使用本地网络IP地址。

1. 导航到以[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)开始的地址。 每个本地计算机的地址将略有不同。 请注意，获取数据时出现CORS错误。 这是因为当前的CORS配置仅允许来自`localhost`的请求。

   ![CORS错误](assets/publish-deployment/cors-error-not-fetched.png)

   接下来，更新AEM发布CORS配置，以允许来自网络IP地址的请求。

1. 导航到[http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html)，然后使用用户名`admin`和密码`admin`登录。

1. 导航到[http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr)并在`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`中找到WKND GraphQL配置。

1. 更新&#x200B;**允许的源**&#x200B;字段以包含网络IP地址：

   ![更新CORS配置](assets/publish-deployment/cors-update.png)

   还可以包含允许来自特定子域的所有请求的正则表达式。 保存更改。

1. 搜索&#x200B;**Apache Sling反向链接过滤器**&#x200B;并查看配置。 还需要&#x200B;**允许空**&#x200B;配置才能从外部域启用GraphQL请求。

   ![Sling 引用过滤器](assets/publish-deployment/sling-referrer-filter.png)

   这些引用已配置为WKND引用站点的一部分。 您可以通过[GitHub存储库](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)查看OSGi配置的完整集。

   >[!NOTE]
   >
   > OSGi配置在提交到源控制的AEM项目中进行管理。 可以使用Cloud Manager将AEM项目部署到AEM作为Cloud Service环境。 [AEM项目原型](https://github.com/adobe/aem-project-archetype)可帮助为特定实施生成项目。

1. 返回到以[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)开始的React应用程序，并观察该应用程序不再引发CORS错误。

   ![更正了CORS错误](assets/publish-deployment/cors-error-corrected.png)

## 恭喜！ {#congratulations}

恭喜！ 您现在已使用AEM发布环境模拟完整的生产部署。 您还学习了如何在AEM中使用CORS配置。

## 其他资源

有关内容片段和GraphQL的更多详细信息，请参阅以下资源：

* [通过GraphQL使用内容片段交付无头内容](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API，用于内容片段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [将代码部署到AEM as aCloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
