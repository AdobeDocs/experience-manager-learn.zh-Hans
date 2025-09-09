---
title: 配置Asset Compute项目的manifest.yml
description: Asset Compute项目的manifest.yml描述了此项目中要部署的所有工作程序。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# 配置 manifest.yml

位于Asset Compute项目根目录中的`manifest.yml`描述了此项目中要部署的所有工作程序。

![manifest.yml](./assets/manifest/manifest.png)

## 默认工作人员定义

辅助进程被定义为`actions`下的Adobe I/O Runtime操作条目，由一组配置组成。

访问其他Adobe I/O集成的工作程序必须将`annotations -> require-adobe-auth`属性设置为`true`，因为此[通过](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)对象公开此工作程序的Adobe I/O凭据`params.auth`。 当工作进程调用Adobe I/O API(例如Adobe Photoshop或Lightroom API)时，通常需要此项，并且每个工作进程可以切换。

1. 打开并查看自动生成的辅助进程`manifest.yml`。 包含多个Asset Compute工作程序的项目，必须为`actions`数组下的每个工作程序定义一个条目。

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
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, or Lightroom.
```

## 定义限制

每个辅助进程都可以在Adobe I/O Runtime中为其执行上下文配置[限制](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)。 应根据工作人员将计算的资产数量、比率、类型以及所执行的工作类型，调整这些值，以便为工作人员提供最佳规模。

在设置限制之前查看[Adobe大小调整指南](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers)。 Asset Compute工作进程在处理资产时可能会耗尽内存，从而导致Adobe I/O Runtime执行被终止，因此请确保该工作进程的大小适合处理所有候选资产。

1. 向新的`inputs`操作条目添加`wknd-asset-compute`部分。 这允许调整Asset Compute工作程序的整体性能和资源分配。

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

最终`manifest.yml`如下所示：

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

Github上的最终`.manifest.yml`位于：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 正在验证manifest.yml

更新生成的Asset Compute `manifest.yml`后，运行本地开发工具并确保使用更新的`manifest.yml`设置成功启动。

要启动适用于Asset Compute项目的Asset Compute开发工具，请执行以下操作：

1. 在Asset Compute项目根目录中打开命令行（在VS Code中，这可以直接在IDE中通过“终端”>“新建终端”打开），然后执行命令：

   ```
   $ aio app run
   ```

1. 本地Asset Compute开发工具将在默认Web浏览器中打开，网址为__http://localhost :9000__。

   ![aio应用运行](assets/environment-variables/aio-app-run.png)

1. 在开发工具初始化时，请观察命令行输出和Web浏览器中的错误消息。
1. 要停止Asset Compute开发工具，请点按执行`Ctrl-C`的窗口中的`aio app run`以终止进程。

## 疑难解答

+ [YAML缩进不正确](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制设置过低](../troubleshooting.md#memorysize-limit-is-set-too-low)
