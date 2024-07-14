---
title: 将Asset compute工作人员部署到Adobe I/O Runtime以与AEM as a Cloud Service一起使用
description: asset compute项目及其包含的工作程序必须部署到Adobe I/O Runtime才可供AEM as a Cloud Service使用。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 128
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# 部署到Adobe I/O Runtime

asset compute项目及其包含的工作程序必须通过Adobe I/OCLI部署到Adobe I/O Runtime，以供AEM as a Cloud Service使用。

在部署到Adobe I/O Runtime以供AEM as a Cloud Service Author服务使用时，只需要两个环境变量：

+ `AIO_runtime_namespace`指向App Builder Workspace部署到
+ `AIO_runtime_auth`是App Builder工作区的身份验证凭据

在调用Asset compute工作进程时，`.env`文件中定义的其他标准变量由AEM as a Cloud Service隐式提供。

## 开发工作区

由于此项目是使用`aio app init`通过`Development`工作区生成的，因此`AIO_runtime_namespace`自动设置为`81368-wkndaemassetcompute-development`，并在我们的本地`.env`文件中具有匹配的`AIO_runtime_auth`。  如果用于发出部署命令的目录中存在一个`.env`文件，则使用其值，除非通过操作系统级别变量导出来取代这些值，这就是[暂存和生产](#stage-and-production)工作区的目标。

使用.env变量部署![aio应用程序](./assets/runtime/development__aio.png)

要部署到项目`.env`文件中定义的工作区，请执行以下操作：

1. 在Asset compute项目的根目录中打开命令行
1. 执行命令`aio app deploy`
1. 执行命令`aio app get-url`以获取工作程序URL，以便在AEM as a Cloud Service处理配置文件中用于引用此自定义Asset compute工作程序。 如果项目包含多个工作进程，则会列出每个工作进程的独立URL。

如果本地开发环境和AEM as a Cloud Service开发环境使用单独的Asset compute部署，则可以采用与[暂存和生产部署](#stage-and-production)相同的方式管理到AEM as a Cloud Service开发环境的部署。

## 暂存和生产工作区{#stage-and-production}

部署到暂存和生产工作区通常由您选择的CI/CD系统完成。 asset compute项目必须离散地部署到每个Workspace（暂存环境，然后是生产环境）。

设置true环境变量将覆盖`.env`中同名变量的值。

使用导出变量部署![aio应用程序](./assets/runtime/stage__export-and-aio.png)

通常由CI/CD系统自动执行，用于部署到暂存和生产环境的常规方法是：

1. 确保已安装[Adobe I/OCLI npm模块和Asset compute插件](../set-up/development-environment.md#aio)
1. 从Git中签出要部署的Asset compute项目
1. 使用与目标工作区（“暂存”或“生产”）对应的值设置环境变量
   + 两个必需变量是`AIO_runtime_namespace`和`AIO_runtime_auth`，通过Workspace的&#x200B;__全部下载__&#x200B;功能在Adobe I/ODeveloper Console中为每个工作区获取。

![Adobe Developer Console - AIO运行时命名空间和身份验证](./assets/runtime/stage-auth-namespace.png)

可通过从命令行发出导出命令来设置这些键的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute工作人员需要任何其他变量（如云存储中的变量），则也应将这些变量作为环境变量导出。

1. 在为要部署到的目标工作区设置所有环境变量后，执行deploy命令：
   + `aio app deploy`
1. AEM as a Cloud Service处理配置文件引用的工作程序URL也可通过以下方式使用：
   + `aio app get-url`。

如果Asset compute项目版本发生更改，则工作进程URL也会发生更改以反映新版本，并且需要在“处理配置文件”中更新该URL。

## Workspace API配置{#workspace-api-provisioning}

当[在Adobe I/O](../set-up/app-builder.md)中设置App Builder项目以支持本地开发时，已创建新的开发工作区，并已将&#x200B;__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__&#x200B;添加到该工作区。

__Asset compute、I/O事件__&#x200B;和&#x200B;__I/O事件管理API__ API仅显式添加到用于本地开发的工作区。 与AEM as a Cloud Service环境集成（独占）的工作区&#x200B;__不__&#x200B;需要明确添加这些API，因为API自然可供AEM as a Cloud Service使用。
