---
title: 配置AEM for SPA Editor和远程SPA
description: 需要AEM项目才能设置支持配置和内容要求，以允许AEM SPA Editor创作远程SPA。
topic: 无外设、SPA、开发
feature: SPA编辑器，核心组件， API，开发
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1220'
ht-degree: 1%

---


# 配置AEM for SPA Editor

在AEM外部管理SPA代码库时，需要AEM项目来设置支持配置和内容要求。 本章将介绍如何创建包含必要配置的AEM项目：

+ AEM WCM核心组件代理
+ AEM Remote SPA Page代理
+ AEM Remote SPA页面模板
+ 基线远程SPA AEM页
+ 子项目，用于定义SPA到AEM URL映射
+ OSGi配置文件夹

## 创建AEM项目

创建管理配置和基准内容的AEM项目。

_始终使用最新版 [AEM Archetype](https://github.com/adobe/aem-project-archetype)。_


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_最后一个命令只需重命名AEM项目文件夹即可，很明显它是AEM项目，不要与远程SPA混淆__

指定`frontendModule="react"`时，`ui.frontend`项目不用于远程SPA用例。 SPA是在AEM的外部开发和管理的，仅将AEM用作内容API。 项目包含`spa-project` AEM Java™依赖项并设置远程SPA页面模板时，`frontendModule="react"`标志是必需的。

AEM项目原型可生成以下用于配置AEM以与SPA集成的元素。

+ __AEM WCM核心组件代理__ 服务  `ui.content/src/.../apps/wknd-app/components`
+ __AEM SPA Remote Page__ proxyat  `ui.content/src/.../apps/wknd-app/components/remotepage`
+ __AEM页面模__ 板  `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __用于定义内容映射的子项__ 目  `ui.content/src/...`
+ __基线远程SPA AEM页__ 面  `ui.content/src/.../content/wknd-app`
+ __OSGi配置文__ 件夹  `ui.config/src/.../apps/wknd-app/osgiconfig`

生成基本AEM项目后，进行一些调整可确保SPA Editor与远程SPA兼容。

## 删除ui.frontend项目

由于SPA是远程SPA，因此假定它是在AEM项目之外开发和管理的。 要避免冲突，请从部署中删除`ui.frontend`项目。 如果未删除`ui.frontend`项目，则将在AEM SPA编辑器中同时加载两个SPA(在`ui.frontend`项目中提供的默认SPA和远程SPA)。

1. 在IDE中打开AEM项目(`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`)
1. 打开根`pom.xml`
1. 从`<modules>`列表注释`<module>ui.frontend</module`

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

   `pom.xml`文件应该如下：

   ![从反应器pom中删除ui.frontend模块](./assets/aem-project/uifrontend-reactor-pom.png)

1. 打开`ui.apps/pom.xml`
1. 注释掉`<artifactId>wknd-app.ui.frontend</artifactId>`上的`<dependency>`

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

   `ui.apps/pom.xml`文件应该如下：

   ![从ui.apps中删除ui.frontend依赖项](./assets/aem-project/uifrontend-uiapps-pom.png)

如果AEM项目是在这些更改之前构建的，请从`ui.apps`项目(`ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`)中手动删除`ui.frontend`生成的客户端库。

## AEM内容映射

要使AEM在SPA编辑器中加载远程SPA，必须建立SPA路由和用于打开和创作内容的AEM页面之间的映射。

稍后将探讨此配置的重要性。

映射可以使用在`/etc/map`中定义的[Sling映射](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1)完成。

1. 在IDE中，打开`ui.content`子项目
1. 导航至  `src/main/content/jcr_root/etc`
1. 创建文件夹`map`
1. 在`map`中，创建一个文件夹`http`
1. 在`http`中，创建包含以下内容的文件`.content.xml`:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. 在`http`中，创建一个文件夹`localhost_any`
1. 在`localhost_any`中，创建包含以下内容的文件`.content.xml`:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. 在`localhost_any`中，创建一个文件夹`wknd-app-routes-adventure`
1. 在`wknd-app-routes-adventure`中，创建包含以下内容的文件`.content.xml`:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages will be created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. 将映射节点添加到AEM包中包含的`ui.content/src/main/content/META-INF/vault/filter.xml`中。

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

文件夹结构和`.context.xml`文件应当如下：

![Sling映射](./assets/aem-project/sling-mapping.png)

`filter.xml`文件应该如下：

![Sling映射](./assets/aem-project/sling-mapping-filter.png)

现在，部署AEM项目时，会自动包括这些配置。

Sling Mapping效果AEM运行在`http`和`localhost`上，因此仅支持本地开发。 当部署到AEM作为Cloud Service时，必须将类似的Sling映射添加到目标`https`和相应的AEM作为Cloud Service域。 有关详细信息，请参阅[Sling映射文档](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)。

## 跨来源资源共享安全策略

接下来，配置AEM以保护内容，以便只有此SPA才能访问AEM内容。 C在AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html)中配置[跨来源资源共享。

1. 在IDE中，打开`ui.config` Maven子项目
1. 导航 `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. 创建名为`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`的文件
1. 将以下内容添加到文件：

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
           "Authorization"
       ]
   }
   ```

`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`文件应该如下：

![SPA Editor CORS配置](./assets/aem-project/cors-configuration.png)

关键配置元素包括：

+ `alloworigin` 指定允许哪些主机从AEM检索内容。
   + `localhost:3000` 已添加以支持本地运行的SPA
   + `https://external-hosted-app` 充当要替换为Remote SPA托管的域的占位符。
