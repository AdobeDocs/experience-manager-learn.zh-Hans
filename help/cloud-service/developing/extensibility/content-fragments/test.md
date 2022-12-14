---
title: 测试AEM内容片段控制台扩展
description: 了解如何在部署到生产之前测试AEM内容片段控制台扩展。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---


# 测试扩展

AEM内容片段控制台扩展可以针对该扩展所属的Adobe组织中的任何AEMas a Cloud Service环境进行测试。

测试扩展是通过精心编制的URL完成的，该URL会指示AEM内容片段控制台加载扩展。

## AEM内容片段控制台URL

![AEM内容片段控制台URL](./assets/test/content-fragment-console-url.png){align="center"}

要创建将非生产扩展装载到AEM内容片段控制台的URL，必须收集所需AEM内容片段控制台的URL。 导航到AEMas a Cloud Service环境以在上测试扩展，并复制其AEM内容片段控制台的URL。

1. 登录到所需的AEMas a Cloud Service环境。

   + 使用AEM开发环境 [测试开发版本](#testing-development-builds)
   + 将AEM Stage或QA开发环境用于 [测试阶段构建](#testing-stage-builds)

1. 选择 __内容片段__ 图标。
1. 等待AEM内容片段控制台在浏览器中加载。
1. 从浏览器的地址栏复制AEM内容片段控制台的URL，它应该类似于：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

在制作用于开发和阶段测试的URL时，下面将使用此URL。

## 测试本地开发内部版本

1. 打开扩展项目根目录的命令行。
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

   测试URL应该类似于：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://localhost:9080
   ```

1. 将测试URL复制并粘贴到浏览器中。

   + 您可能最初必须这样做，然后定期 [接受HTTPS证书](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 对于本地应用程序的主机(`https://localhost:9080`)。

1. 加载AEM内容片段控制台时，会将扩展的本地版本插入到该控制台中进行测试，并在本地应用程序生成器应用程序运行期间热重新加载更改。

>[!IMPORTANT]
>
>请记住，使用此方法时，开发中的扩展仅会影响您的体验，而AEM内容片段控制台的所有其他用户则无需插入的扩展即可访问它。


## 测试阶段构建

1. 打开扩展项目根目录的命令行。
1. 确保Stage工作区处于活动状态（或用于测试的任何工作区）。

   ```shell
   $ aio app use -w Stage
   ```
   合并对 `.env` 和 `.aio`.
1. 部署更新的扩展App Builder应用程序。 如果未登录，请运行 `aio login` 第一个。

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

   测试URL应该类似于：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html
   ```

1. 将测试URL复制并粘贴到浏览器中。
1. AEM内容片段控制台会插入部署到中的Stage工作区的扩展版本。 可以将此阶段URL共享给QA或企业用户以进行测试。

请记住，使用此方法时，仅当使用手工暂存URL访问时，暂存扩展才会插入到AEM内容片段控制台的中。

1. 可以通过运行 `aio app deploy` 同样，在使用测试URL时，这些更改会自动反映。
1. 要删除要测试的扩展，请运行 `aio app undeploy`.



