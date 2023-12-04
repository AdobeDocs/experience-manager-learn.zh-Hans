---
title: 批量属性更新示例AEM内容片段控制台扩展
description: 批量更新内容片段属性的示例AEM内容片段控制台扩展。
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: fbfb5c10-95f8-4875-88dd-9a941d7a16fd
duration: 1550
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---

# 批量属性更新示例扩展

>[!VIDEO](https://video.tv.adobe.com/v/3412296?quality=12&learn=on)

此示例AEM内容片段控制台扩展是 [操作栏](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) 用于将内容片段属性批量更新为通用值的扩展。

示例扩展的功能流程如下所示：

![Adobe I/O Runtime操作流程](./assets/bulk-property-update/flow.png){align="center"}

1. 选择内容片段，然后单击中扩展的按钮 [操作栏](#extension-registration) 打开 [模态](#modal).
2. 此 [模态](#modal) 显示使用构建的自定义输入表单 [React频谱](https://react-spectrum.adobe.com/react-spectrum/).
3. 提交表单会将选定内容片段的列表和AEM主机发送到 [自定义Adobe I/O Runtime操作](#adobe-io-runtime-action).
4. 此 [Adobe I/O Runtime操作](#adobe-io-runtime-action) 验证输入并向AEM发出HTTPPUT请求以更新选定的内容片段。
5. 用于更新指定属性的每个内容片段的一系列HTTPPUT。
6. AEMas a Cloud Service会保留对内容片段的属性更新，并返回对Adobe I/O Runtime操作的成功或失败响应。
7. 该模式窗口收到了Adobe I/O Runtime操作的响应，并显示成功批量更新的列表。

## 扩展点

此示例将扩展到扩展点 `actionBar` 向内容片段控制台添加自定义按钮。

| AEM UI已扩展 | 扩展点 |
| ------------------------ | --------------------- | 
| [内容片段控制台](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [操作栏](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |


## 扩展示例

该示例使用现有的Adobe Developer Console项目，并在通过初始化App Builder应用程序时使用以下选项 `aio app init`.

+ 您要搜索哪些模板？ `All Extension Points`
+ 选择要安装的模板：` @adobe/aem-cf-admin-ui-ext-tpl`
+ 您希望如何命名扩展？： `Bulk property update`
+ 请提供扩展的简短描述： `An example action bar extension that bulk updates a single property one or more content fragments.`
+ 您希望从哪个版本开始？： `0.0.1`
+ 您接下来想要做什么？
   + `Add a custom button to Action Bar`
      + 请提供按钮的标签名称： `Bulk property update`
      + 是否需要显示按钮的模式窗口？ `y`
   + `Add server-side handler`
      + Adobe I/O Runtime允许您根据需要调用无服务器代码。 您希望如何命名此操作？： `generic`

生成的App Builder扩展应用程序已更新，如下所述。

### 应用程序路由{#app-routes}

此 `src/aem-cf-console-admin-1/web-src/src/components/App.js` 包含 [React路由器](https://reactrouter.com/en/main).

路由有两种逻辑集：

1. 第一个路由将请求映射到 `index.html`，它会调用负责的React组件 [扩展注册](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. 第二组路由将URL映射到渲染扩展模式内容的React组件。 此 `:selection` param表示分隔列表内容片段路径。

   如果扩展具有多个用于调用离散操作的按钮，则 [扩展注册](#extension-registration) 映射到此处定义的路由。

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

### 延期注册

`ExtensionRegistration.js`，映射到 `index.html` route，是AEM扩展的入口点，并定义：

1. 扩展按钮的位置将显示在AEM创作体验中(`actionBar` 或 `headerMenu`)
1. 中扩展按钮的定义 `getButton()` 函数
1. 按钮的点击处理程序，位于 `onClick()` 函数

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'bulk-property-update',     // Unique ID for the button
              'label': 'Bulk property update',  // Button label 
              'icon': 'Edit'                    // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/bulk-property-update",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|'))
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Bulk property update",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

### 模态

扩展的每个路由，如中所定义 [`App.js`](#app-routes)，映射到在扩展的模式中呈现的React组件。

在此示例应用程序中，有一个模式React组件(`BulkPropertyUpdateModal.js`)具有三种状态：

1. 正在加载，指示用户必须等待
1. 批量属性更新表单，允许用户指定要更新的属性名称和值
1. 批量属性更新操作的响应，其中列出了已更新的内容片段和无法更新的内容片段

很重要的一点是，与扩展中的AEM的任何交互都应委派给 [AppBuilder Adobe I/O Runtime操作](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)，这是一个单独的无服务器进程，运行于 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
使用Adobe I/O Runtime操作与AEM通信是为了避免跨源资源共享(CORS)连接问题。

提交批量属性更新表单后，自定义表单 `onSubmitHandler()` 调用Adobe I/O Runtime操作，传递当前AEM主机（域）和用户的AEM访问令牌，这反过来调用 [AEM内容片段API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) 以更新内容片段。

当收到来自Adobe I/O Runtime操作的响应时，模式会更新以显示批量属性更新操作的结果。

+ `src/aem-cf-console-admin-1/web-src/src/components/BulkPropertyUpdateModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Form,
  Provider,
  Content,
  defaultTheme,
  ContextualHelp,
  Text,
  TextField,
  ButtonGroup,
  Button,
  Heading,
  ListView,
  Item,
} from '@adobe/react-spectrum'

import Spinner from "./Spinner"

import { useParams } from "react-router-dom"

import allActions from '../config.json'
import actionWebInvoke from '../utils'

import { extensionId } from "./Constants"

export default function BulkPropertyUpdateModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState()
  
  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  const [propertyName, setPropertyName] = useState(null);
  const [propertyValue, setPropertyValue] = useState(null);
  const [validationState, setValidationState] = useState({});

  // Get the selected content fragment paths from the route parameter `:selection`
  let { selection } = useParams();
  let fragmentIds = selection?.split('|') || [];
  
  console.log('Content Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error("Unable to locate a list of Content Fragments to update.")
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])


  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (actionInvokeInProgress) {
    // If the bulk property action has been invoked but not completed, display a loading spinner
    return <Spinner />
  } else if (actionResponse) {
    // If the bulk property action has completed, display the response
    return renderResponse();
  } else {
    // Else display the bulk property update form
    return renderForm();
  }

  /**
   * Renders the Bulk Property Update form. 
   * This form has two fields:
   * - Property Name: The name of the Content Fragment property name to update
   * - Property Value: the value the Content Fragment property, specified by the Property Name, will be updated to
   * 
   * @returns the Bulk Property Update form
   */
  function renderForm() {
    return (
      // Use React Spectrum components to render the form
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">
          <Flex width="100%">
            <Form 
              width="100%">
              <TextField label="Property name"
                          isRequired={true}
                          validationState={validationState?.propertyName}
                onChange={setPropertyName}
                contextualHelp={
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>The <strong>Property name</strong> must be a valid for the Content Fragment model(s) the selected Content Fragments implement.</Text>
                    </Content>
                  </ContextualHelp>
                } />

              <TextField
                label="Property value"
                validationState={validationState?.propertyValue}
                onChange={setPropertyValue} />

              <ButtonGroup align="start" marginTop="size-200">
                <Button variant="cta" onPress={onSubmitHandler}>Update {fragmentIds.length} Content Fragments</Button>
              </ButtonGroup>
            </Form>
          </Flex>

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>
    )
  }
  /**
   * Display the response from the Adobe I/O Runtime action in the modal.
   * This includes:
   * - A list of content fragments that were updated successfully
   * - A list a content fragments that failed to update
   * 
   * @returns the response view
   */
  function renderResponse() {
    // Separate the successful and failed content fragments updates
    const successes = actionResponse.filter(item => item.status === 200);
    const failures = actionResponse.filter(item => item.status !== 200);

    return (
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">

          <Text>Bulk updated property <strong>{propertyName}</strong> with value <strong>{propertyValue}</strong></Text>

          {/* Render the list of content fragments that were updated successfully */}
          {successes.length > 0 &&
            <><Heading level="4">{successes.length} Content Fragments successfully updated</Heading>
              <ListView
                items={successes}
                selectionMode="none"
                aria-label="Successful updates"
              >
                {(item) => (
                  <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                    {item.fragmentId.split('/').pop()}
                  </Item>
                )}
              </ListView></>}

          {/* Render the list of content fragments that failed to update */}
          {failures.length > 0 &&
            <><Heading level="4">{failures.length} Content Fragments failed to update</Heading><ListView
              items={failures}
              selectionMode="none"
              aria-label="Failed updates"
            >
              {(item) => (
                <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                  {item.fragmentId.split('/').pop()}
                </Item>
              )}
            </ListView></>}

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>);
  }

  /**
   * Provide a close button for the modal, else it cannot be closed (without refreshing the browser window)
   * 
   * @returns a button that closes the modal.
   */
   function renderCloseButton() {
    return (
      <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
        <ButtonGroup align="end">
          <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
        </ButtonGroup>
      </Flex>
    );
  }

  /**
   * Handle the Bulk Property Update form submission.
   * This function calls the supporting Adobe I/O Runtime action to update the selected Content Fragments, and then returns the response for display in the modal
   * When invoking the Adobe I/O Runtime action, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The list of Content Fragment paths to update
   * - The Content Fragment property name to update
   * - The value to update the Content Fragment property to
   * 
   * @returns a list of content fragment update successes and failures
   */
  async function onSubmitHandler() {
    // Validate the form input fields
    if (propertyName?.length > 1) {
      setValidationState({propertyName: 'valid', propertyValue: 'valid'});
    } else {
      setValidationState({propertyName: 'invalid', propertyValue: 'valid'});
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    console.log('headers', headers);

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentIds: fragmentIds,
      propertyName: propertyName,
      propertyValue: propertyValue
    };

    // Invoke the Adobe I/O Runtime action named `generic`. This name defined in the `ext.config.yaml` file.
    const action = 'generic';

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      const actionResponse = await actionWebInvoke(allActions[action], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);

      console.log(`Response from ${action}:`, actionResponse)
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```


### Adobe I/O Runtime操作

AEM扩展应用程序生成器应用程序可以定义或使用0个或多个Adobe I/O Runtime操作。
Adobe运行时操作应负责需要与AEM或其他AdobeWeb服务交互的工作。

在此示例应用程序中，Adobe I/O Runtime操作 — 使用默认名称 `generic`  — 负责：

1. 向AEM内容片段API发出一系列HTTP请求以更新内容片段。
1. 收集这些HTTP请求的响应，将其归类为成功和失败
1. 返回成功和失败列表以供模式显示(`BulkPropertyUpdateModal.js`)

+ `src/aem-cf-console-admin-1/actions/generic/index.js`


```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

// main function that will be executed by Adobe I/O Runtime
async function main (params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action')

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params))

    // check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'fragmentIds', 'propertyName', 'propertyValue' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    const body = {
      "properties": {
        "elements": {
          [params.propertyName]: {
            "value": params.propertyValue
          }
        }
      }
    };

    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    let results = await Promise.all(params.fragmentIds.map(async (fragmentId) => {

      logger.info(`Updating fragment ${fragmentId} with property ${params.propertyName} and value ${params.propertyValue}`);

      const res = await fetch(`${params.aemHost}${fragmentId.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    const response = {
      statusCode: 200,
      body: results
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the A
     return response;

  } catch (error) {
    // log any server errors
    logger.error(error)
    // return with 500
    return errorResponse(500, 'server error', logger)
  }
}

exports.main = main
```
