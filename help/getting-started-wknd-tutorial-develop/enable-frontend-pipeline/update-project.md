---
title: 更新全栈 AEM 项目，以使用前端管道
description: 了解如何更新全栈 AEM 项目，以便能为前端管道启用它，使它仅构建和部署前端工件。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 307
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 100%

---

# 更新全栈 AEM 项目，以使用前端管道 {#update-project-enable-frontend-pipeline}

在本章中，我们将对 __WKND Sites 项目__&#x200B;做一些配置更改，以使用前端管道部署 JavaScript 和 CSS，而不需要完整的全栈管道执行。这样可以将前端与后端工件的开发和部署生命周期解耦，从而实现整体上更快速的迭代式开发过程。

## 目标 {#objectives}

* 更新全栈项目，以使用前端管道

## 全栈 AEM 项目的配置更改概述

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## 先决条件 {#prerequisites}

这是一个多段式教程，并假设您已经查看了 [‘ui.frontend’ 模块](./review-uifrontend-module.md)。


## 对全栈 AEM 项目的更改

有三个与项目相关的配置更改和一个样式更改需要部署，然后进行测试运行，因此 WKND 项目中总共有四个特定的更改，使其能够用于前端管道契约。

1. 移除全栈构建周期中的 `ui.frontend` 模块

   * 在 WKND Sites 项目的根目录 `pom.xml` 中注释 `<module>ui.frontend</module>` 子模块条目。

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * 并注释 `ui.apps/pom.xml` 中的相关的依赖项

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. 添加两个新的 webpack 配置文件，使 `ui.frontend` 模块符合前端管道契约。

   * 将现有的 `webpack.common.js` 复制为 `webpack.theme.common.js`，并将 `output` 属性和 `MiniCssExtractPlugin`、`CopyWebpackPlugin` 插件配置参数更改如下：

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './theme' }
           ]
       })
   ...
   ```

   * 将现有的 `webpack.prod.js` 复制为 `webpack.theme.prod.js`，并将 `common` 变量的位置更改为上述文件，如下：

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >上述两个“webpack”配置更改将具有不同的输出文件和文件夹名称，因此我们可以轻松区分 clientlib（全栈）和主题生成的（前端）管道前端工件。
   >
   >如您所知，也可以跳过上述更改，使用现有的 webpack 配置，但需要进行以下更改。
   >
   >您可以自行决定如何命名或组织它们。


   * 在 `package.json` 文件中，确保 `name` 属性值与 `/conf` 节点中的网站名称相同。在 `scripts` 属性中，`build` 脚本指示了如何从这个模块构建前端文件。

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. 添加两个 Sling 配置，为使用前端管道准备 `ui.content` 模块。

   * 在 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` 创建一个文件——这包括 `ui.frontend` 模块通过 webpack 构建过程在 `dist` 文件夹中生成的所有前端文件。

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    在 __AEM WKND Sites 项目__&#x200B;中查看完整的 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml)。


   * 其次，`com.adobe.aem.wcm.site.manager.config.SiteConfig` 和 `themePackageName` 值与 `package.json` 和 `name` 属性值相同，`siteTemplatePath` 指向一个 `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` 存根路径值。

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    在 __AEM WKND Sites 项目__&#x200B;中查看完整的 [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml)。

1. 为了测试如何通过前端管道来部署主题或样式更改，我们通过更新 `ui.frontend/src/main/webpack/base/sass/_variables.scss` 将 `text-color` 改为 Adobe 红色（或者您自行选择的其他颜色）。

   ```css
       $black:     #a40606;
       ...
   ```

最后，将这些更改推送到项目的 Adobe git 存储库。


>[!AVAILABILITY]
>
> 这些更改可在 __AEM WKND Sites 项目__&#x200B;的&#x200B;[__前端管道__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline)分支中的 GitHub 上找到。


## 请注意——_启用前端管道_&#x200B;按钮

当您选择网站根目录或网站页面时，[边栏选择器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html)的[网站](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html)选项会显示 **启用前端管道**&#x200B;按钮。点击&#x200B;**启用前端管道**&#x200B;按钮会覆盖上述 **Sling 配置**，请确保您在通过 Cloud Manager 管道执行部署了上述更改后&#x200B;**不要点击**&#x200B;这个按钮。

![启用前端管道按钮](assets/enable-front-end-Pipeline-button.png)

如果误点击此按钮，您就必须重新运行管道，以确保前端管道契约和更改重新恢复。

## 恭喜！ {#congratulations}

祝贺您更新了 WKND Sites 项目，以便能为前端管道契约启用它。

## 后续步骤 {#next-steps}

在下一章[使用前端管道进行部署](create-frontend-pipeline.md)中，您将创建并运行前端管道，并验证我们如何&#x200B;__不再使用__&#x200B;基于“/etc.clientlibs”的前端资源交付。