+ `allowedpaths` 指定此CORS配置涵盖AEM中的哪些路径。默认设置允许访问AEM中的所有内容，但这可以仅限SPA可以访问的特定路径，例如：`/content/wknd-app`。

## 将AEM页面设置为远程SPA页面模板

AEM项目原型可生成为AEM与远程SPA集成准备的项目，但需要对自动生成的AEM页面结构进行小而重要的调整。 自动生成的AEM页面的类型必须更改为&#x200B;__远程SPA页面__，而不是&#x200B;__SPA页面__。

1. 在IDE中，打开`ui.content`子项目
1. 打开到`src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. 使用以下方式更新此`.content.xml`文件：

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

关键更改是对`jcr:content`节点的更新：

+ `cq:template` 到 `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` 到 `wknd-app/components/remotepage`

`src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`文件应该如下：

![主页 .content.xml更新](./assets/aem-project/home-content-xml.png)

这些更改允许此页面(作为AEM中的SPA根目录)在SPA编辑器中加载远程SPA。

>[!NOTE]
>
>如果此项目之前是AEM，请确保将AEM页面删除为&#x200B;__站点> WKND应用程序>我们> en > WKND应用程序主页__，因为`ui.content`项目设置为&#x200B;__merge__&#x200B;节点，而不是&#x200B;__update__。

此页面也可以删除并重新创建为AEM中的远程SPA页面，但是，由于此页面是在`ui.content`项目中自动创建的，因此最好在代码库中更新它。

## 将AEM项目部署到AEM SDK

1. 确保AEM作者服务在端口4502上运行
1. 从命令行中，导航到AEM Maven项目的根
1. 使用Maven将项目部署到您的本地AEM SDK作者服务

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## 配置根AEM页

部署AEM项目后，准备SPA Editor以加载远程SPA的最后一步。 在AEM中，标记由AEM Project Archetype生成的与SPA根`/content/wknd-app/us/en/home`对应的AEM页面。

1. 登录到AEM作者
1. 导航到&#x200B;__站点> WKND应用程序>我们> en__
1. 选择&#x200B;__WKND应用程序主页__，然后点按&#x200B;__属性__

   ![WKND应用程序主页 — 属性](./assets/aem-content/edit-home-properties.png)

1. 导航到&#x200B;__SPA__&#x200B;选项卡
1. 填写&#x200B;__远程SPA配置__
   + __SPA主机URL__:  `http://localhost:3000`
      + 远程SPA根目录的URL

   ![WKND应用程序主页 — 远程SPA配置](./assets/aem-content/remote-spa-configuration.png)

1. 点按&#x200B;__保存并关闭__

请记住，我们将此页面的类型更改为&#x200B;__远程SPA页面__&#x200B;的类型，这允许我们查看其&#x200B;__页面属性__&#x200B;中的&#x200B;__SPA__&#x200B;选项卡。

此配置只能在AEM页面上设置，该页面对应于SPA的根。 此页面下的所有AEM页面都会继承该值。

## 恭喜

您现在已准备好AEM配置并将其部署到您的本地AEM作者！ 您现在知道如何：

+ 通过注释掉`ui.frontend`中的依赖项，删除AEM项目原型生成的SPA
+ 将Sling映射添加到AEM，将SPA路由映射到AEM中的资源
+ 设置AEM跨来源资源共享安全策略，以允许远程SPA使用AEM中的内容
+ 将AEM项目部署到您的本地AEM SDK作者服务
+ 使用SPA主机URL页属性将AEM页面标记为远程SPA根

## 后续步骤

配置AEM后，我们可以侧重于[引导远程SPA](./spa-bootstrap.md)，支持使用SPA AEM Editor编辑可编辑区域！