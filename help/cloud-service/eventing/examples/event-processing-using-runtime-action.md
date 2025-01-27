---
title: 使用Adobe I/O Runtime操作处理AEM事件
description: 了解如何使用Adobe I/O Runtime操作处理收到的AEM事件。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 558
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
exl-id: c362011e-89e4-479c-9a6c-2e5caa3b6e02
source-git-commit: efa0a16649c41fab8309786a766483cfeab98867
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# 使用Adobe I/O Runtime操作处理AEM事件

了解如何使用[Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/)操作处理接收的AEM事件。 此示例增强了前面的示例[Adobe I/O Runtime Action和AEM Events](runtime-action.md)，请确保您已完成它，然后再继续此示例。

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

在此示例中，事件处理将原始事件数据和接收的事件作为活动消息存储在Adobe I/O Runtime存储中。 但是，如果事件是&#x200B;_修改的内容片段_&#x200B;类型，则它还会调用AEM创作服务来查找修改详细信息。 最后，它在单页应用程序(SPA)中显示事件详细信息。

## 先决条件

要完成本教程，您需要：

- 启用了[AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)的AEM as a Cloud Service环境。 此外，示例[WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project)项目必须部署到该项目。

- 访问[Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/)。

- 本地计算机上已安装[Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)。

- 从前面示例[Adobe I/O Runtime操作和AEM事件](./runtime-action.md#initialize-project-for-local-development)本地初始化的项目。

## AEM Events processor操作

在此示例中，事件处理器[操作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)执行以下任务：

- 将收到的事件解析为活动消息。
- 如果收到的事件是&#x200B;_修改的内容片段_&#x200B;类型，请回调AEM创作服务以查找修改详细信息。
- 在Adobe I/O Runtime存储中保留原始事件数据、活动消息和修改详细信息（如果有）。

要执行上述任务，我们先从向项目添加操作开始，开发JavaScript模块以执行上述任务，最后更新操作代码以使用开发的模块。

有关完整代码，请参阅附加的[WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip)文件，以下部分突出显示关键文件。

### 添加操作

- 要添加操作，请运行以下命令：

  ```bash
  aio app add action
  ```

- 选择`@adobe/generator-add-action-generic`作为操作模板，将操作命名为`aem-event-processor`。

  ![添加操作](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### 开发JavaScript模块

为了执行上述任务，让我们开发以下JavaScript模块。

- `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js`模块确定收到的事件是否为&#x200B;_修改的内容片段_&#x200B;类型。

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js`模块调用AEM作者服务以查找修改详细信息。

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  请参阅[AEM服务凭据教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en)以了解详细信息。 此外，用于管理密钥和操作参数的[App Builder配置文件](https://developer.adobe.com/app-builder/docs/guides/configuration/)。

- `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js`模块将原始事件数据、活动消息和修改详细信息（如果有）存储在Adobe I/O Runtime存储中。

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### 更新操作代码

最后，更新`src/dx-excshell-1/actions/aem-event-processor/index.js`处的操作代码以使用开发的模块。

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## 其他资源

- `src/dx-excshell-1/actions/model`文件夹包含`aemEvent.js`和`errors.js`个文件，操作使用这些文件分别分析收到的事件和处理错误。
- `src/dx-excshell-1/actions/load-processed-aem-events`文件夹包含操作代码，SPA使用此操作从Adobe I/O Runtime存储中加载已处理的AEM事件。
- `src/dx-excshell-1/web-src`文件夹包含SPA代码，该代码显示已处理的AEM事件。
- `src/dx-excshell-1/ext.config.yaml`文件包含操作配置和参数。

## 概念和关键要点

虽然事件处理要求因项目而异，但本示例的关键要求包括：

- 可以使用Adobe I/O Runtime操作完成事件处理。
- 运行时操作可以与系统(如内部应用程序、第三方解决方案和Adobe解决方案)通信。
- 运行时操作是围绕内容更改设计的业务流程的入口点。
