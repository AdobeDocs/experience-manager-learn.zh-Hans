---
title: 為SPA編輯器和遠端SPA設定AEM
description: 設定支援的設定和內容要求需要AEM專案，以允許AEM SPA Editor編寫遠端SPA。
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

# 為SPA編輯器設定AEM

雖然SPA程式碼庫是在AEM之外進行管理，但需要AEM專案來設定支援的設定和內容要求。 本章會逐步說明如何建立包含必要設定的AEM專案：

+ AEM WCM Core Components代理
+ AEM遠端SPA頁面Proxy
+ AEM遠端SPA頁面範本
+ 基準遠端SPA AEM頁面
+ 用於定義SPA至AEM URL對應的子專案
+ OSGi設定資料夾

## 從GitHub下載基礎專案

下載 `aem-guides-wknd-graphql` 專案(從Github.com)。 這將包含此專案中使用的一些基準線檔案。

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## 建立AEM專案

建立管理設定和基準內容的AEM專案。 此專案將在複製的專案內產生 `aem-guides-wknd-graphql` 專案的 `remote-spa-tutorial` 資料夾。

_一律使用最新版本的 [AEM原型](https://github.com/adobe/aem-project-archetype)._

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

_最後一個命令只會重新命名AEM專案資料夾，因此很清楚它是AEM專案，不要與遠端SPA混淆__

當 `frontendModule="react"` 已指定，則 `ui.frontend` 專案不適用於遠端SPA使用案例。 SPA是在AEM外部開發和管理，僅使用AEM作為內容API。 此 `frontendModule="react"` 專案需要標幟包括  `spa-project` AEM Java™相依性及設定遠端SPA頁面範本。

AEM專案原型會產生下列元素，用於設定AEM以與SPA整合。

+ __AEM WCM Core Components代理__ 於 `ui.apps/src/.../apps/wknd-app/components`
+ __AEM SPA遠端頁面Proxy__ 於 `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __AEM頁面範本__ 於 `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __定義內容對應的子專案__ 於 `ui.content/src/...`
+ __基準遠端SPA AEM頁面__ 於 `ui.content/src/.../content/wknd-app`
+ __OSGi設定資料夾__ 於 `ui.config/src/.../apps/wknd-app/osgiconfig`

產生基本AEM專案後，幾項調整可確保SPA Editor與遠端SPA相容。

## 移除ui.frontend專案

由於SPA是遠端SPA，假設它是在AEM專案之外開發和管理的。 若要避免衝突，請移除 `ui.frontend` 部署專案。 如果 `ui.frontend` 未移除專案、兩個SPA、中提供的預設SPA `ui.frontend` 專案和遠端SPA會同時載入AEM SPA編輯器中。

1. 開啟AEM專案(`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`)。
1. 開啟根目錄 `pom.xml`
1. 註解 `<module>ui.frontend</module` 從 `<modules>` 清單

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

   此 `pom.xml` 檔案應如下所示：

   ![從reactor pom移除ui.frontend模組](./assets/aem-project/uifrontend-reactor-pom.png)

1. 開啟 `ui.apps/pom.xml`
1. 註解 `<dependency>` 於 `<artifactId>wknd-app.ui.frontend</artifactId>`

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

   此 `ui.apps/pom.xml` 檔案應如下所示：

   ![從ui.apps中移除ui.frontend相依性](./assets/aem-project/uifrontend-uiapps-pom.png)

如果AEM專案是在這些變更之前建置，請手動刪除 `ui.frontend` 產生的使用者端資源庫 `ui.apps` 專案位置 `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## AEM內容對應

若要讓AEM在SPA編輯器中載入遠端SPA，必須建立SPA路由和用於開啟及編寫內容的AEM頁面之間的對應。

稍後會探討此設定的重要性。

對應可以透過完成 [Sling對應](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) 定義於 `/etc/map`.

1. 在IDE中，開啟 `ui.content` 子專案
1. 导航至  `src/main/content/jcr_root`
1. 建立資料夾 `etc`
1. 在 `etc`，建立資料夾 `map`
1. 在 `map`，建立資料夾 `http`
1. 在 `http`，建立檔案 `.content.xml` 包含下列內容：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. 在 `http` ，建立資料夾 `localhost_any`
1. 在 `localhost_any`，建立檔案 `.content.xml` 包含下列內容：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. 在 `localhost_any` ，建立資料夾 `wknd-app-routes-adventure`
1. 在 `wknd-app-routes-adventure`，建立檔案 `.content.xml` 包含下列內容：

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

1. 將對應節點新增至 `ui.content/src/main/content/META-INF/vault/filter.xml` 以包含在AEM套件中。

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

資料夾結構和 `.context.xml` 檔案應如下所示：

![Sling對應](./assets/aem-project/sling-mapping.png)

此 `filter.xml` 檔案應如下所示：

![Sling對應](./assets/aem-project/sling-mapping-filter.png)

現在，部署AEM專案時，這些設定會自動包含在內。

Sling對應影響AEM執行於 `http` 和 `localhost`，因此僅支援本機開發。 部署至AEMas a Cloud Service時，必須將類似的Sling對應新增至目標 `https` 以及適當的AEMas a Cloud Service網域。如需詳細資訊，請參閱 [Sling對應檔案](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## 跨原始資源共用安全性原則

接下來，設定AEM以保護內容，使只有此SPA可以存取AEM內容。 設定 [AEM中的跨原始資源共用](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. 在IDE中，開啟 `ui.config` Maven子專案
1. 导航 `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. 建立名為的檔案 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. 將下列專案新增至檔案：

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

此 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` 檔案應如下所示：

![SPA編輯器CORS設定](./assets/aem-project/cors-configuration.png)

主要組態元素包括：

+ `alloworigin` 指定允許哪些主機從AEM擷取內容。
   + `localhost:3000` 新增以支援在本機執行的SPA
   + `https://external-hosted-app` 充當預留位置，以取代為遠端SPA託管所在的網域。
+ `allowedpaths` 指定此CORS設定涵蓋AEM中的哪些路徑。 預設允許存取AEM中的所有內容，不過這只能限定為SPA可以存取的特定路徑，例如： `/content/wknd-app`.

## 將AEM頁面設定為遠端SPA頁面範本

AEM專案原型會產生一個專案，以準備與遠端SPA進行AEM整合，但需要對自動產生的AEM頁面結構進行小幅但重要的調整。 自動產生的AEM頁面必須將其型別變更為 __遠端SPA頁面__，而非 __SPA頁面__.

1. 在IDE中，開啟 `ui.content` 子專案
1. 開啟至 `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. 更新此 `.content.xml` 檔案：

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

主要變更為 `jcr:content` 節點的：

+ `cq:template` 到 `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` 到 `wknd-app/components/remotepage`

此 `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` 檔案應如下所示：

![首頁.content.xml更新](./assets/aem-project/home-content-xml.png)

這些變更可讓此頁面(在AEM中充當SPA根目錄)在SPA編輯器中載入遠端SPA。

>[!NOTE]
>
>如果此專案先前已部署至AEM，請務必刪除AEM頁面，如下所示 __網站> WKND應用程式>我們>英文> WKND應用程式首頁__，作為 `ui.content`  專案已設定為 __合併__ 節點，而非 __更新__.

此頁面也可以在AEM本身中移除並重新建立為遠端SPA頁面，但由於此頁面是在 `ui.content` 專案最好在程式碼庫中更新。

## 將AEM專案部署至AEM SDK

1. 請確定AEM Author服務正在連線埠4502上執行
1. 從命令列，瀏覽至AEM Maven專案的根目錄
1. 使用Maven將專案部署到您的本機AEM SDK作者服務

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn全新安裝 — PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## 設定根AEM頁面

部署AEM專案後，最後一個步驟是準備SPA編輯器以載入我們的遠端SPA。 在AEM中，標示與SPA根目錄相對應的AEM頁面，`/content/wknd-app/us/en/home`，由AEM專案原型產生。

1. 登入AEM Author
1. 導覽至 __網站> WKND應用程式>我們>英文__
1. 選取 __wknd應用程式首頁__，然後點選 __屬性__

   ![WKND應用程式首頁 — 屬性](./assets/aem-content/edit-home-properties.png)

1. 導覽至 __SPA__ 標籤
1. 填寫 __遠端SPA設定__
   + __SPA主機URL__： `http://localhost:3000`
      + 遠端SPA根目錄的URL

   ![WKND應用程式首頁 — 遠端SPA設定](./assets/aem-content/remote-spa-configuration.png)

1. 點選 __儲存並關閉__

請記住，我們已將此頁面的型別變更為的 __遠端SPA頁面__，因此可讓我們檢視 __SPA__ 標籤中的變數 __頁面屬性__.

此設定只能在對應至SPA根目錄的AEM頁面上設定。 此頁面下的所有AEM頁面都會繼承值。

## 恭喜

您現在已準備AEM設定，並將其部署至本機AEM作者！ 您現在知道如何：

+ 將中的相依性註解，移除AEM專案原型產生的SPA `ui.frontend`
+ 新增Sling對應至AEM，將SPA路由對應至AEM中的資源
+ 設定AEM跨原始資源共用安全性原則，以允許遠端SPA使用來自AEM的內容
+ 將AEM專案部署到您的本機AEM SDK作者服務
+ 使用AEM主機URL頁面屬性，將SPA頁面標示為遠端SPA根目錄

## 后续步骤

AEM設定完成後，我們可以 [啟動載入遠端SPA](./spa-bootstrap.md) 支援使用AEM SPA編輯器編輯區域！
