---
title: 本地开发设置
description: 了解如何为通用编辑器设置本地开发环境，以便您可以编辑一个 React 示例应用程序的内容。
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 189
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 47bef697-5253-493a-b9f9-b26c27d2db56
source-git-commit: 7c58c5cb6a3d99a9577206b3e5e0b8dcd55a850e
workflow-type: ht
source-wordcount: '787'
ht-degree: 100%

---

# 本地开发设置

了解如何设置本地开发环境，以使用 AEM 通用编辑器编辑一个 React 应用程序的内容。

## 先决条件

学习本教程需具备以下条件：

- 基本的 HTML 和 JavaScript 技能。
- 必须在本地安装以下工具：
   - [Node.js](https://nodejs.org/en/download/)
   - [Git](https://git-scm.com/downloads)
   - IDE 或代码编辑器，例如 [Visual Studio Code](https://code.visualstudio.com/)
- 下载并安装以下软件：
   - [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk)：它包含 Quickstart Jar，用于在开发过程中本地运行 AEM Author 和 Publish。
   - [通用编辑器服务](https://experienceleague.adobe.com/zh-hans/docs/experience-cloud/software-distribution/home)：通用编辑器服务的一个本地副本，它具有部分功能，可以从软件分发门户下载。
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy)：一个使用自签名证书进行本地开发的简单的本地 SSL HTTP 代理。AEM 通用编辑器需要 React 应用程序的 HTTPS URL 才能将其加载到编辑器中。

## 本地设置

按照以下步骤设置本地开发环境：

### AEM SDK

要为 WKND Teams React 应用程序提供内容，请在本地 AEM SDK 中安装以下包。

- [WKND Teams - 内容包](./assets/basic-tutorial-solution.content.zip)：包含内容片段模型、内容片段和持久的 GraphQL 查询。
- [WKND Teams - 配置包](./assets/basic-tutorial-solution.ui.config.zip)：包含跨源资源共享 (CORS) 和令牌身份验证处理程序配置。CORS 促进非 AEM 的 Web 属性，支持客户端通过浏览器调用 AEM 的 GraphQL API；令牌身份验证处理程序则用于对每个 AEM 请求进行身份验证。

  ![WKND Teams - 包](./assets/wknd-teams-packages.png)

### React 应用程序

要设置 WKND Teams React 应用程序，请按照以下步骤操作：

1. 从 `basic-tutorial` 解决方案分支克隆 [WKND Teams React 应用程序](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial)。

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 导航到 `basic-tutorial` 目录，然后在代码编辑器中打开它。

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. 安装依赖项，启动 React 应用程序。

   ```bash
   $ npm install
   $ npm start
   ```

1. 在浏览器中打开 WKND Teams React 应用程序，位于：[http://localhost:3000](http://localhost:3000)。现在显示团队成员列表及其详细信息。React 应用程序的内容由本地 AEM SDK 通过 GraphQL API (`/graphql/execute.json/my-project/all-teams`) 提供，您可以使用浏览器的网络选项卡对此进行验证。

   ![WKND Teams - React 应用程序](./assets/wknd-teams-react-app.png)

### 通用编辑器服务

要设置&#x200B;**本地**&#x200B;通用编辑器服务，请按照以下步骤操作：

1. 从[软件分发门户](https://experience.adobe.com/downloads)下载最新版本的通用编辑器服务。

   ![软件分发 - 通用编辑器服务](./assets/universal-editor-service.png)

1. 将下载的 zip 文件解压缩，然后将 `universal-editor-service.cjs` 文件复制到名为 `universal-editor-service` 的新目录。

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. 在 `universal-editor-service` 目录中创建 `.env` 文件，然后添加以下环境变量：

   ```bash
   # The port on which the Universal Editor service runs
   UES_PORT=8000
   # Disable SSL verification
   UES_TLS_REJECT_UNAUTHORIZED=false
   ```

1. 启动本地通用编辑器服务。

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

上述命令将在端口 `8000` 上启动通用编辑器服务，您会看到以下输出：

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### 本地 SSL HTTP 代理

AEM 通用编辑器要求通过 HTTPS 提供 React 应用程序。现在，我们来设置一个使用自签名证书进行本地开发的本地 SSL HTTP 代理。

按照以下步骤设置本地 SSL HTTP 代理，然后通过 HTTPS 提供 AEM SDK 和通用编辑器服务：

1. 安装全局使用的 `local-ssl-proxy` 包。

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. 为以下服务启动本地 SSL HTTP 代理的两个实例：

   - 端口 `8443` 上的 AEM SDK 本地 SSL HTTP 代理。
   - 端口 `8001` 上的通用编辑器服务本地 SSL HTTP 代理。

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### 更新 React 应用程序以使用 HTTPS

要为 WKND Teams React 应用程序启用 HTTPS，请按照以下步骤操作：

1. 在终端中按下 `Ctrl + C` 停止 React。
1. 更新 `package.json` 文件，将 `HTTPS=true` 环境变量包含在 `start` 脚本中。

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. 更新 `.env.development` 文件中的 `REACT_APP_HOST_URI`，以使用 HTTPS 协议和 AEM SDK 的本地 SSL HTTP 代理端口。

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. 更新 `../src/proxy/setupProxy.auth.basic.js` 文件，以通过 `secure: false` 选项使用宽松的 SSL 设置。

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. 启动 React 应用程序。

   ```bash
   $ npm start
   ```

## 验证设置

通过上述步骤设置好本地开发环境后，我们现在来验证设置。

### 本地验证

确保以下服务通过 HTTPS 在本地运行，您可能需要同意浏览器中关于自签名证书的安全警告：

1. WKND Teams React 应用程序位于 [https://localhost:3000](https://localhost:3000)
1. AEM SDK 位于 [https://localhost:8443](https://localhost:8443)
1. 通用编辑器服务位于 [https://localhost:8001](https://localhost:8001)

### 在通用编辑器中加载 WKND Teams React 应用程序

现在我们在通用编辑器中加载 WKND Teams React 应用程序，以验证设置：

1. 在您的浏览器中打开通用编辑器 https://experience.adobe.com/#/aem/editor。如果出现提示，请使用您的 Adobe ID 登录。

1. 在通用编辑器的网站 URL 输入字段中输入 WKND Teams React 应用程序 URL，然后点击 `Open`。

   ![通用编辑器 - 网站 URL](./assets/universal-editor-site-url.png)

1. 通用编辑器中加载了 WKND Teams React 应用程序，**但您还不能编辑内容**。您需要对 React 应用程序进行适配，才能使用通用编辑器编辑内容。

   ![通用编辑器 - WKND Teams React 应用程序](./assets/universal-editor-wknd-teams.png)


## 后续步骤

了解如何[适配 React 应用程序以编辑内容](./instrument-to-edit-content.md)。
