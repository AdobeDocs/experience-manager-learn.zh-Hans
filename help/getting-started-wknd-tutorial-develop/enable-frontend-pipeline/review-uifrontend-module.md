---
title: 查看全栈项目的ui.frontend模块
description: 回顾基于Maven的全栈AEM Sites项目的前端开发、部署和交付生命周期。
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
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 2%

---


# 查看全栈AEM项目的“ui.frontend”模块 {#aem-full-stack-ui-frontent}

在中，我们将重点介绍全栈AEM项目的“ui.frontend”模块，以审查前端工件的开发、部署和交付 __WKND Sites项目__.


## 目标 {#objective}

* 了解AEM全栈项目中前端对象的构建和部署流程
* 查看AEM全栈项目的 `ui.frontend` 模块 [webpack](https://webpack.js.org/) 配置
* AEM客户端库（也称为clientlibs）生成过程

## AEM全栈和快速站点创建项目的前端部署流程

>[!IMPORTANT]
>
>此视频介绍并演示了两者的前端流程 **全栈和快速网站创建** 项目来概述前端资源构建、部署和交付模型中的细微差异。

>[!VIDEO](https://video.tv.adobe.com/v/3409344/)

## 前提条件 {#prerequisites}


* 克隆 [AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)
* 已构建并将克隆的AEM WKND Sites项目部署到AEMas a Cloud Service。

请参阅AEM WKND Site项目 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) 以了解更多详细信息。

## AEM全栈项目前端项目流 {#flow-of-frontend-artifacts}

以下是 __开发、部署和交付__ 全栈AEM项目中前端工件的流量。

![开发、部署和交付前端工件](assets/Dev-Deploy-Delivery-AEM-Project.png)


在开发阶段，前端会通过更新 `ui.frontend/src/main/webpack` 文件夹。 然后，在构建时， [webpack](https://webpack.js.org/) module-bundler和maven插件会将这些文件转换为位于 `ui.apps` 模块。

运行 [__全栈__ Cloud Manager中的管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

前端资源通过以开头的URI路径传送到Web浏览器 `/etc.clientlibs/`，和通常缓存在AEM Dispatcher和CDN上。


>[!NOTE]
>
> 同样，在 __AEM快速网站创建历程__, [前端更改](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) 通过运行 __前端__ 管道，请参阅 [设置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### 查看WKND Sites项目中的Web包配置 {#development-frontend-webpack-clientlib}

* 有三个 __webpack__ 用于捆绑WKND站点前端资源的配置文件。

   1. `webpack.common`  — 其中包含 __公共__ 配置以指导WKND资源捆绑和优化。 的 __输出__ 属性可告知在何处发出它创建的统一文件(也称为JavaScript包，但不要与AEM OSGi包混淆)。 默认名称设置为 `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` 包含 __开发__ webpack-dev-serve的配置，并指向要使用的HTML模板。 它还包含运行于的AEM实例的代理配置 `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` 包含 __生产__ 配置，并使用插件将开发文件转换为优化的包。

   ```javascript
       ...
       module.exports = merge(common, {
           mode: 'production',
           optimization: {
               minimize: true,
               minimizer: [
                   new TerserPlugin(),
                   new CssMinimizerPlugin({ ...})
           }
       ...    
   ```


* 捆绑的资源将移至 `ui.apps` 模块使用 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 插件，使用 `clientlib.config.js` 文件。

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* 的 __frontend-maven-plugin__ 从 `ui.frontend/pom.xml` 在AEM项目构建期间协调webpack捆绑和clientlib生成。

`$ mvn clean install -PautoInstallSinglePackage`

### 部署到AEMas a Cloud Service {#deployment-frontend-aemaacs}

的 [__全栈__ 管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) 将这些更改部署到AEMas a Cloud Service环境。


### 从AEMas a Cloud Service提交 {#delivery-frontend-aemaacs}

通过全栈管道部署的前端资源将从AEM Site传送到Web浏览器，如 `/etc.clientlibs` 文件。 您可以通过访问 [公共托管的WKND站点](https://wknd.site/content/wknd/us/en.html) 以及网页的查看来源。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 恭喜！ {#congratulations}

恭喜，您已查看了全栈项目的ui.frontend模块

## 下面的步骤 {#next-steps}

在下一章中， [更新项目以使用前端管道](update-project.md)，您将更新AEM WKND Sites项目以为前端管道合同启用它。
