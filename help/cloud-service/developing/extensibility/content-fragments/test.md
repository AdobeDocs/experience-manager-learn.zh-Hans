---
title: 测试AEM内容片段控制台扩展
description: 了解如何在部署到生产环境之前测试AEM内容片段控制台扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# 测试扩展

AEM内容片段控制台扩展可以根据扩展所属的Adobe组织中的任何AEMas a Cloud Service环境进行测试。

测试扩展是通过巧尽心思构建的URL完成的，该URL会指示AEM内容片段控制台加载该扩展。

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

## AEM内容片段控制台URL

![AEM内容片段控制台URL](./assets/test/content-fragment-console-url.png){align="center"}

要创建将非生产扩展挂载到AEM内容片段控制台的URL，必须收集所需的AEM内容片段控制台的URL。 导航到AEMas a Cloud Service环境以测试扩展，并复制其AEM内容片段控制台的URL。

1. 登录到所需的AEMas a Cloud Service环境。

   + 将AEM开发环境用于 [测试开发构建](#testing-development-builds)
   + 将AEM Stage或QA开发环境用于 [测试阶段生成](#testing-stage-builds)

1. 选择 __内容片段__ 图标。
1. 等待在浏览器中加载AEM内容片段控制台。
1. 从浏览器的地址栏复制AEM内容片段控制台的URL，它应类似于：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

在为“开发”和“暂存”测试编写URL时，将使用此URL。

## 测试本地开发构建

1. 打开指向扩展项目根目录的命令行。
1. 将AEM内容片段控制台扩展作为本地应用程序生成器应用程序运行

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

1. 将以下两个查询参数添加到 [AEM内容片段控制台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`，通常 `&ext=https://localhost:9080`.

   添加上述两个查询参数(`devMode` 和 `ext`)作为 __第一__ URL中的查询参数，因为内容片段控制台使用哈希路由(`#/@wknd/aem/...`)，因此在 `#` 行不通。

   测试URL应如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 将测试URL复制并粘贴到浏览器中。

   + 您可能必须首先执行，然后定期执行 [接受HTTPS证书](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 对于本地应用程序的主机(`https://localhost:9080`)。

1. AEM内容片段控制台加载时将插入扩展的本地版本进行测试，并且只要本地App Builder应用程序正在运行，就可以热重新加载所做的更改。

>[!IMPORTANT]
>
>请记住，在使用此方法时，正在开发的扩展只会影响您的体验，而AEM内容片段控制台的所有其他用户无需插入扩展即可访问该扩展。


## 测试阶段构建

1. 打开指向扩展项目根目录的命令行。
1. 确保暂存工作区处于活动状态（或任何用于测试的工作区）。

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

1. 将以下两个查询参数添加到 [AEM内容片段控制台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   添加上述两个查询参数(`devMode` 和 `ext`)作为 __第一__ URL中的查询参数，因为内容片段控制台使用哈希路由(`#/@wknd/aem/...`)，因此在 `#` 行不通。

   测试URL应如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 将测试URL复制并粘贴到浏览器中。
1. AEM内容片段控制台会注入部署到中的暂存工作区的扩展版本。 可以将此暂存URL共享给QA或商业用户进行测试。

请记住，在使用此方法时，仅当使用手工暂存URL访问时，暂存扩展才会插入到AEM内容片段控制台中。

1. 可以通过运行来更新已部署的扩展 `aio app deploy` 再次重申，并且这些更改会在使用测试URL时自动反映。
1. 要删除扩展进行测试，请运行 `aio app undeploy`.
