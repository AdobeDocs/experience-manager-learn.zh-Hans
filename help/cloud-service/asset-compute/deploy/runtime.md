---
title: 将资产计算工作者部署到Adobe I/O Runtime，与AEM一起作为Cloud Service
description: '资产计算项目及其包含的工作程序必须部署到Adobe I/O Runtime，由AEM作为Cloud Service使用。 '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---


# 部署到Adobe I/O Runtime

资产计算项目及其包含的工作线程必须通过AdobeI/O CLI部署到Adobe I/O Runtime，以便由AEM用作Cloud Service。

部署到Adobe I/O Runtime供AEM作为Cloud Service作者服务使用时，只需要两个环境变量：

+ `AIO_runtime_namespace` 指向要部署的Adobe项目Firefly Workspace
+ `AIO_runtime_auth` 是Adobe项目Firefly工作区的身份验证凭据

在调用资产计算工作 `.env` 器时，AEM隐式将文件中定义的其他标准变量作为Cloud Service提供。

## 开发工作区

由于此项目是使用工 `aio app init` 作区生 `Development` 成的，因 `AIO_runtime_namespace` 此会自动设置为 `81368-wkndaemassetcompute-development` 与本地文件 `AIO_runtime_auth` 中的匹配项 `.env` 匹配。  如果用 `.env` 于发出deploy命令的目录中存在文件，则使用其值，除非通过操作系统级变量导出取代它们，该导出是舞台和生产工作 [区的目标](#stage-and-production) 。

![使用。env变量部署aio应用程序](./assets/runtime/development__aio.png)

要部署到项目文件中定义的工作区，请执行以 `.env` 下操作：

1. 打开资产计算项目根目录中的命令行
1. 执行命令 `aio app deploy`
1. 执行命令以 `aio app get-url` 获取工作器URL，以在AEM中用作Cloud Service处理用户档案，以引用此自定义资产计算工作器。 如果项目包含多个工作线程，则列出每个工作线程的离散URL。

如果本地开发和AEM作为Cloud Service开发环境使用单独的资产计算部署，则作为Cloud Service开发的AEM部署可以与Stage和Production部署相 [同的方式进行管理](#stage-and-production)。

## 舞台和生产工作区{#stage-and-production}

部署到舞台和生产工作区通常由您选择的CI/CD系统完成。 资产计算项目必须分别部署到每个工作区（舞台和生产）。

设置真环境变量将覆盖中同名变量的值 `.env`。

![使用导出变量部署aio应用程序](./assets/runtime/stage__export-and-aio.png)

部署到舞台和生产环境的一般方法是：

1. 确保 [AdobeI/O CLI npm模块和Asset Compute插件已安装](../set-up/development-environment.md#aio)
1. 查看要从Git部署的资产计算项目
1. 使用与环境工作区（舞台或生产）对应的值设置目标变量
   + 这两个必需的变量是 `AIO_runtime_namespace` 和 `AIO_runtime_auth` 通过AdobeI/O开发人员控制台中的每个工作区的“下载全部”功能 __获取的__ 。

![Adobe开发人员控制台- AIO运行时命名空间和身份验证](./assets/runtime/stage-auth-namespace.png)

通过从命令行发出导出命令可以设置这些键的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的资产计算工作线程需要任何其他变量(如在云存储)，则这些变量也应作为环境变量导出。

1. 为要部署的环境工作区设置所有目标变量后，执行部署命令：
   + `aio app deploy`
1. AEM作为Cloud Service处理用户档案引用的工作器URL也可通过以下方式获得：
   + `aio app get-url`。

如果资产计算项目版本更改了工作器URL，则工作器URL也会更改以反映新版本，并且需要在处理用户档案中更新该URL。

## 工作区API设置{#workspace-api-provisioning}

在 [AdobeI/O中设置Adobe项目Firefly项目以支持本地开发](../set-up/firefly.md) ，将创建一个新的开发工作区，并将 __资产计算、I/O事件____和I/O事件管理API添加到该工作区__ 。

资产 __计算、I/O事件____和I/O事件管理API__ API只显式添加到用于本地开发的工作区。 将AEM（独家）集成为Cloud Service环境的工作区 __不需__ 要显式添加的这些API，因为API是作为Cloud Service自然提供的。
