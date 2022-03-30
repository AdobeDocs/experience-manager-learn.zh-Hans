---
title: 将Asset compute工作程序部署到Adobe I/O Runtime以与AEM as a Cloud Service一起使用
description: asset compute项目及其包含的工作程序必须部署到Adobe I/O Runtime，以供AEM as a Cloud Service使用。
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

部署到Adobe I/O Runtime以供AEMas a Cloud Service创作服务使用时，只需要两个环境变量：

+ `AIO_runtime_namespace` 指向要部署的应用程序生成器工作区
+ `AIO_runtime_auth` 是App Builder工作区的身份验证凭据

在 `.env` 文件在调用AEM工作程序时由Asset compute as a Cloud Service隐式提供。

## 开发工作区

由于此项目是使用 `aio app init` 使用 `Development` 工作区， `AIO_runtime_namespace` 自动设置为 `81368-wkndaemassetcompute-development` 匹配 `AIO_runtime_auth` 在我们本地 `.env` 文件。  如果 `.env` 文件存在于用于发出deploy命令的目录中，将使用其值，除非它们通过操作系统级别变量导出来取代，这就是执行此操作的方式 [阶段和生产](#stage-and-production) 已定位工作区。

![使用env变量部署aio应用程序](./assets/runtime/development__aio.png)

要部署到项目中定义的工作区，请执行以下操作 `.env` 文件：

1. 在Asset compute项目的根中打开命令行
1. 执行命令 `aio app deploy`
1. 执行命令 `aio app get-url` 要获取工作程序URL以在AEMas a Cloud Service处理配置文件中使用，以引用此自定义Asset compute工作程序。 如果项目包含多个工作程序，则会列出每个工作程序的离散URL。

如果本地开发和AEMas a Cloud Service开发环境使用单独的Asset compute部署，则对AEMas a Cloud Service开发部署的管理方式与 [暂存和生产部署](#stage-and-production).

## 暂存和生产工作区{#stage-and-production}

部署到舞台和生产工作区通常由您选择的CI/CD系统来完成。 必须将Asset compute项目离散地部署到每个工作区（暂存，然后是生产）。

设置true环境变量将覆盖中同名变量的值 `.env`.

![使用导出变量部署aio应用程序](./assets/runtime/stage__export-and-aio.png)

通常由CI/CD系统自动化的一般方法是：

1. 确保 [Adobe I/OCLI npm模块和Asset compute插件](../set-up/development-environment.md#aio) 已安装
1. 查看要从Git部署的Asset compute项目
1. 使用与目标工作区（暂存或生产）对应的值设置环境变量
   + 这两个必需变量是 `AIO_runtime_namespace` 和 `AIO_runtime_auth` 和是通过Adobe I/O开发人员控制台中的 __全部下载__ 功能。

![Adobe开发人员控制台 — AIO运行时命名空间和身份验证](./assets/runtime/stage-auth-namespace.png)

可以通过从命令行发出导出命令来设置这些键的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute工作程序需要任何其他变量（例如云存储），则这些变量也应导出为环境变量。

1. 在将目标工作区设置为要部署到的所有环境变量后，执行deploy命令：
   + `aio app deploy`
1. AEMas a Cloud Service处理配置文件引用的工作URL也可通过以下方式获得：
   + `aio app get-url`。

如果Asset compute项目版本更改了工作程序URL，则工作程序URL也会更改以反映新版本，并且该URL需要在处理配置文件中进行更新。

## 工作区API配置{#workspace-api-provisioning}

When [在中设置App Builder项目Adobe I/O](../set-up/app-builder.md) 为支持本地开发，创建了一个新的开发工作区，并 __asset compute、 I/O事件__ 和 __I/O事件管理API__ 被添加到其中。

的 __asset compute、 I/O事件__ 和 __I/O事件管理API__ API仅显式添加到用于本地开发的工作区中。 与AEMas a Cloud Service环境集成（仅限）的工作区可以 __not__ 需要明确添加这些API，因为API可自然地供AEMas a Cloud Service使用。
