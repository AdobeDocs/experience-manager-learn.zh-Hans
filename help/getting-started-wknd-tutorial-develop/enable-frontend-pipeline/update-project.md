---
title: 更新全栈AEM项目以使用前端管道
description: 了解如何更新全栈AEM项目以便为前端管道启用它，以便它仅构建和部署前端工件。
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 372
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# 更新全栈AEM项目以使用前端管道 {#update-project-enable-frontend-pipeline}

在本章中，我们对 __WKND站点项目__ 使用前端管道来部署JavaScript和CSS，而不是要求完全的全栈管道执行。 这将前端和后端工件的开发和部署生命周期分离，从而允许在整个开发过程中实现更快速、迭代的开发过程。

## 目标 {#objectives}

* 更新全栈项目以使用前端管道

## 全栈AEM项目中的配置更改概述

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## 前提条件 {#prerequisites}

本教程包含多个部分，并假定您已查看 [“ui.frontend”模块](./review-uifrontend-module.md).


## 对全栈AEM项目的更改

有三项与项目相关的配置更改和一项要为测试运行部署的样式更改，因此WKND项目中总共有四项特定的更改，以便为前端管道合同启用它。

1. 删除 `ui.frontend` 全栈构建周期中的模块

   * 在中，WKND Sites项目的根 `pom.xml` 评论 `<module>ui.frontend</module>` 子模块条目。

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

   * 和注释来自的相关依赖项 `ui.apps/pom.xml`

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

1. 准备 `ui.frontend` 用于前端管道合同的模块，添加两个新的webpack配置文件。

   * 复制现有 `webpack.common.js` 作为 `webpack.theme.common.js`，并更改 `output` 属性和 `MiniCssExtractPlugin`， `CopyWebpackPlugin` 插件配置参数如下所示：

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
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * 复制现有 `webpack.prod.js` 作为 `webpack.theme.prod.js`，并更改 `common` 变量在上述文件中的位置为

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >以上两个“webpack”配置更改将具有不同的输出文件和文件夹名称，因此我们可以轻松区分clientlib（全栈）和生成的主题（前端）管道前端工件。
   >
   >如您所知，也可以跳过上述更改以使用现有Webpack配置，但需要以下更改。
   >
   >这取决于您想要如何命名或组织它们。


   * 在 `package.json` 文件，确保  `name` 属性值与中的网站名称相同 `/conf` 节点。 在 `scripts` 属性， a `build` 指导如何从该模块构建前端文件的脚本。

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

1. 准备 `ui.content` 模块，用于前端管道，添加两个Sling配置。

   * 创建文件于 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`  — 这包括 `ui.frontend` 模块在下生成 `dist` 文件夹中的webpack构建过程。

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
   >    查看完整的 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 在 __AEM WKND站点项目__.


   * 第二个 `com.adobe.aem.wcm.site.manager.config.SiteConfig` 使用 `themePackageName` 值与 `package.json` 和 `name` 属性值和 `siteTemplatePath` 指向 `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` 存根路径值。

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
   >    请参见，完整的 [站点配置](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 在 __AEM WKND站点项目__.

1. 主题或样式更改为通过测试运行的前端管道部署，我们正在更改 `text-color` Adobe为红色（或者您也可以选择自己的颜色），方法是 `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

最后，将这些更改推送到程序的AdobeGit存储库。


>[!AVAILABILITY]
>
> 这些更改可在GitHub上的 [__前端管道__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 的分支 __AEM WKND站点项目__.


## 注意 —  _启用前端管道_ 按钮

此 [边栏选择器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 的 [站点](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 选项显示 **启用前端管道** 按钮时，选择您的站点根或站点页面。 点击 **启用前端管道** 按钮将覆盖上述内容 **Sling配置**，请确保 **您不单击** 在通过Cloud Manager管道执行部署上述更改后，可使用此按钮。

![启用前端管道按钮](assets/enable-front-end-Pipeline-button.png)

如果错误地单击它，则必须重新运行管道，以确保前端管道合同和更改恢复。

## 恭喜！ {#congratulations}

恭喜，您已更新WKND Sites项目以便为前端管道合同启用它。

## 后续步骤 {#next-steps}

在下一章中， [使用前端管道部署](create-frontend-pipeline.md)，您将创建并运行前端管道，并验证如何 __已移开__ 从基于“/etc.clientlibs”的前端资源投放中。
