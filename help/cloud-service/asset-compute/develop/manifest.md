---
title: 配置资产计算项目的manifest.yml
description: 资产计算项目的manifest.yml描述了要部署的此项目中的所有Worker。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# 配置manifest.yml

位 `manifest.yml`于资产计算项目根目录中，描述了要部署的此项目中的所有工作线程。

![manifest.yml](./assets/manifest/manifest.png)

## 默认工作器定义

Worker定义为“Adobe I/O Runtime”操作 `actions`条目，由一组配置组成。

访问其他AdobeI/O集成的Worker必须将 `annotations -> require-adobe-auth` 属性 `true` 设置为， [因为这会通过对象显示Worker的AdobeI](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) /O `params.auth` 凭据。 当工作人员调用AdobeI/O API(如Adobe Photoshop、Lightroom或Sensei API)时，通常需要此参数，并且可以根据工作人员切换。

1. 打开并查看自动生成的工作人员 `manifest.yml`。 包含多个资产计算工作线程的项目，必须为阵列下的每个工作线程定义一个 `actions` 条目。

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

每个工作者都可以在 [Adobe I/O Runtime](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) ，为其执行上下文配置限制。 应根据员工要计算的资产量、比率和类型以及所执行的工作类型，调整这些值以为员工提供最佳规模。

在设置 [限制前查看Adobe](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) 大小调整指南。 资产计算工作人员在处理资产时可能内存不足，导致Adobe I/O Runtime执行被杀，因此请确保该工作人员的大小适合处理所有候选资产。

1. 向新操 `inputs` 作条目中添 `wknd-asset-compute` 加一个部分。 这允许调整资产计算工作者的总体性能和资源分配。

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

## 完成的清单。yml

最终结果 `manifest.yml` 如下：

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

## manifest.yml on Github

最后一 `.manifest.yml` 节在Github上提供，网址为：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 验证manifest.yml

更新生成的资产计 `manifest.yml` 算后，运行本地开发工具，并确保使用更新的设置成功开始 `manifest.yml` 用户。

要开始资产计算项目的资产计算开发工具，请执行以下操作：

1. 在“资产计算”项目根目录中打开命令行（在VS代码中，可以通过“终端”>“新建终端”在IDE中直接打开它），然后执行命令：

   ```
   $ aio app run
   ```

1. 本地资产计算开发工具将在您的默认Web浏览器中打开，网 __址为http://localhost:9000__。

   ![aio应用程序运行](assets/environment-variables/aio-app-run.png)

1. 当开发工具初始化时，观察命令行输出和Web浏览器是否有错误消息。
1. 要停止资产计算开发工具，请点 `Ctrl-C` 按已执行的窗口以 `aio app run` 终止该流程。

## 疑难解答

+ [YAML缩进不正确](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制设置得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
