---
title: 自定义错误页面
description: 了解如何为AEM as a Cloud Service托管的网站实施自定义错误页面。
version: Cloud Service
feature: Brand Experiences, Configuring, Developing
topic: Content Management, Development
role: Developer
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-24T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: 01e6ef917d855e653eccfe35a2d7548f12628604
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---


# 自定义错误页面

了解如何为AEM as a Cloud Service托管的网站实施自定义错误页面。

在本教程中，您将学习：

- 默认错误页面
- 提供错误页面的位置
   - AEM服务类型 — 作者、发布、预览
   - Adobe管理的CDN
- 用于自定义错误页面的选项
   - ErrorDocument Apache指令
   - ACS AEM Commons — 错误页面处理程序
   - CDN错误页面

## 默认错误页面

让我们回顾一下何时显示错误页面、默认错误页面以及提供这些页面的位置。

出现以下情况时，将显示错误页面：

- 页面不存在(404)
- 未被授权访问页面(403)
- 服务器错误(500)，原因是代码问题或服务器不可访问。

AEM as a Cloud Service为上述方案提供了&#x200B;_默认错误页面_。 它是一个通用页面，与您的品牌不匹配。

默认错误页&#x200B;_从_ AEM服务类型&#x200B;_（创作、发布、预览）或_ Adobe管理的CDN _提供_。 有关更多详细信息，请参阅下表。

| 错误页面提供自 | 详细信息 |
|---------------------|:-----------------------:|
| AEM服务类型 — 作者、发布、预览 | 当页面请求由AEM服务类型提供并且出现上述任何错误情况时，错误页面将从AEM服务类型提供。 |
| Adobe管理的CDN | 当Adobe管理的CDN _无法访问AEM服务类型_ （源服务器）时，将从Adobe管理的CDN提供错误页。 **这是一个不太可能发生但值得规划的事件。** |


例如，从AEM服务类型和Adobe管理的CDN提供的默认错误页如下所示：

![默认AEM错误页](./assets/aem-default-error-pages.png)


但是，您可以&#x200B;_自定义AEM服务类型和Adobe管理的_ CDN错误页以匹配您的品牌并提供更好的用户体验。

## 用于自定义错误页面的选项

以下选项可用于自定义错误页面：

