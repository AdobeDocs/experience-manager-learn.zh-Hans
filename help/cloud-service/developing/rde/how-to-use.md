---
title: 如何利用快速开发环境
description: 了解如何使用快速开发环境从本地计算机部署代码和内容。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 65d54f0137786c7e8ac9ac962c424dd20bf5f3dd
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---


# 如何利用快速开发环境

学习 **如何使用** AEMas a Cloud Service中的快速开发环境(RDE)。 部署代码和内容，以便从您喜爱的集成开发环境(IDE)将近最终代码的开发周期更快地部署到RDE。

使用 [AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 您了解如何通过运行AEM-RDE的 `install` 命令。

- AEM代码和内容包（所有， ui.apps）部署
- OSGi包和配置文件部署
- Apache和Dispatcher将部署配置为zip文件
- 单个文件，如HTL， `.content.xml` （对话框XML）部署
- 查看其他RDE命令，如 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## 先决条件

克隆 [WKND站点](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 项目，并在您喜爱的IDE中打开它以将AEM工件部署到RDE。

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

然后，运行以下maven命令，生成该代码并将其部署到本地AEM-SDK。

```
$ cd aem-guides-wknd/
$ mvn clean install -PautoInstallSinglePackage
```

## 使用AEM-RDE插件部署AEM工件

使用 `aem:rde:install` 命令，让我们部署各种AEM工件。

### 部署 `all` 和 `dispatcher` 软件包

通常的起点是首先部署 `all` 和 `dispatcher` 包。

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

成功部署后，在创作和发布服务上验证WKND站点。 您应该能够在WKND站点页面上添加、编辑内容并发布该内容。

### 增强和部署组件

让我们加强 `Hello World Component` 并部署到RDE。

1. 打开对话框XML(`.content.xml`从 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` 文件夹
1. 添加 `Description` 文本字段 `Text` 对话框字段

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
1. 呈现 `Description` 属性 `<div>` 元素 `Text` 属性。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. 通过执行Maven内部版本或同步单个文件，验证对本地AEM-SDK所做的更改。

1. 通过将更改部署到RDE `ui.apps` 包，或通过部署单个Dialog和HTL文件来包装。

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

在上述单个文件部署命令示例中， `-t` 和 `-p` 标记用于分别指示JCR路径的类型和目标。 让我们查看可用 `install` 命令选项。

```shell
$ aio aem:rde:install --help
```

旗子不言自明， `-s` 标记对于仅将部署定位到创作或发布服务非常有用。 使用 `-t` 标记部署时 **content-file或content-xml** 文件以及 `-p` 标记，用于在AEM RDE环境中指定目标JCR路径。

### 部署OSGi包

要了解如何部署OSGi包，请让我们增强 `HelloWorldModel` Java™类并将其部署到RDE。

1. 打开 `HelloWorldModel.java` 文件来源 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 文件夹
1. 更新 `init()` 方法如下所示：

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 通过部署 `core` 通过maven命令捆绑
1. 通过运行以下命令将更改部署到RDE

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 通过添加或编辑 `Hello World Component` 在WKND网站页面上。

### 部署OSGi配置

您可以部署单个配置文件或完整的配置包，例如：

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
>要仅在创作或发布实例上安装OSGi配置，请使用 `-s` 标记。


### 部署Apache或Dispatcher配置

Apache或Dispatcher配置文件 **无法单独部署**，但是整个Dispatcher文件夹结构需要以ZIP文件的形式部署。

1. 在的配置文件中进行所需的更改 `dispatcher` 模块，为演示目的，请更新 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` 缓存 `html` 文件的持续时间为60秒。

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

1. 在本地验证更改，请参阅 [在本地运行Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) 以了解更多详细信息。
1. 通过运行以下命令将更改部署到RDE:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. 验证RDE上的更改

## 其他AEM RDE插件命令

让我们查看其他AEM RDE插件命令，以从本地计算机管理RDE并与之交互。

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

了解 [使用RDE的开发/部署生命周期](./development-life-cycle.md) 快速提供功能。


## 其他资源

[RDE命令文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Adobe I/O Runtime CLI插件，用于与AEM快速开发环境进行交互](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM项目设置](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
