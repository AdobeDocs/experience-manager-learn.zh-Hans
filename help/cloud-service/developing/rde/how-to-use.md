---
title: 如何使用快速开发环境
description: 了解如何使用快速开发环境从本地计算机部署代码和内容。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
source-git-commit: 27d065761643030de68176ebb4ca10bc152844df
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---

# 如何使用快速开发环境

了解 **使用方法** AEMas a Cloud Service中的快速开发环境(RDE)。 从您喜爱的集成开发环境(IDE)中，将代码和内容部署到RDE，以加快近乎最终版本的开发周期。

使用 [AEM WKND站点项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 您将了解如何通过运行AEM-RDE将各种AEM工件部署到RDE `install` 命令。

- AEM代码和内容包(all， ui.apps)部署
- OSGi捆绑包和配置文件部署
- Apache和Dispatcher将部署配置为zip文件
- 单个文件，如HTL、 `.content.xml` （对话框XML）部署
- 查看其他RDE命令，如 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## 先决条件

克隆 [WKND站点](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 项目并在您喜爱的IDE中将其打开，以将AEM工件部署到RDE上。

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

然后，通过运行以下maven命令来构建它并将其部署到本地AEM-SDK。

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## 使用AEM-RDE插件部署AEM工件

使用 `aem:rde:install` 命令，让我们部署各种AEM工件。

### 部署 `all` 和 `dispatcher` 包

一个常见的起点是首先部署 `all` 和 `dispatcher` 通过运行以下命令来部署包。

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

成功部署后，请在创作和发布服务上验证WKND站点。 您应该能够在WKND网站页面上添加和编辑内容并进行发布。

### 增强和部署组件

让我们增强 `Hello World Component` 并将其部署到RDE。

1. 打开对话框XML (`.content.xml`)文件来源 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` 文件夹
1. 添加 `Description` 文本字段位于现有字段之后 `Text` 对话框字段

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. 打开 `helloworld.html` 文件来源 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` 文件夹
1. 呈现 `Description` 属性位于现有属性之后 `<div>` 元素 `Text` 属性。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. 通过执行maven构建或同步各个文件，验证本地AEM-SDK上的更改。

1. 通过以下方式将更改部署到RDE `ui.apps` 软件包或部署单个对话框和HTL文件。

   ```shell
   # Using 'ui.apps' package
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 通过添加或编辑 `Hello World Component` 在WKND网站页面上。

### 查看 `install` 命令选项

在上述单个文件部署命令示例中， `-t` 和 `-p` 标记用于分别指示JCR路径的类型和目标。 让我们查看可用的 `install` 命令选项。

```shell
$ aio aem:rde:install --help
```

旗子很明晰， `-s` 标记可用于将部署仅定位到创作或发布服务。 使用 `-t` 在部署时标记 **content-file或content-xml** 文件以及 `-p` 用于指定AEM RDE环境中的目标JCR路径的标志。

### 部署OSGi捆绑包

要了解如何部署OSGi捆绑包，让我们增强 `HelloWorldModel` Java™类并将其部署到RDE。

1. 打开 `HelloWorldModel.java` 文件来源 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 文件夹
1. 更新 `init()` 方法：

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 通过部署来验证本地AEM-SDK上的更改 `core` 通过maven命令捆绑包
1. 通过运行以下命令将更改部署到RDE

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 通过添加或编辑 `Hello World Component` 在WKND网站页面上。

### 部署OSGi配置

例如，您可以部署单个配置文件或完整的配置包，例如：

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>要仅在创作或发布实例上安装OSGi配置，请使用 `-s` 标志。


### 部署Apache或Dispatcher配置

Apache或Dispatcher配置文件 **无法单独部署**，但整个Dispatcher文件夹结构需要以ZIP文件的形式部署。

1. 在的配置文件中进行所需的更改 `dispatcher` 模块，出于演示目的，请更新 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` 以缓存 `html` 文件仅保留60秒。

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. 在本地验证更改，请参见 [在本地运行Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) 了解更多详细信息。
1. 通过运行以下命令将更改部署到RDE：

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. 验证RDE上的更改

## 其他AEM RDE插件命令

让我们查看要管理的其他AEM RDE插件命令，并与本地计算机的RDE交互。

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

使用上述命令，可以从您喜爱的IDE中管理RDE，以加快开发/部署生命周期。

## 后续步骤

了解 [使用RDE的开发/部署生命周期](./development-life-cycle.md) 快速交付功能。


## 其他资源

[RDE命令文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[用于与AEM快速开发环境交互的Adobe I/O Runtime CLI插件](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM项目设置](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
