---
title: 验证AEM UI扩展
description: 了解如何在部署到生产环境之前预览、测试和验证AEM UI扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 0%

---

# 验证扩展

AEM UI扩展可以根据扩展所属的Adobe组织中的任何AEMas a Cloud Service环境进行验证。

测试扩展是通过专门构建的URL完成的，该URL指示AEM加载扩展（仅针对该请求）。

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> 上面的视频展示了如何使用内容片段控制台扩展来说明App Builder扩展应用程序预览和验证。 但是，请务必注意，所涵盖的概念可应用于所有AEM UI扩展。

## AEM UI URL

![AEM内容片段控制台URL](./assets/verify/content-fragment-console-url.png){align="center"}

要创建将非生产扩展挂载到AEM中的URL，必须获取插入该扩展的AEM UI的URL。 导航到AEMas a Cloud Service环境以验证扩展，然后打开要在其中预览扩展的UI。

例如，要预览内容片段控制台的扩展，请执行以下操作：

1. 登录到所需的AEMas a Cloud Service环境。
2. 选择 __内容片段__ 图标。
3. 等待在浏览器中加载AEM内容片段控制台。
4. 从浏览器的地址栏复制AEM内容片段控制台的URL，它应类似于：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

在精心构建URL以进行开发和暂存验证时，将使用此URL。 如果针对其他AEM UI验证该扩展，请获取这些URL并应用以下相同步骤。

## 验证本地开发构建

1. 打开指向扩展项目根目录的命令行。
1. 将AEM UI扩展作为本地App Builder应用程序运行

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

请注意本地应用程序URL，如上所示 `-> https://localhost:9080`

1. 将以下两个查询参数添加到 [AEM UI的URL](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`，通常 `&ext=https://localhost:9080`.

   添加上述两个查询参数(`devMode` 和 `ext`)作为 __第一__ url中的查询参数。 AEM可扩展UI的使用哈希路由(`#/@wknd/aem/...`)，因此在 `#` 不起作用。

   预览URL应如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

2. 将预览URL复制并粘贴到浏览器中。

   + 你一开始可能得做，然后定期， [接受HTTPS证书](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 对于本地应用程序的主机(`https://localhost:9080`)。

3. AEM UI将加载并插入扩展的本地版本以进行验证。

>[!IMPORTANT]
>
>请记住，在使用此方法时，正在开发的扩展只会影响您的体验，而AEM UI的所有其他用户体验的UI没有插入的扩展。

## 验证阶段生成

1. 打开指向扩展项目根目录的命令行。
1. 确保暂存工作区处于活动状态（或使用任何用于验证的工作区）。

   ```shell
   $ aio app use -w Stage
   ```

   将任何更改合并到 `.env` 和 `.aio`.

1. 部署更新的扩展App Builder应用程序。 如果未登录，则运行 `aio login` 首先。

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. 将以下两个查询参数添加到 [AEM UI的URL](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   添加上述两个查询参数(`devMode` 和 `ext`)作为 __第一__ URL中的查询参数，因为可扩展的AEM UI使用哈希路由(`#/@wknd/aem/...`)，因此在 `#` 不起作用。

   预览URL应如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 将预览URL复制并粘贴到浏览器中。
1. AEM内容片段控制台会注入部署到中的暂存工作区的扩展版本。 可以将此暂存URL共享给QA或商业用户进行验证。

请记住，在使用此方法时，仅当使用手工暂存URL访问时，暂存扩展才会插入到AEM内容片段控制台中。

1. 可以通过运行来更新已部署的扩展 `aio app deploy` 再次重申，并且这些更改会在使用预览URL时自动反映出来。
1. 要删除扩展以进行验证，请运行 `aio app undeploy`.

## 预览小书签

为便于创建上述预览和预览URL，可创建加载扩展的JavaScript小书签。

下面的小书签预览了 [本地开发构建](#verify-local-development-builds) 的扩展 `https://localhost:9080`. 预览 [暂存内部版本](#verify-stage-builds)，使用创建小书签 `previewApp` 变量设置为已部署的App Builder应用程序的URL。

1. 在浏览器中创建书签。
2. 编辑书签。
3. 为书签提供一个有意义的名称，例如 `AEM UI Extension Preview (localhost:9080)`.
4. 将书签的URL设置为以下代码：

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

5. 导航到可扩展的AEM UI以加载预览扩展，然后单击小书签。

>[!TIP]
>
> 如果App Builder扩展未加载，则在使用时， `&ext=https://localhost:9080`，直接在浏览器选项卡中打开该主机和端口，并接受自签名证书。 然后再次尝试使用小书签。
