---
title: 将Asset compute工作人员部署到Adobe I/O Runtime，以与AEM一起用作Cloud Service
description: 'asset compute项目及其所包含的工作人员必须部署到Adobe I/O Runtime，由AEM作为Cloud Service。 '
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

asset compute项目及其包含的工作程序必须通过Adobe I/OCLI部署到Adobe I/O Runtime，以便由AEM作为Cloud Service使用。

部署到Adobe I/O Runtime供AEM作为Cloud Service作者服务使用时，只需要两个环境变量：

+ `AIO_runtime_namespace` 指向要部署的Adobe项目Firefly Workspace
+ `AIO_runtime_auth` 是Adobe项目Firefly工作区的身份验证凭据

在`.env`文件中定义的其他标准变量在调用Asset compute工时由AEM隐式提供为Cloud Service。

## 开发工作区

由于此项目是使用`Development`工作区使用`aio app init`生成的，因此在本地`.env`文件中，`AIO_runtime_namespace`会自动设置为`81368-wkndaemassetcompute-development`并且与`AIO_runtime_auth`匹配。  如果`.env`文件存在于用于发出deploy命令的目录中，则使用其值，除非它们通过操作系统级别变量导出（即[舞台和生产](#stage-and-production)工作区的目标）取代。

![使用。env变量部署aio应用程序](./assets/runtime/development__aio.png)

要部署到项目`.env`文件中定义的工作区，请执行以下操作：

1. 打开Asset compute项目根中的命令行
1. 执行命令`aio app deploy`
1. 执行命令`aio app get-url`以获取工作器URL，以在AEM中作为Cloud Service处理用户档案使用，以引用此自定义Asset compute工作器。 如果项目包含多个工作线程，则列出每个工作线程的离散URL。

如果本地开发和AEM作为Cloud Service开发环境使用单独的Asset compute部署，则可以像[Stage和Production部署](#stage-and-production)一样管理部署到AEM作为Cloud Service开发。

## 舞台和生产工作区{#stage-and-production}

部署到舞台和生产工作区通常由您选择的CI/CD系统完成。 asset compute项目必须分散部署到每个Workspace（舞台和生产）。

设置真环境变量将覆盖`.env`中同名变量的值。

![使用导出变量部署aio应用程序](./assets/runtime/stage__export-and-aio.png)

部署到舞台和生产环境的一般方法是：

1. 确保安装了[Adobe I/OCLI npm模块和Asset compute插件](../set-up/development-environment.md#aio)
1. 查看要从Git部署的Asset compute项目
1. 使用与环境工作区（舞台或生产）对应的值设置目标变量
   + 这两个必需变量是`AIO_runtime_namespace`和`AIO_runtime_auth`，它们是通过工作区的&#x200B;__下载全部__&#x200B;功能在Adobe I/O开发人员控制台中按工作区获取的。

![Adobe开发人员控制台- AIO运行时命名空间和身份验证](./assets/runtime/stage-auth-namespace.png)

通过从命令行发出导出命令可以设置这些键的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute工作者需要任何其他变量(如云存储)，则这些变量也应作为环境变量导出。

1. 为要部署的环境工作区设置所有目标变量后，执行部署命令：
   + `aio app deploy`
1. AEM作为Cloud Service处理用户档案引用的工作器URL也可通过以下方式获得：
   + `aio app get-url`。

如果Asset compute项目版本更改了工作器URL，则工作器URL也会更改以反映新版本，并且需要在处理用户档案中更新该URL。

## 工作区API设置{#workspace-api-provisioning}

当[在Adobe I/O设置Adobe项目Firefly项目以支持本地开发时，将创建新的开发工作区，并在其中添加&#x200B;__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__。](../set-up/firefly.md)

__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__ API只显式添加到用于本地开发的工作区。 将AEM（独家）集成为Cloud Service环境的工作区&#x200B;__不__&#x200B;需要显式添加的这些API，因为API可自然地作为Cloud Service提供给AEM。