| 适用于 | 选项名称 | 描述 |
|---------------------|:-----------------------:|:-----------------------:|
| AEM服务类型 — 发布和预览 | ErrorDocument指令 | 在Apache配置文件中使用[ErrorDocument](https://httpd.apache.org/docs/2.4/custom-error.html)指令指定自定义错误页面的路径。 仅适用于AEM服务类型 — 发布和预览。 |
| AEM服务类型 — 创作、发布、预览 | ACS AEM Commons错误页面处理程序 | 使用[ACS AEM Commons错误页处理程序](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html)自定义所有AEM服务类型的错误。 |
| Adobe管理的CDN | CDN错误页面 | 当Adobe管理的CDN无法访问AEM服务类型（源服务器）时，使用CDN错误页来自定义错误页。 |


## 先决条件

在本教程中，您将学习如何使用&#x200B;_ErrorDocument_&#x200B;指令、_ACS AEM Commons Error Page Handler_&#x200B;和&#x200B;_CDN Error Pages_&#x200B;选项自定义错误页面。 要学习本教程，您需要：

- [本地AEM开发环境](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview)或AEM as a Cloud Service环境。 _CDN错误页面_&#x200B;选项适用于AEM as a Cloud Service环境。

- 用于自定义错误页面的[AEM WKND项目](https://github.com/adobe/aem-guides-wknd)。

## 设置

- 通过执行以下步骤，克隆AEM WKND项目并将其部署到本地AEM开发环境：

  ```
  # For local AEM development environment
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallSinglePackage -PautoInstallSinglePackagePublish
  ```

- 对于AEM as a Cloud Service环境，请通过运行[全栈管道](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline)来部署AEM WKND项目，请参阅[非生产管道](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/cloud-manager/cicd-non-production-pipeline)示例。

- 验证WKND网站页面是否正确呈现。

## 用于自定义AEM提供的错误页面的ErrorDocument Apache指令{#errordocument}

要自定义AEM提供的错误页面，请使用`ErrorDocument` Apache指令。

在AEM as a Cloud Service中，`ErrorDocument` Apache指令选项仅适用于发布和预览服务类型。 它不适用于创作服务类型，因为Apache + Dispatcher不是部署架构的一部分。

让我们看看[AEM WKND](https://github.com/adobe/aem-guides-wknd)项目如何使用`ErrorDocument` Apache指令显示自定义错误页。

- `ui.content.sample`模块包含标记的[错误页面](https://github.com/adobe/aem-guides-wknd/tree/main/ui.content.sample/src/main/content/jcr_root/content/wknd/language-masters/en/errors) @ `/content/wknd/language-masters/en/errors`。 在[本地AEM](http://localhost:4502/sites.html/content/wknd/language-masters/en/errors)或AEM as a Cloud Service `https://author-p<ID>-e<ID>.adobeaemcloud.com/ui#/aem/sites.html/content/wknd/language-masters/en/errors`环境中查看它们。

- 来自`dispatcher`模块的`wknd.vhost`文件包含：
   - 指向上述[错误页](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/variables/custom.vars#L7-L8)的[ErrorDocument指令](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L139-L143)。
   - [DispatcherPassError](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L133)值设置为1，因此Dispatcher允许Apache处理所有错误。

  ```
  ...
  # ErrorDocument directive in wknd.vhost file
  ErrorDocument 404 ${404_PAGE}
  ErrorDocument 500 ${500_PAGE}
  ErrorDocument 502 ${500_PAGE}
  ErrorDocument 503 ${500_PAGE}
  ErrorDocument 504 ${500_PAGE}
  
  ...
  # DispatcherPassError value in wknd.vhost file
  <IfModule disp_apache2.c>
      ...
      DispatcherPassError        1
  </IfModule>
  
  # Custom error pages path in custom.vars file
  Define 404_PAGE /content/wknd/us/en/errors/404.html
  Define 500_PAGE /content/wknd/us/en/errors/500.html
  ...
  ```

- 通过在您的环境中输入错误的页面名称或路径(例如[https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html](https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html))来查看WKND网站的自定义错误页面。

## ACS AEM Commons-Error页面处理程序，用于自定义AEM提供的错误页面{#acs-aem-commons}

要自定义&#x200B;_所有AEM服务类型_&#x200B;中的AEM提供的错误页，您可以使用[ACS AEM Commons Error Page Handler](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html)选项。

。有关详细的分步说明，请参阅[如何使用](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html#how-to-use)部分。

## 用于自定义CDN提供的错误页面的CDN错误页面{#cdn-error-pages}

要自定义Adobe管理的CDN提供的错误页面，请使用CDN错误页面选项。

让我们实施CDN错误页以在Adobe管理的CDN无法访问AEM服务类型（源服务器）时自定义错误页。

>[!IMPORTANT]
>
> _Adobe管理的CDN无法访问AEM服务类型_ （源服务器）是&#x200B;**不可能的事件**，但值得规划。

实施CDN错误页面的高级步骤包括：

- 将自定义错误页面内容开发为单页应用程序(SPA)。
- 将CDN错误页面所需的静态文件托管在可公开访问的位置。
- 配置CDN规则(errorPages)并引用上述静态文件。
- 使用Cloud Manager管道将配置的CDN规则部署到AEM as a Cloud Service环境。
- 测试CDN错误页面。


### CDN错误页面概述

CDN错误页由Adobe管理的CDN作为单页应用程序(SPA)实现。 由Adobe管理的CDN投放的SPAHTML文档包含最低限度的HTML片段。 自定义错误页面内容是使用JavaScript文件动态生成的。 必须开发JavaScript文件，并将其托管在客户可公开访问的位置。

Adobe管理的CDN提供的HTML片段具有以下结构：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    
    ...

    <title>{title}</title>
    <link rel="icon" href="{icoUrl}">
    <link rel="stylesheet" href="{cssUrl}">
  </head>
  <body>
    <script src="{jsUrl}"></script>
  </body>
</html>
```

HTML片段包含以下占位符：

1. **jsUrl**： JavaScript文件的绝对URL，用于通过动态创建HTML元素呈现错误页面内容。
1. **cssUrl**： CSS文件的绝对URL，用于设置错误页面内容的样式。
1. **icoUrl**： favicon的绝对URL。



### 开发自定义错误页面

让我们将WKND特定的品牌错误页面内容开发为单页应用程序(SPA)。

出于演示目的，让我们使用[React](https://react.dev/)，但是，您可以使用任何JavaScript框架或库。

- 通过运行以下命令创建新的React项目：

  ```
  $ npx create-react-app aem-cdn-error-page
  ```

- 在您喜爱的代码编辑器中打开项目，并更新以下文件：

   - `src/App.js`：它是呈现错误页面内容的主组件。

     ```javascript
     import logo from "./wknd-logo.png";
     import "./App.css";
     
     function App() {
       return (
         <>
           <div className="App">
             <div className="container">
             <img src={logo} className="App-logo" alt="logo" />
             </div>
           </div>
           <div className="container">
             <div className="error-code">CDN Error Page</div>
             <h1 className="error-message">Ruh-Roh! Page Not Found</h1>
             <p className="error-description">
               We're sorry, we are unable to fetch this page!
             </p>
           </div>
         </>
       );
     }
     
     export default App;
     ```

   - `src/App.css`：为错误页面内容设置样式。

     ```css
     .App {
       text-align: left;
     }
     
     .App-logo {
       height: 14vmin;
       pointer-events: none;
     }
     
     
     body {
       margin-top: 0;
       padding: 0;
       font-family: Arial, sans-serif;
       background-color: #fff;
       color: #333;
       display: flex;
       justify-content: center;
       align-items: center;
     }
     
     .container {
       text-align: letf;
       padding-top: 10px;
     }
     
     .error-code {
       font-size: 4rem;
       font-weight: bold;
       color: #ff6b6b;
     }
     
     .error-message {
       font-size: 2.5rem;
       margin-bottom: 10px;
     }
     
     .error-description {
       font-size: 1rem;
       margin-bottom: 20px;
     }
     ```

   - 将`wknd-logo.png`文件添加到`src`文件夹。 将[文件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-512.png)复制为`wknd-logo.png`。

   - 将`favicon.ico`文件添加到`public`文件夹。 将[文件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-32.png)复制为`favicon.ico`。

   - 通过运行以下项目验证WKND品牌CDN错误页面内容：

     ```
     $ npm start
     ```

     打开浏览器并导航到`http://localhost:3000/`以查看CDN错误页面内容。

   - 构建项目以生成静态文件：

     ```
     $ npm run build
     ```

     静态文件在`build`文件夹中生成。


或者，您也可以下载包含上述React项目文件的[aem-cdn-error-page.zip](./assets/aem-cdn-error-page.zip)文件。

接下来，将以上静态文件托管在可公开访问的位置。

### CDN错误页所需的主机静态文件

让我们在Azure Blob存储中托管静态文件。 但是，您可以使用任何静态文件托管服务，如[Netlify](https://www.netlify.com/)、[Vercel](https://vercel.com/)或[AWS S3](https://aws.amazon.com/s3/)。

- 按照官方[Azure Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)文档创建容器并上传静态文件。

  >[!IMPORTANT]
  >
  >如果您使用其他静态文件托管服务，请按照其文档来托管静态文件。

- 确保静态文件可公开访问。 我的WKND演示特定存储帐户设置如下：

   - **存储帐户名称**： `aemcdnerrorpageresources`
   - **容器名称**： `static-files`

  ![Azure Blob存储](./assets/azure-blob-storage-settings.png)

- 在`static-files`以上容器中，已上传`build`文件夹中的以下文件：

   - `error.js`： `build/static/js/main.<hash>.js`文件已重命名为`error.js`和[可公开访问](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js)。
   - `error.css`： `build/static/css/main.<hash>.css`文件已重命名为`error.css`和[可公开访问](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css)。
   - `favicon.ico`： `build/favicon.ico`文件已按原样上传，并且[可公开访问](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico)。

接下来，配置CDN规则(errorPages)并引用上述静态文件。

### 配置CDN规则

让我们配置使用上述静态文件呈现CDN错误页面内容的`errorPages` CDN规则。

1. 从AEM项目的主`config`文件夹中打开`cdn.yaml`文件。 例如，[WKND项目的cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml)文件。

1. 将以下CDN规则添加到`cdn.yaml`文件：

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     # The CDN Error Page configuration. 
     # The error page is displayed when the Adobe-managed CDN is unable to reach the origin server.
     # It is implemented as a Single Page Application (SPA) and WKND branded content must be generated dynamically using the JavaScript file 
     errorPages:
       spa:
         title: Adobe AEM CDN Error Page # The title of the error page
         icoUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico # The PUBLIC URL of the favicon
         cssUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css # The PUBLIC URL of the CSS file
         jsUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js # The PUBLIC URL of the JavaScript file
   ```

1. 保存、提交更改并将更改推送到Adobe上游存储库。

### 部署CDN规则

最后，使用Cloud Manager管道将配置的CDN规则部署到AEM as a Cloud Service环境。

1. 在Cloud Manager中，导航到&#x200B;**管道**&#x200B;部分。

1. 创建新管道或选择仅部署&#x200B;**Config**&#x200B;文件的现有管道。 有关详细步骤，请参阅[创建配置管道](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)。

1. 单击&#x200B;**运行**&#x200B;按钮以部署CDN规则。

![部署CDN规则](./assets/deploy-cdn-rule-for-cdn-error-pages.png)

### 测试CDN错误页面

要测试CDN错误页面，请执行以下步骤：

- 打开浏览器并导航到Publish环境URL，将`cdnstatus?code=404`附加到URL，例如[https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404](https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404)，或使用[自定义域URL](https://wknd.enablementadobe.com/cdnstatus?code=404)进行访问

  ![WKND - CDN错误页](./assets/wknd-cdn-error-page.png)

- 支持的代码为：403、404、406、500和503。

- 验证浏览器网络选项卡以查看从Azure Blob存储加载的静态文件。 由Adobe管理的CDN交付的HTML文档包含裸的最低内容，并且JavaScript文件动态创建标记的错误页面内容。

  ![CDN错误页网络选项卡](./assets/wknd-cdn-error-page-network-tab.png)

## 摘要

在本教程中，您已了解从中提供错误页面的默认错误页面，以及自定义错误页面的选项。 您已了解如何使用`ErrorDocument` Apache指令、`ACS AEM Commons Error Page Handler`和`CDN Error Pages`选项实施自定义错误页面。

## 其他资源

- [配置CDN错误页](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-error-pages)

- [Cloud Manager — 配置管道](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#config-deployment-pipeline)

