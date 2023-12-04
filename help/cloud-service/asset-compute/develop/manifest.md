---
title: 配置Asset compute项目的manifest.yml
description: asset compute项目的manifest.yml描述了此项目中要部署的所有工作程序。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 172
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# 配置manifest.yml

此 `manifest.yml`位于部署项目的根目录中的，描述了此项目中要Asset compute的所有工作程序。

![manifest.yml](./assets/manifest/manifest.png)

## 默认工作人员定义

工作人员在下被定义为Adobe I/O Runtime操作条目 `actions`，由一组配置组成。

访问其他Adobe I/O集成的工作者必须设置 `annotations -> require-adobe-auth` 属性至 `true` 如下所示 [公开工作人员的Adobe I/O凭据](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) 通过 `params.auth` 对象。 当工作进程调用Adobe I/OAPI(例如Adobe Photoshop、Lightroom或Sensei API)时，通常需要此项，并且每个工作进程可以切换。

1. 打开并查看自动生成的工作人员 `manifest.yml`. 包含多个Asset compute工作程序的项目，必须在下为每个工作人员定义一个条目 `actions` 数组。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## 定义限制

每个工作人员可以配置 [限制](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) 以了解其在Adobe I/O Runtime中的执行上下文。 应根据工作人员将计算的资产数量、比率、类型以及所执行的工作类型，调整这些值，以便为工作人员提供最佳规模。

审核 [Adobe大小调整指南](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) 设置限制之前。 asset compute工作程序在处理资源时可能会耗尽内存，从而导致Adobe I/O Runtime执行被终止，因此请确保该工作程序具有适当的大小以处理所有候选资源。

1. 添加 `inputs` 部分 — 新 `wknd-asset-compute` 操作条目。 这允许调整Asset compute工作程序的总体性能和资源分配。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## 完成的manifest.yml

最终的 `manifest.yml` 类似于：

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## Github上的manifest.yml

最终的 `.manifest.yml` 可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 正在验证manifest.yml

生成的Asset compute后 `manifest.yml` ，运行本地开发工具，并确保使用更新后的成功启动 `manifest.yml` 设置。

要为Asset compute项目启动Asset compute开发工具，请执行以下操作：

1. 在Asset compute项目根中打开命令行（在VS代码中，这可以直接在IDE中通过“终端”>“新建终端”打开），然后执行命令：

   ```
   $ aio app run
   ```

1. 本地Asset compute开发工具将在您的默认Web浏览器中打开，网址为 __http://localhost:9000__.

   ![aio应用程序运行](assets/environment-variables/aio-app-run.png)

1. 在开发工具初始化时，请观察命令行输出和Web浏览器中的错误消息。
1. 要停止“Asset compute开发工具”，请点按 `Ctrl-C` 在窗口中执行 `aio app run` 以终止进程。

## 疑难解答

+ [YAML缩进不正确](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制设置过低](../troubleshooting.md#memorysize-limit-is-set-too-low)
