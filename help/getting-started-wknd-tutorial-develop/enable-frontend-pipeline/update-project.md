---
title: 更新全栈AEM项目以使用前端管道
description: 了解如何更新全栈AEM项目以为前端管道启用该项目，以便该项目仅生成和部署前端工件。
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---


# 更新全栈AEM项目以使用前端管道 {#update-project-enable-frontend-pipeline}

在本章中，我们对 __WKND Sites项目__ 以使用前端管道来部署JavaScript和CSS，而不需要完全执行全栈管道。 这允许分离前端和后端工件的开发和部署生命周期，从而允许整体上更快速、更迭代的开发过程。

## 目标 {#objectives}

* 更新全栈项目以使用前端管道

## 全栈AEM项目中配置更改概述

>[!VIDEO](https://video.tv.adobe.com/v/3409419/)

## 前提条件 {#prerequisites}

这是一个多部分教程，假定您已查看 [“ui.frontend”模块](./review-uifrontend-module.md).


## 对全栈AEM项目的更改

测试运行需要部署三个与项目相关的配置更改和一个样式更改，因此，在WKND项目中总共进行了四次特定更改，以便为前端管道合同启用它。

1. 删除 `ui.frontend` 全堆栈构建周期中的模块

   * 在中，WKND Sites项目的根 `pom.xml` 注释 `<module>ui.frontend</module>` 子模块条目。

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

   * 和与注释相关的依赖项 `ui.apps/pom.xml`

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

1. 准备 `ui.frontend` 用于前端管道合同的模块，方法是添加两个新的webpack配置文件。

   * 复制现有 `webpack.common.js` as `webpack.theme.common.js`，更改 `output` 资产和 `MiniCssExtractPlugin`, `CopyWebpackPlugin` 插件配置参数如下所示：

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'clientlib-[name]/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * 复制现有 `webpack.prod.js` as `webpack.theme.prod.js`，并更改 `common` 变量在上述文件中的位置

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >上述两项“webpack”配置更改将具有不同的输出文件和文件夹名称，因此我们可以轻松地区分clientlib（全栈）和主题生成（前端）管道前端工件。
   >
   >如您所猜，也可以跳过上述更改以使用现有WebPack配置，但需要进行以下更改。
   >
   >要如何命名或组织，取决于您自己。


   * 在 `package.json` 文件，确保  `name` 属性值与 `/conf` 节点。 在 `scripts` 资产， a `build` 指示如何从此模块构建前端文件的脚本。

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

1. 准备 `ui.content` 用于前端管道的模块（通过添加两个Sling配置）。

   * 在以下位置创建新文件 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`  — 其中包括 `ui.frontend` 模块在下生成 `dist` 文件夹。

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
   >    请参阅，完成 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 在 __AEM WKND Sites项目__.


   * 第二 `com.adobe.aem.wcm.site.manager.config.SiteConfig` 和 `themePackageName` 值与 `package.json` 和 `name` 属性值和 `siteTemplatePath` 指向 `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` 存根路径值。

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
   >    请参阅，完成 [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 在 __AEM WKND Sites项目__.

1. 要通过前端管道部署以用于测试运行的主题或样式发生更改，我们正在进行更改 `text-color` Adobe为红色（或者您可以自行选择），方法是 `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

最后，将这些更改推送到您程序的Adobegit存储库。


>[!AVAILABILITY]
>
> 这些更改可在GitHub的 [__前端管道__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 分支 __AEM WKND Sites项目__.


## 恭喜！ {#congratulations}

恭喜，您已更新WKND Sites项目，以便为前端管道合同启用该项目。

## 下面的步骤 {#next-steps}

在下一章中， [使用前端管道部署](create-frontend-pipeline.md)，您将创建并运行前端管道并验证我们的 __搬走了__ 从基于“/etc.clientlibs”的前端资源交付中。
