---
title: 使用AEM发布服务进行生产部署 — AEM无外设快速入门 — GraphQL
description: 了解AEM创作和发布服务以及针对无外设应用程序的推荐部署模式。 在本教程中，学习如何使用环境变量根据目标环境动态更改GraphQL端点。 了解如何正确配置AEM以实现跨来源资源共享(CORS)。
sub-product: 资产
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
translation-type: tm+mt
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '2361'
ht-degree: 1%

---


# 使用AEM发布服务进行生产部署

在本教程中，您将设置一个本地环境，以模拟从Author实例分发到Publish实例的内容。 您还将生成React App的生产构建，该应用程序配置为使用GraphQL API从AEM发布环境中使用内容。 在此过程中，您将学习如何有效使用环境变量以及如何更新AEM CORS配置。

## 前提条件

本教程是多部分教程的一部分。 假定已完成前几部分中概述的步骤。

## 目标

了解如何：

* 了解AEM作者和发布体系结构。
* 了解管理环境变量的最佳实践。
* 了解如何正确配置AEM以实现跨来源资源共享(CORS)。

## 作者发布部署模式{#deployment-pattern}

完整的AEM环境由作者、发布和调度程序组成。 创作服务是内部用户创建、管理和预览内容的场所。 发布服务被视为“实时”环境，通常是最终用户与之交互的内容。 内容在创作服务上经过编辑和批准后，将分发到发布服务。

AEM无外设应用程序的最常见部署模式是让应用程序的生产版本连接到AEM发布服务。

![高级部署模式](assets/publish-deployment/high-level-deployment.png)

上图描述了此常见部署模式。

1. **内容作者**&#x200B;使用AEM作者服务创建、编辑和管理内容。
2. **内容作者**&#x200B;和其他内部用户可以直接在创作服务上预览内容。 可以设置预览版本的应用程序，它连接到作者服务。
3. 内容获得批准后，即可&#x200B;**已发布**&#x200B;到AEM发布服务。
4. **最终** 用户与应用程序的生产版本交互。生产应用程序连接到发布服务，并使用GraphQL API请求和使用内容。

本教程通过向当前设置添加AEM发布实例来模拟上述部署。 在前几章中，React App通过直接连接到Author实例而充当预览。 React App的生产版本将部署到连接到新Publish实例的静态Node.js服务器。

最后，将运行三台本地服务器：

* http://localhost:4502 — 作者实例
* http://localhost:4503 - Publish实例
* http://localhost:5000 — 在生产模式下响应应用程序，连接到Publish实例。

## 安装AEM SDK — 发布模式{#aem-sdk-publish}

当前，我们在&#x200B;**作者**&#x200B;模式下有一个SDK正在运行的实例。 也可以在&#x200B;**发布**&#x200B;模式下启动SDK，以模拟AEM发布环境。

有关设置本地开发环境[的更详细指南，请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)。

1. 在本地文件系统上，创建一个专用文件夹以安装Publish实例，即名为`~/aem-sdk/publish`。
1. 复制前几章中用于Author实例的Quickstartjar文件，并将其粘贴到`publish`目录中。 或者，也可以导航到[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)并下载最新的SDK并解压Quickstartjar文件。
1. 将jar文件重命名为`aem-publish-p4503.jar`。

   `publish`字符串指定在“发布”模式下快速启动jar开始。 `p4503`指定Quickstart服务器在端口4503上运行。

