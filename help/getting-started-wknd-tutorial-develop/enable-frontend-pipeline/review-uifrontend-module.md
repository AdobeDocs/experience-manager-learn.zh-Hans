---
title: 查看全栈项目的ui.frontend模块
description: 查看基于maven的全栈AEM Sites项目的前端开发、部署和交付生命周期。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# 查看全栈AEM项目的“ui.frontend”模块 {#aem-full-stack-ui-frontent}

在本章中，我们通过重点介绍&#x200B;__WKND Sites项目__&#x200B;的“ui.frontend”模块，回顾了全栈AEM项目的前端工件的开发、部署和交付。


## 目标 {#objective}

* 了解AEM全栈项目中前端工件的生成和部署流程
* 查看AEM全栈项目的`ui.frontend`模块的[webpack](https://webpack.js.org/)配置
* AEM client library（也称为clientlibs）生成流程

## AEM全栈和快速站点创建项目的前端部署流程

>[!IMPORTANT]
>
>此视频介绍并演示了&#x200B;**全栈和快速站点创建**&#x200B;项目的前端流程，以概述前端资源构建、部署和投放模型中的细微差异。

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 先决条件 {#prerequisites}


* 克隆[AEM WKND站点项目](https://github.com/adobe/aem-guides-wknd)
* 构建克隆的AEM WKND Sites项目并将其部署到AEM as a Cloud Service。

有关详细信息，请参阅AEM WKND站点项目[README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md)。

## AEM全栈项目前端构件流 {#flow-of-frontend-artifacts}

下面是全栈AEM项目中前端工件的&#x200B;__开发、部署和交付__&#x200B;流的高级表示形式。

![前端项目的开发、部署和交付](assets/Dev-Deploy-Delivery-AEM-Project.png)


在开发阶段，通过更新`ui.frontend/src/main/webpack`文件夹中的CSS、JS文件来执行前端更改，如样式更改和重新品牌化。 然后，在构建期间，[webpack](https://webpack.js.org/)模块捆绑器和maven插件将这些文件转换为`ui.apps`模块下的优化AEM clientlibs。

在AEM as a Cloud Service[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=zh-Hans)中运行&#x200B;__全栈栈__&#x200B;管道时，将前端更改部署到Cloud Manager环境。

前端资源通过以`/etc.clientlibs/`开头的URI路径交付给Web浏览器，通常缓存在AEM Dispatcher和CDN上。


>[!NOTE]
>
> 同样，在&#x200B;__AEM快速站点创建历程__&#x200B;中，通过运行&#x200B;__前端__&#x200B;管道，将[前端更改](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=zh-Hans)部署到AEM as a Cloud Service环境。请参阅[设置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=zh-Hans)

### 查看WKND站点项目中的Webpack配置 {#development-frontend-webpack-clientlib}

* 有三个&#x200B;__webpack__&#x200B;配置文件用于捆绑WKND Sites前端资源。

   1. `webpack.common` — 这包含用于指示WKND资源捆绑和优化的&#x200B;__common__&#x200B;配置。 __output__&#x200B;属性说明在何处发出它创建的统一文件(也称为JavaScript包，但不要与AEM OSGi包混淆)。 默认名称设置为`clientlib-site/js/[name].bundle.js`。

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js`包含webpack-dev-serve的&#x200B;__开发__&#x200B;配置并指向要使用的HTML模板。 它还包含在`localhost:4502`上运行的AEM实例的代理配置。

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js`包含&#x200B;__生产__&#x200B;配置，并使用插件将开发文件转换为优化包。

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


* 使用`clientlib.config.js`文件中管理的配置，使用[aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator)插件将捆绑资源移动到`ui.apps`模块。

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

* 来自`ui.frontend/pom.xml`的&#x200B;__frontend-maven-plugin__&#x200B;在AEM项目构建期间编排webpack捆绑包和clientlib生成。

`$ mvn clean install -PautoInstallSinglePackage`

### 部署到AEM as a Cloud Service {#deployment-frontend-aemaacs}

[__全栈栈__&#x200B;管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=zh-Hans&#full-stack-pipeline)将这些更改部署到AEM as a Cloud Service环境。


### 来自AEM as a Cloud Service的投放 {#delivery-frontend-aemaacs}

通过全栈管道部署的前端资源将作为`/etc.clientlibs`文件从AEM站点传送到Web浏览器。 您可以通过访问[公开托管的WKND网站](https://wknd.site/content/wknd/us/en.html)并查看网页源来验证此信息。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 恭喜！ {#congratulations}

恭喜，您已查看全栈项目的ui.frontend模块

## 后续步骤 {#next-steps}

在下一章[更新项目以使用前端管道](update-project.md)中，您将更新AEM WKND Sites项目以便为前端管道合同启用它。
