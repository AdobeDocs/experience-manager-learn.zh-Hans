---
title: 配置Asset compute项目的manifest.yml
description: asset compute项目的manifest.yml描述了此项目中要部署的所有工作程序。
feature: asset compute微服务
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: 集成、开发
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# 配置manifest.yml

位于Asset compute项目根目录的`manifest.yml`描述了此项目中要部署的所有工作程序。

![manifest.yml](./assets/manifest/manifest.png)

## 默认工作程序定义

工作程序在`actions`下定义为Adobe I/O Runtime操作条目，由一组配置组成。

访问其他Adobe I/O集成的工作程序必须将`annotations -> require-adobe-auth`属性设置为`true`，因为此[通过`params.auth`对象公开工作程序的Adobe I/O凭据](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis)。 当工作人员发出Adobe I/OAPI(如Adobe Photoshop、Lightroom或Sensei API)时，通常需要此功能，并且可以按工作人员切换该功能。

1. 打开并查看自动生成的工作程序`manifest.yml`。 包含多个Asset compute工作程序的项目必须在`actions`阵列下为每个工作程序定义一个条目。

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

每个工作者都可以在Adobe I/O Runtime中为其执行上下文配置[limits](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md)。 应根据工作人员要计算的资产数量、速率和类型以及所执行的工作类型，对这些值进行调整，以便为工作人员提供最佳大小调整。

在设置限制之前，请查看[Adobe大小调整指南](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers)。 asset compute工作程序在处理资产时可能内存不足，导致Adobe I/O Runtime执行被终止，因此请确保该工作程序的大小适合处理所有候选资产。

1. 在新的`wknd-asset-compute`操作条目中添加`inputs`部分。 这允许调整Asset compute工作者的总体性能和资源分配。

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

## 已完成manifest.yml

最终的`manifest.yml`如下所示：

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

最终`.manifest.yml`可在Github上获取，网址为：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 验证manifest.yml

更新生成的Asset compute`manifest.yml`后，运行本地开发工具，并确保使用更新的`manifest.yml`设置成功启动。

要为Asset compute项目启动Asset compute开发工具，请执行以下操作：

1. 在Asset compute项目根目录中打开命令行（在VS代码中，可以通过“终端”>“新终端”直接在IDE中打开此命令），然后执行命令：

   ```
   $ aio app run
   ```

1. 本地Asset compute开发工具将在默认Web浏览器中的&#x200B;__http://localhost:9000__&#x200B;打开。

   ![aio应用程序运行](assets/environment-variables/aio-app-run.png)

1. 当开发工具初始化时，请观看命令行输出和Web浏览器中的错误消息。
1. 要停止Asset compute开发工具，请点按执行`aio app run`的窗口中的`Ctrl-C`以终止该进程。

## 疑难解答

+ [YAML缩进不正确](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制设置过低](../troubleshooting.md#memorysize-limit-is-set-too-low)
