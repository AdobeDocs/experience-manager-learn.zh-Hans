---
title: 将Asset compute工作程序部署到Adobe I/O Runtime以用于AEMas a Cloud Service
description: asset compute项目及其包含的工作程序必须部署到Adobe I/O Runtime，以供AEMas a Cloud Service使用。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# 部署到Adobe I/O Runtime

asset compute项目及其包含的工作程序必须通过Adobe I/OCLI部署到Adobe I/O Runtime，以供AEMas a Cloud Service使用。

在部署到Adobe I/O Runtime以供AEMas a Cloud Service创作服务使用时，只需两个环境变量：

+ `AIO_runtime_namespace` 指向要部署到的App Builder工作区
+ `AIO_runtime_auth` 是App Builder工作区的身份验证凭据

中定义的其他标准变量 `.env` 文件由AEMas a Cloud Service在调用Asset compute工作进程时隐式提供。

## 开发工作区

因为此项目是使用 `aio app init` 使用 `Development` 工作区， `AIO_runtime_namespace` 自动设置为 `81368-wkndaemassetcompute-development` 具有匹配 `AIO_runtime_auth` 在我们当地 `.env` 文件。  如果 `.env` 文件存在于用于发出deploy命令的目录中，将使用其值，除非通过OS级别变量导出替换这些值，具体情况如下 [暂存和生产](#stage-and-production) 工作区已定位。

![使用.env变量部署aio应用程序](./assets/runtime/development__aio.png)

部署到项目中定义的工作区 `.env` 文件：

1. 在Asset compute项目的根目录中打开命令行
1. 执行命令 `aio app deploy`
1. 执行命令 `aio app get-url` 以获取工作进程URL以在AEMas a Cloud Service处理配置文件中使用以引用此自定义Asset compute工作进程。 如果项目包含多个工作程序，则会列出每个工作程序的离散URL。

如果本地开发环境和AEMas a Cloud Service开发环境使用单独的Asset compute部署，则可以采用与相同的方式管理到AEMas a Cloud Service开发环境的部署 [暂存和生产部署](#stage-and-production).

## 暂存和生产工作区{#stage-and-production}

部署到暂存和生产工作区通常由您选择的CI/CD系统完成。 asset compute项目必须离散部署到每个工作区（暂存然后是生产）。

设置真正的环境变量将覆盖中同名变量的值 `.env`.

![使用导出变量部署aio应用程序](./assets/runtime/stage__export-and-aio.png)

通常由CI/CD系统自动执行，用于部署到暂存和生产环境的一般方法是：

1. 确保 [Adobe I/OCLI npm模块和Asset compute插件](../set-up/development-environment.md#aio) 已安装
1. 查看要从Git部署的Asset compute项目
1. 使用与目标工作区（“暂存”或“生产”）对应的值设置环境变量
   + 两个必需变量为 `AIO_runtime_namespace` 和 `AIO_runtime_auth` 和通过Workspace的，在Adobe I/O开发人员控制台中为每个工作区获取 __全部下载__ 功能。

![Adobe Developer控制台 — AIO运行时命名空间和身份验证](./assets/runtime/stage-auth-namespace.png)

可通过从命令行发出导出命令来设置这些键的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute工作人员需要任何其他变量（例如cloud storage），则这些变量也应当导出为环境变量。

1. 设置目标工作区要部署到的所有环境变量后，执行deploy命令：
   + `aio app deploy`
1. AEMas a Cloud Service处理配置文件引用的工作程序URL也可通过以下方式使用：
   + `aio app get-url`。

如果Asset compute项目版本更改，则工作程序URL也会更改以反映新版本，并且需要在“处理配置文件”中更新该URL。

## 工作区API配置{#workspace-api-provisioning}

时间 [在Adobe I/O中设置应用程序生成器项目](../set-up/app-builder.md) 为了支持本地开发，已创建了一个新的开发工作区，并且 __asset compute、I/O事件__ 和 __I/O事件管理API__ 已添加到其中。

此 __asset compute、I/O事件__ 和 __I/O事件管理API__ API仅显式添加到用于本地开发的工作区。 （独占）与AEMas a Cloud Service环境集成的工作区可以 __非__ 需要明确添加这些API，因为这些API会自然而然地提供给AEMas a Cloud Service。
