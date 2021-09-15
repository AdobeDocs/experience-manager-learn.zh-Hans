---
title: 调试Asset compute工作程序
description: asset compute工作程序可以通过多种方式进行调试，从简单的调试日志语句，到作为远程调试器附加的VS Code，再到为从AEM as a Cloud Service启动的Adobe I/O Runtime中的激活提取日志。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# 调试Asset compute工作程序

asset compute工作程序可以通过多种方式进行调试，从简单的调试日志语句，到作为远程调试器附加的VS Code，再到为从AEM as a Cloud Service启动的Adobe I/O Runtime中的激活提取日志。

## 日志记录

调试Asset compute工作程序的最基本形式在工作程序代码中使用传统的`console.log(..)`语句。 `console` JavaScript对象是隐式的全局对象，因此无需导入或要求导入，因为它始终存在于所有上下文中。

根据Asset compute工作程序的执行方式，这些日志语句可以以不同的方式进行审阅：

+ 从`aio app run`开始，将打印日志打印到标准输出日志，并将[开发工具的](../develop/development-tool.md)激活日志记录到
   ![aio app run console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 从`aio app test`，将打印到`/build/test-results/test-worker/test.log`的日志
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 使用`wskdebug`时，日志语句将打印到VS代码调试控制台（查看>调试控制台），标准输出
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 使用`aio app logs`，日志语句将打印到激活日志输出

## 通过附加的调试器进行远程调试

>[!WARNING]
>
>使用Microsoft Visual Studio代码1.48.0或更高版本以与wskdebug兼容

[wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模块支持将调试器附加到Asset compute工作程序，包括在VS代码中设置断点并逐步完成代码。

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_使用wskdebug调试Asset compute工作程序的点进（无音频）_

1. 确保安装了[wskdebug](../set-up/development-environment.md#wskdebug)和[ngrok](../set-up/development-environment.md#ngork) npm模块
1. 确保安装并运行[Docker Desktop和支持的Docker映像](../set-up/development-environment.md#docker)
1. 关闭开发工具的任何活动运行实例。
1. 使用`aio app deploy`部署最新代码并记录已部署的操作名称（`[...]`之间的名称）。 这将用于更新步骤8中的`launch.json`。

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 使用命令`npx adobe-asset-compute devtool`启动Asset compute开发工具的新实例
1. 在VS代码中，点按左侧导航中的调试图标
   + 如果出现提示，请点按&#x200B;__创建launch.json文件> Node.js__&#x200B;以创建新的`launch.json`文件。
   + 否则，点按&#x200B;__启动程序__&#x200B;下拉列表右侧的&#x200B;__齿轮__&#x200B;图标，以在编辑器中打开现有的`launch.json`。
1. 将以下JSON对象配置添加到`configurations`数组：

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. 从下拉菜单中选择新的&#x200B;__wskdebug__
1. 点按&#x200B;__wskdebug__&#x200B;下拉列表左侧的绿色&#x200B;__运行__&#x200B;按钮
1. 打开`/actions/worker/index.js` ，然后点按行号左侧的以添加断点1。 导航到在步骤6中打开的Asset compute开发工具Web浏览器窗口
1. 点按&#x200B;__运行__&#x200B;按钮以执行工作程序
1. 导航回VS代码，导航到`/actions/worker/index.js`并逐步完成代码
1. 要退出可调试的开发工具，请点按步骤6中运行`npx adobe-asset-compute devtool`命令的终端中的`Ctrl-C`

## 从Adobe I/O Runtime访问日志{#aio-app-logs}

[AEM as aCloud Service通过在Adobe I/O Runtime中直接调用处理用户档](../deploy/processing-profiles.md) 案来利用Asset compute工作程序。由于这些调用不涉及本地开发，因此无法使用本地工具(如Asset compute开发工具或wskdebug)来调试其执行。 相反，可以使用Adobe I/OCLI从在Adobe I/O Runtime的特定工作区中执行的工作程序中获取日志。

1. 确保根据需要调试的工作区，通过`AIO_runtime_namespace`和`AIO_runtime_auth`设置[特定于工作区的环境变量](../deploy/runtime.md)。
1. 从命令行中，执行`aio app logs`
   + 如果工作区遇到大量流量，请通过`--limit`标记扩展激活日志的数量：
      `$ aio app logs --limit=25`
1. 将返回最新（最多为提供的`--limit`）激活日志作为命令的输出以供审阅。

   ![aio应用程序日志](./assets/debug/aio-app-logs.png)

## 疑难解答

+ [调试器未附加](../troubleshooting.md#debugger-does-not-attach)
+ [断点不会暂停](../troubleshooting.md#breakpoints-no-pausing)
+ [未附加VS代码调试器](../troubleshooting.md#vs-code-debugger-not-attached)
+ [工作程序开始执行后附加的VS代码调试器](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [调试时工作程序超时](../troubleshooting.md#worker-times-out-while-debugging)
+ [无法终止调试器进程](../troubleshooting.md#cannot-terminate-debugger-process)
