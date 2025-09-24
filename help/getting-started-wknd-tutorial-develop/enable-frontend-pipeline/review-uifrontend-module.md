---
title: 查看全栈项目的 ui.frontend 模块
description: 查看基于 maven 的全栈 AEM Sites 项目的前端开发、部署和交付生命周期。
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
workflow-type: ht
source-wordcount: '560'
ht-degree: 100%

---

# 查看全栈 AEM 项目的 &#39;ui.frontend&#39; 模块 {#aem-full-stack-ui-frontent}

在本章中，我们将查看全栈 AEM 项目的前端工件的开发、部署和交付，并重点关注 __WKND Sites 项目__&#x200B;的 ui.frontend 模块。


## 目标 {#objective}

* 了解 AEM 全栈项目中前端工件的构建和部署流程
* 查看 AEM 全栈项目的 `ui.frontend` 模块的 [webpack](https://webpack.js.org/) 配置
* AEM 客户端库（也称为 clientlibs）生成过程

## AEM 全栈和快速网站创建项目的前端部署流程

>[!IMPORTANT]
>
>此视频说明并演示了&#x200B;**全栈和快速网站创建**&#x200B;两个项目的前端流程，概述了前端资源构建、部署和交付模型的细微差别。

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 先决条件 {#prerequisites}


* 克隆 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)
* 构建克隆的 AEM WKND Sites 项目，并将其部署到 AEM as a Cloud Service。

查看 AEM WKND 网站项目 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) 了解更多详细信息。

## AEM 全栈项目前端工件流程 {#flow-of-frontend-artifacts}

下面是一个全栈 AEM 项目中前端工件的&#x200B;__开发、部署和交付__&#x200B;流程的高级示意图。

![前端工件的开发、部署和交付](assets/Dev-Deploy-Delivery-AEM-Project.png)


在开发阶段，通过更新 `ui.frontend/src/main/webpack` 文件夹中的 CSS、JS 文件进行前端更改，例如确定样式和重新品牌化等。然后在构建期间，[webpack](https://webpack.js.org/) module-bundler 和 maven 插件将这些文件转换为 `ui.apps` 模块下的优化 AEM 客户端库。

[__在 Cloud Manager 中运行全栈__&#x200B;管道时](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html)，前端更改被部署到 AEM as a Cloud Service 环境。

前端资源通过以 `/etc.clientlibs/` 开头的 URI 路径被传递到网页浏览器，通常被缓存在 AEM Dispatcher 和内容传递网络上。


>[!NOTE]
>
> 与此相似，在 __AEM 快速网站创建历程__&#x200B;中，[前端更改](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html)通过运行&#x200B;__前端__&#x200B;管道被部署到 AEM as a Cloud Service 环境，请参阅[设置您的管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### 查看 WKND 网站项目中的 webpack 配置 {#development-frontend-webpack-clientlib}

* 有三个 __webpack__ 配置文件用于捆绑 WKND 网站前端资源。

   1. `webpack.common` - 包含&#x200B;__常见__&#x200B;配置，用于指导 WKND 资源捆绑和优化。__输出__&#x200B;属性指示将其创建的合并文件（也称为 JavaScript 捆绑包，但不要与 AEM OSGi 捆绑包混淆）发送到何处。默认名称被设置为 `clientlib-site/js/[name].bundle.js`。

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` 包含 webpack-dev-serve 的&#x200B;__开发__&#x200B;配置，并指向要使用的 HTML 模板。它还包含在 `localhost:4502` 上运行的 AEM 实例的代理配置。

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` 包含 __生产__&#x200B;配置，使用插件将开发文件转换为经过优化的捆绑包。

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


* 捆绑资源通过 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 插件被移动到 `ui.apps` 模块，并使用 `clientlib.config.js` 文件中管理的配置。

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

* 在 AEM 项目构建期间，来自 `ui.frontend/pom.xml` 的 __frontend-maven-plugin__ 协调安排 webpack 捆绑和 clientlib 生成。

`$ mvn clean install -PautoInstallSinglePackage`

### 部署到 AEM as a Cloud Service {#deployment-frontend-aemaacs}

[__全栈__&#x200B;管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline)将这些更改部署到 AEM as a Cloud Service 环境。


### 从 AEM as a Cloud Service 交付 {#delivery-frontend-aemaacs}

通过全栈管道部署的前端资源以 `/etc.clientlibs` 文件的形式从 AEM 网站被交付到网页浏览器。您可以访问[公开托管的 WKND 网站](https://wknd.site/content/wknd/us/en.html)并查看网页源代码，对此进行验证。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 恭喜！ {#congratulations}

祝贺您已查看全栈项目的 ui.frontend 模块

## 后续步骤 {#next-steps}

在下一章[更新项目以使用前端管道](update-project.md)中，您将更新 AEM WKND Sites 项目，以便为前端管道契约启用它。
