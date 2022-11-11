---
title: 为AEM Editor和远程SPA配置SPA
description: 需要AEM项目才能进行支持设置的配置和内容要求，以便AEM SPA Editor能够创作远程SPA。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 1%

---

# 为AEM编辑器配置SPA

虽然SPA代码库是在AEM之外管理的，但需要AEM项目才能设置支持配置和内容要求。 本章将介绍如何创建包含必要配置的AEM项目：

+ AEM WCM核心组件代理
+ AEM远程SPA页面代理
+ AEM远程SPA页面模板
+ 基线远程SPA AEM页面
+ 用于定义SPA到AEM URL映射的子项目
+ OSGi配置文件夹

## 从GitHub下载基础项目

下载 `aem-guides-wknd-graphql` 项目。 这将包含此项目中使用的一些基线文件。

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## 创建AEM项目

创建一个AEM项目，其中管理配置和基线内容。 此项目将在克隆内生成 `aem-guides-wknd-graphql` 项目的 `remote-spa-tutorial` 文件夹。

_始终使用 [AEM原型](https://github.com/adobe/aem-project-archetype)._

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_最后一个命令只需重命名AEM项目文件夹，这样就可以清楚地知道它是AEM项目，而不要与远程SPA混淆__

While `frontendModule="react"` ，则 `ui.frontend` 项目不用于远程SPA用例。 SPA是在AEM外部开发和管理的，仅将AEM用作内容API。 的 `frontendModule="react"` 项目需要标记，包括  `spa-project` AEM Java™依赖项并设置远程SPA页面模板。

AEM项目原型会生成以下元素，这些元素用于配置AEM以与SPA集成。

+ __AEM WCM核心组件代理__ at `ui.apps/src/.../apps/wknd-app/components`
+ __AEM SPA Remote Page代理__ at `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __AEM页面模板__ at `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __用于定义内容映射的子项目__ at `ui.content/src/...`
+ __基线远程SPA AEM页面__ at `ui.content/src/.../content/wknd-app`
+ __OSGi配置文件夹__ at `ui.config/src/.../apps/wknd-app/osgiconfig`

生成基本AEM项目后，进行一些调整可确保SPA编辑器与远程SPA兼容。

## 删除ui.frontend项目

由于SPA是远程SPA，因此假定它是在AEM项目之外开发和管理的。 要避免冲突，请删除 `ui.frontend` 项目。 如果 `ui.frontend` 项目、两个SPA(在 `ui.frontend` 项目和远程SPA在AEM SPA编辑器中同时加载。

1. 打开AEM项目(`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`)
1. 打开根 `pom.xml`
1. 注释 `<module>ui.frontend</module` 从 `<modules>` 列表

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   的 `pom.xml` 文件应该如下所示：

   ![从Reactor pom中删除ui.frontend模块](./assets/aem-project/uifrontend-reactor-pom.png)

1. 打开 `ui.apps/pom.xml`
1. 注释掉 `<dependency>` on `<artifactId>wknd-app.ui.frontend</artifactId>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   的 `ui.apps/pom.xml` 文件应该如下所示：

   ![从ui.apps中删除ui.frontend依赖项](./assets/aem-project/uifrontend-uiapps-pom.png)

如果AEM项目是在这些更改之前构建的，请手动删除 `ui.frontend` 从生成的客户端库 `ui.apps` 项目 `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## AEM内容映射

要使AEM在SPA编辑器中加载远程SPA，必须建立SPA路由和用于打开和创作内容的AEM页面之间的映射。

此配置的重要性稍后会进行探讨。

映射可通过 [Sling映射](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) 定义 `/etc/map`.

1. 在IDE中，打开 `ui.content` 子项目
1. 导航至  `src/main/content/jcr_root`
1. 创建文件夹 `etc`
1. 在 `etc`，创建文件夹 `map`
1. 在 `map`，创建文件夹 `http`
1. 在 `http`，创建文件 `.content.xml` 内容：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. 在 `http` ，创建文件夹 `localhost_any`
1. 在 `localhost_any`，创建文件 `.content.xml` 内容：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. 在 `localhost_any` ，创建文件夹 `wknd-app-routes-adventure`
1. 在 `wknd-app-routes-adventure`，创建文件 `.content.xml` 内容：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. 将映射节点添加到 `ui.content/src/main/content/META-INF/vault/filter.xml` 包含在AEM包中。

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

文件夹结构和 `.context.xml` 文件应该如下所示：

![Sling映射](./assets/aem-project/sling-mapping.png)

的 `filter.xml` 文件应该如下所示：

![Sling映射](./assets/aem-project/sling-mapping-filter.png)

现在，部署AEM项目时，会自动包含这些配置。

Sling映射效果AEM运行于 `http` 和 `localhost`，因此仅支持本地开发。 部署到AEMas a Cloud Service时，必须添加与目标类似的Sling映射 `https` 和相应的AEMas a Cloud Service域。有关更多信息，请参阅 [Sling映射文档](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## 跨源资源共享安全策略

接下来，配置AEM以保护内容，以便只有此SPA可以访问AEM内容。 配置 [AEM中的跨源资源共享](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. 在IDE中，打开 `ui.config` Maven子项目
1. 导航 `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. 创建名为 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. 将以下内容添加到文件中：

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

的 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` 文件应该如下所示：

![SPA编辑器CORS配置](./assets/aem-project/cors-configuration.png)

关键配置元素包括：

+ `alloworigin` 指定允许哪些主机从AEM检索内容。
   + `localhost:3000` 添加了以支持在本地运行的SPA
   + `https://external-hosted-app` 用作占位符，以替换为Remote SPA托管的域。
+ `allowedpaths` 指定此CORS配置涵盖AEM中的哪些路径。 默认设置允许访问AEM中的所有内容，但其范围只能为SPA可访问的特定路径，例如： `/content/wknd-app`.

## 将AEM页面设置为远程SPA页面模板

AEM项目原型会生成一个为AEM与远程SPA集成做准备的项目，但需要对自动生成的AEM页面结构进行细微而重要的调整。 自动生成的AEM页面必须将其类型更改为 __远程SPA页__，而不是 __SPA页面__.

1. 在IDE中，打开 `ui.content` 子项目
1. 打开到 `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. 更新此 `.content.xml` 文件：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

关键更改是 `jcr:content` 节点：

+ `cq:template` 到 `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` 到 `wknd-app/components/remotepage`

的 `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` 文件应该如下所示：

![主页.content.xml更新](./assets/aem-project/home-content-xml.png)

这些更改允许此页面(在AEM中充当SPA根)在SPA编辑器中加载远程SPA。

>[!NOTE]
>
>如果此项目之前已部署到AEM，请确保将AEM页面删除为 __站点> WKND应用程序>用户> en > WKND应用程序主页__，作为 `ui.content`  项目设置为 __合并__ 节点，而不是 __更新__.

此页面也可以在AEM中删除，并重新创建为远程SPA页面，但由于此页面是在 `ui.content` 项目最好在代码库中更新它。

## 将AEM项目部署到AEM SDK

1. 确保AEM创作服务在端口4502上运行
1. 从命令行中，导航到AEM Maven项目的根
1. 使用Maven将项目部署到本地AEM SDK创作服务

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## 配置根AEM页面

部署AEM项目后，需要最后一步来准备SPA Editor以加载我们的远程SPA。 在AEM中，标记与SPA根目录对应的AEM页面，`/content/wknd-app/us/en/home`，由AEM项目原型生成。

1. 登录到AEM作者
1. 导航到 __站点> WKND应用程序>美国> en__
1. 选择 __WKND应用程序主页__，然后点按 __属性__

   ![WKND应用程序主页 — 属性](./assets/aem-content/edit-home-properties.png)

1. 导航到 __SPA__ 选项卡
1. 填写 __远程SPA配置__
   + __SPA主机URL__: `http://localhost:3000`
      + 指向远程SPA根的URL

   ![WKND应用程序主页 — 远程SPA配置](./assets/aem-content/remote-spa-configuration.png)

1. 点按 __保存并关闭__

请记住，我们已将此页面的类型更改为 __远程SPA页__，这就是为了查看 __SPA__ 选项卡 __页面属性__.

此配置只能在与SPA根对应的AEM页面上设置。 此页面下方的所有AEM页面都将继承该值。

## 恭喜

您现在已准备好AEM配置并将其部署到本地AEM作者！ 您现在知道如何：

+ 通过注释掉AEM中的依赖项，删除生成的SPA项目原型 `ui.frontend`
+ 将Sling映射添加到AEM，以将SPA路由映射到AEM中的资源
+ 设置AEM跨源资源共享安全策略，以允许远程SPA使用AEM中的内容
+ 将AEM项目部署到本地AEM SDK创作服务
+ 使用AEM主机URL页面属性将SPA页面标记为远程SPA根

## 后续步骤

配置AEM后，我们可以 [引导远程SPA](./spa-bootstrap.md) 支持使用AEM SPA编辑器的可编辑区域！