1. 打开新的终端窗口，并导航到包含jar文件的文件夹。 安装和开始AEM实例：

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 请提供管理员密码作为`admin`。 任何管理员密码都是可以接受的，但建议使用本地开发的默认密码以避免出现额外的配置。
1. AEM实例安装完成后，将在[http://localhost:4503/content.html](http://localhost:4503/content.html)中打开一个新的浏览器窗口

   应返回“404未找到”页面。 这是全新的AEM实例，尚未安装任何内容。

## 安装示例内容和GraphQL端点{#wknd-site-content-endpoints}

与Author实例一样，Publish实例需要启用GraphQL终结点，并需要示例内容。 然后，在Publish实例上安装WKND参考站点。

1. 下载WKND站点的最新编译的AEM包：[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 确保下载与AEM兼容的标准版作为Cloud Service,**不**`classic`版本。

1. 直接导航到：[http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html)，用户名为`admin`，密码为`admin`。
1. 接下来，导航到[http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)的“包管理器”。
1. 单击&#x200B;**上载包**&#x200B;并选择在上一步中下载的WKND包。 单击&#x200B;**安装**&#x200B;以安装软件包。
1. 安装包后，WKND参考站点现在位于[http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html)。
1. 单击菜单栏中的“注销”按钮，以`admin`用户身份注销。

   ![WKND注销参考站点](assets/publish-deployment/sign-out-wknd-reference-site.png)

   与AEM作者实例不同，AEM发布实例默认为匿名只读访问。 我们希望在运行React应用程序时模拟匿名用户的体验。

## 更新环境变量以指向Publish实例{#react-app-publish}

接下来，更新React应用程序使用的环境变量以指向Publish实例。 React App应&#x200B;**仅**&#x200B;在生产模式下连接到Publish实例。

然后，添加一个新文件`.env.production.local`以模拟生产体验。

1. 在IDE中打开WKND GraphQL React应用程序。

1. 在`aem-guides-wknd-graphql/react-app`下，添加一个名为`.env.production.local`的文件。
1. 使用以下内容填充`.env.production.local`:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![添加新的环境变量文件](assets/publish-deployment/env-production-local-file.png)

   使用环境变量，无需在应用程序代码中添加额外的逻辑，即可在作者环境或发布应用程序之间轻松切换GraphQL端点。 有关[ React的自定义环境变量的详细信息，请访问](https://create-react-app.dev/docs/adding-custom-environment-variables)。

   >[!NOTE]
   >
   > 请注意，由于默认情况下“发布”环境提供对内容的匿名访问，因此不包含任何身份验证信息。

## 部署静态节点服务器{#static-server}

React应用程序可以使用webpack服务器启动，但仅用于开发。 接下来，通过使用[serve](https://github.com/vercel/serve)来使用Node.js托管React应用程序的生产构建，来模拟生产部署。

1. 打开新的终端窗口并导航到`aem-guides-wknd-graphql/react-app`目录

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 使用以下命令安装[serve](https://github.com/vercel/serve):

   ```shell
   $ npm install serve --save-dev
   ```

1. 在`react-app/package.json`打开文件`package.json`。 添加名为`serve`的脚本：

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   `serve`脚本执行两个操作。 首先，生成React App的生产构建。 其次，Node.js服务器开始并使用生产构建。

1. 返回到终端并输入命令以开始静态服务器：

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

1. 打开新浏览器并导航到[http://localhost:5000/](http://localhost:5000/)。 您应看到提供的React App。

   ![React App Served](assets/publish-deployment/react-app-served-port5000.png)

   请注意，GraphQL查询正在处理主页。 Inspect使用您的开发人员工具发出&#x200B;**XHR**&#x200B;请求。 请注意，GraphQLPOST是位于`http://localhost:4503/content/graphql/global/endpoint.json`的Publish实例。

   但是，主页上的所有图像都已损坏！

1. 单击其中一个“Adventure Detail”页面。

   ![冒险细节错误](assets/publish-deployment/adventure-detail-error.png)

   请注意，为`adventureContributor`引发了GraphQL错误。 在下一个练习中，修复了损坏的图像和`adventureContributor`问题。

## 绝对图像引用{#absolute-image-references}

图像显示为断开状态，因为`<img src`属性设置为相对路径，最后指向位于`http://localhost:5000/`的Node静态服务器。 这些图像应指向AEM发布实例。 有几种可能的解决办法。 使用webpack dev服务器时，文件`react-app/src/setupProxy.js`在webpack服务器和AEM作者实例之间为对`/content`的任何请求设置代理。 代理配置可以在生产环境中使用，但必须在Web服务器级别进行配置。 例如，[Apache的代理模块](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)。

可更新应用程序以包含使用`REACT_APP_HOST_URI`环境变量的绝对URL。 相反，让我们使用AEM GraphQL API的功能来请求图像的绝对URL。

1. 停止Node.js服务器。
1. 返回IDE并在`react-app/src/components/Adventures.js`打开文件`Adventures.js`。
1. 将`_publishUrl`属性添加到`allAdventuresQuery`中的`ImageRef`:

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

   `_publishUrl` 以 `_authorUrl` 及将值内置到 `ImageRef` 对象中，以便更轻松地包含绝对url。

1. 重复上述步骤以修改`filterQuery(activity)`函数中使用的查询，以包含`_publishUrl`属性。
1. 在构建`<img src=''>`标签时，修改`function AdventureItem(props)`处的`AdventureItem`组件以引用`_publishUrl`而不是`_path`属性：

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. 在`react-app/src/components/AdventureDetail.js`打开文件`AdventureDetail.js`。
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

1. 修改`AdventureDetail.js`中“Adventure Primary Image”和“Contributor Picture Reference”的两个`<img>`标签：

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

1. 返回终端并开始静态服务器：

   ```shell
   $ npm run serve
   ```

1. 导航到[http://localhost:5000/](http://localhost:5000/)并观察出现的图像以及`<img src''>`属性指向`http://localhost:4503`。

   ![已修复损坏的图像](assets/publish-deployment/broken-images-fixed.png)

## 模拟内容发布{#content-publish}

请回想，当请求“Adventure Details”（冒险详细信息）页面时，会为`adventureContributor`引发GraphQL错误。 **Contributor**&#x200B;内容片段模型在Publish实例中尚不存在。 对&#x200B;**Adventure**&#x200B;内容片段模型的更新也在Publish实例中不可用。 这些更改是直接对Author实例进行的，需要分发到Publish实例。

在向依赖内容片段或内容片段模型更新的应用程序推出新更新时，需要考虑这一点。

接下来，允许在本地作者实例和发布实例之间模拟内容发布。

1. 开始创作实例（如果尚未启动），并导航到[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)的包管理器
1. 下载包[EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip)并使用包管理器安装它。

   此包会安装一个配置，使创作实例能够将内容发布到发布实例。 [此配置的手动步骤可在此处](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution)找到。

   >[!NOTE]
   >
   > 在AEM作为Cloud Service环境中，作者层会自动设置为将内容分发到发布层。

1. 从&#x200B;**AEM开始**&#x200B;菜单，导航到&#x200B;**工具** > **资产** > **内容片段模型**。

1. 单击&#x200B;**WKND Site**&#x200B;文件夹。

1. 选择所有三种型号，然后单击&#x200B;**发布**:

   ![发布内容片段模型](assets/publish-deployment/publish-contentfragment-models.png)

   出现确认对话框，单击&#x200B;**发布**。

1. 导航到位于[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的Bali Surf Camp内容片段。

1. 单击顶部菜单栏中的&#x200B;**发布**&#x200B;按钮。

   ![在内容片段编辑器中单击“发布”按钮](assets/publish-deployment/publish-bali-content-fragment.png)

1. “发布”向导显示应发布的任何相关资产。 在这种情况下，将列出引用的片段&#x200B;**stacey-roswells**，并引用多个图像。 引用的资产会随片段一起发布。

   ![要发布的引用资产](assets/publish-deployment/referenced-assets.png)

   再次单击&#x200B;**发布**&#x200B;按钮以发布内容片段和相关资产。

1. 返回至运行在[http://localhost:5000/](http://localhost:5000/)的React App。 您现在可以点击巴厘岛冲浪夏令营，查看冒险的细节。

1. 切换回位于[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的AEM作者实例，并更新片段的&#x200B;**Title**。 **保存并** 关闭片段。然后，**发布**&#x200B;片段。
1. 返回至[http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)并观察已发布的更改。

   ![巴厘冲浪夏令营发布更新](assets/publish-deployment/bali-surf-camp-update.png)

## 更新COR配置

AEM默认是安全的，不允许非AEM web属性进行客户端调用。 AEM跨来源资源共享(CORS)配置允许特定域向AEM发出调用。

接下来，尝试AEM Publish实例的CORS配置。

1. 返回到使用命令`npm run serve`运行React App的终端窗口：

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

1. 导航到以[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)开头的地址。 地址对于每台本地计算机将略有不同。 请注意，获取数据时出现CORS错误。 这是因为当前CORS配置仅允许来自`localhost`的请求。

   ![CORS错误](assets/publish-deployment/cors-error-not-fetched.png)

   然后，更新AEM发布CORS配置以允许来自网络IP地址的请求。

1. 导航到[http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html)并使用用户名`admin`和密码`admin`登录。

1. 导航到[http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr)并在`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`找到WKND GraphQL配置。

1. 更新&#x200B;**允许的来源**&#x200B;字段以包含网络IP地址：

   ![更新CORS配置](assets/publish-deployment/cors-update.png)

   还可以包括允许来自特定子域的所有请求的常规表达式。 保存更改。

1. 搜索&#x200B;**Apache Sling推荐人过滤器**&#x200B;并查看配置。 还需要&#x200B;**允许空**&#x200B;配置来启用来自外部域的GraphQL请求。

   ![Sling 引用过滤器](assets/publish-deployment/sling-referrer-filter.png)

   这些组件已配置为WKND参考站点的一部分。 您可以通过[GitHub存储库](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)视图全套OSGi配置。

   >[!NOTE]
   >
   > OSGi配置在提交到源代码控制的AEM项目中进行管理。 可以使用Cloud Manager将AEM项目部署到AEM作为Cloud Service环境。 [AEM项目原型](https://github.com/adobe/aem-project-archetype)可以帮助生成特定实施的项目。

1. 返回至以[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)开头的React应用程序，并观察应用程序不再引发CORS错误。

   ![更正了CORS错误](assets/publish-deployment/cors-error-corrected.png)

## 恭喜！{#congratulations}

恭喜！ 您现在已使用AEM发布环境模拟完整生产部署。 您还学习了如何在AEM中使用CORS配置。

## 其他资源

有关内容片段和GraphQL的更多详细信息，请参阅以下资源：

* [使用内容片段和GraphQL的无外设内容投放](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API，与内容片段一起使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [将代码作为Cloud Service部署到AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
