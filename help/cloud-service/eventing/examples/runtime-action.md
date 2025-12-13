---
title: Adobe I/O Runtime操作和AEM事件
description: 了解如何使用Adobe I/O Runtime操作接收AEM事件并查看事件详细信息，例如有效负载、标头和元数据。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
exl-id: b1c127a8-24e7-4521-b535-60589a1391bf
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 1%

---

# Adobe I/O Runtime操作和AEM事件

了解如何使用[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/)操作接收AEM事件并查看事件详细信息，例如有效负载、标头和元数据。

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime是一个无服务器平台，允许响应Adobe I/O Events执行代码。 从而帮助您构建事件驱动型应用程序，而无需担心基础架构。

在此示例中，您创建了一个接收AEM事件并记录事件详细信息的Adobe I/O Runtime [操作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)。
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

高级步骤包括：

- 在Adobe Developer Console中创建项目
- 初始化项目以进行本地开发
- 在Adobe Developer Console中配置项目
- 触发AEM事件并验证操作执行情况

## 先决条件

要完成本教程，您需要：

- 已启用[AEM事件的AEM as a Cloud Service环境](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)。

- 访问[Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started)。

- 本地计算机上已安装[Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)。

## 在Adobe Developer Console中创建项目

要在Adobe Developer Console中创建项目，请执行以下步骤：

- 导航到[Adobe Developer Console](https://developer.adobe.com/)并单击&#x200B;**控制台**&#x200B;按钮。

- 在&#x200B;**快速入门**&#x200B;部分中，单击&#x200B;**从模板创建项目**。 然后在&#x200B;**浏览模板**&#x200B;对话框中，选择&#x200B;**App Builder**&#x200B;模板。

- 根据需要更新项目标题、应用程序名称并添加工作区。 然后，单击&#x200B;**保存**。

  ![在Adobe Developer Console中创建项目](../assets/examples/runtime-action/create-project.png)


## 初始化项目以进行本地开发

要将Adobe I/O Runtime操作添加到项目中，必须初始化项目以进行本地开发。 在本地计算机打开终端上，导航到要初始化项目的位置，然后执行以下步骤：

- 通过运行初始化项目

  ```bash
  aio app init
  ```

- 选择`Organization`、您在上一步中创建的`Project`和工作区。 在`What templates do you want to search for?`步骤中，选择`All Templates`选项。

  ![组织项目选择 — 初始化项目](../assets/examples/runtime-action/all-templates.png)

- 从模板列表中选择`@adobe/generator-app-excshell`选项。

  ![可扩展性模板 — 初始化项目](../assets/examples/runtime-action/extensibility-template.png)

- 在收藏的IDE中打开项目，例如VSCode。

- 所选的&#x200B;_可扩展性模板_ (`@adobe/generator-app-excshell`)提供了一个通用运行时操作，代码位于`src/dx-excshell-1/actions/generic/index.js`文件中。 让我们更新它以使其简单，记录事件详细信息并返回成功响应。 不过，在下例中，将增强处理收到的AEM事件。

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- 最后，通过运行在Adobe I/O Runtime上部署更新的操作。

  ```bash
  aio app deploy
  ```

## 在Adobe Developer Console中配置项目

要接收AEM事件并执行上一步中创建的Adobe I/O Runtime操作，请在Adobe Developer Console中配置项目。

- 在Adobe Developer Console中，导航到在上一步中创建的[项目](https://developer.adobe.com/console/projects)，然后单击以将其打开。 选择`Stage`工作区，这是部署操作的位置。

- 单击“添加服务”**&#x200B;**&#x200B;按钮并选择&#x200B;**API**&#x200B;选项。 在&#x200B;**添加API**&#x200B;模式中，选择&#x200B;**Adobe服务** > **I/O管理API**，然后单击&#x200B;**下一步**，执行其他配置步骤并单击&#x200B;**保存配置的API**。

  ![添加服务 — 配置项目](../assets/examples/runtime-action/add-io-management-api.png)

- 同样，单击“**添加服务**”按钮并选择“**事件**”选项。 在&#x200B;**添加事件**&#x200B;对话框中，选择&#x200B;**Experience Cloud** > **AEM Sites**，然后单击&#x200B;**下一步**。 按照其他配置步骤，选择AEMCS实例、事件类型和其他详细信息。

- 最后，在&#x200B;**如何接收事件**&#x200B;步骤中，展开&#x200B;**运行时操作**&#x200B;选项并选择在上一步中创建的&#x200B;_通用_&#x200B;操作。 单击&#x200B;**保存配置的事件**。

  ![运行时操作 — 配置项目](../assets/examples/runtime-action/select-runtime-action.png)

- 查看事件注册详细信息，以及&#x200B;**调试跟踪**&#x200B;选项卡，并验证&#x200B;**质询探测**&#x200B;请求和响应。

  ![事件注册详细信息](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## 触发AEM事件

要通过已在上述AEM项目中注册的AEM as a Cloud Service环境触发Adobe Developer Console事件，请执行以下步骤：

- 通过[Cloud Manager](https://my.cloudmanager.adobe.com/)访问并登录AEM as a Cloud Service创作环境。

- 根据您的&#x200B;**订阅事件**，创建、更新、删除、发布或取消发布内容片段。

## 查看事件详细信息

完成上述步骤后，您应该会看到正在交付给常规操作的AEM事件。

您可以在事件注册详细信息的&#x200B;**调试跟踪**&#x200B;选项卡中查看事件详细信息。

![AEM活动详细信息](../assets/examples/runtime-action/aem-event-details.png)


## 后续步骤

在下一个示例中，我们将增强此操作以处理AEM事件，回调AEM创作服务以获取内容详细信息，将详细信息存储在Adobe I/O Runtime存储中，并通过单页应用程序(SPA)显示它们。
