---
title: 调试Asset compute工作程序
description: asset compute工作程序可以通过多种方式进行调试，从简单的调试日志语句，到作为远程调试器的附加VS Code，再到在Adobe I/O Runtime中为从AEMas a Cloud Service启动的激活提取日志。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---

# 调试Asset compute工作程序

asset compute工作程序可以通过多种方式进行调试，从简单的调试日志语句，到作为远程调试器的附加VS Code，再到在Adobe I/O Runtime中为从AEMas a Cloud Service启动的激活提取日志。

## 日志记录

asset compute调试人员最基本的调试方式是使用传统的 `console.log(..)` 工作人员代码中的语句。 此 `console` JavaScript对象是一个隐式全局对象，因此无需导入或要求它，因为它始终存在于所有上下文中。

根据执行Asset compute工作进程的方式，可查看这些log语句：

+ 从 `aio app run`，将日志打印到标准输出和 [开发工具的](../develop/development-tool.md) 激活日志
  ![aio应用程序运行console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 从 `aio app test`，日志打印到 `/build/test-results/test-worker/test.log`
  ![aio应用程序测试控制台日志(...)](./assets/debug/console-log__aio-app-test.png)
+ 使用 `wskdebug`，日志语句打印到VS代码调试控制台（查看>调试控制台），标准输出
  ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 使用 `aio app logs`，日志语句打印到激活日志输出

## 通过附加的调试器进行远程调试

>[!WARNING]
>
>使用Microsoft Visual Studio Code 1.48.0或更高版本，以便与wskdebug兼容

此 [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模块支持将调试器附加到Asset compute工作程序，包括在VS代码中设置断点以及逐步执行代码的功能。

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_使用wskdebug调试Asset compute工作程序的点进（无音频）_

1. 确保 [wskdebug](../set-up/development-environment.md#wskdebug) 和 [ngrok](../set-up/development-environment.md#ngork) npm模块已安装
1. 确保 [Docker Desktop和支持的Docker映像](../set-up/development-environment.md#docker) 已安装并正在运行
1. 关闭开发工具的任何活动运行实例。
1. 使用部署最新的代码 `aio app deploy`  并记录已部署的操作名称(名称介于 `[...]`)。 这用于更新 `launch.json` 步骤8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 使用命令启动Asset compute开发工具的新实例 `npx adobe-asset-compute devtool`
1. 在VS Code中，点按左侧导航中的调试图标
   + 如果出现提示，请点按 __创建launch.json文件> Node.js__ 以新建 `launch.json` 文件。
   + 否则，点按 __齿轮__ 图标（位于页面右侧） __启动项目__ 下拉菜单以打开现有的 `launch.json` 在编辑器中。
1. 将以下JSON对象配置添加到 `configurations` 数组：

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

1. 选择新的 __wskdebug__ 从下拉菜单中
1. 点按绿色 __运行__ 按钮左侧 __wskdebug__ 下拉列表
1. 打开 `/actions/worker/index.js` 并点按行号左侧以添加断点1。 导航到在步骤6中打开的Asset compute开发工具Web浏览器窗口
1. 点按 __运行__ 按钮以执行工作进程
1. 导航回VS代码，到 `/actions/worker/index.js` 并逐步执行代码
1. 要退出可调试的开发工具，请点按 `Ctrl-C` 在运行的终端中 `npx adobe-asset-compute devtool` 步骤6中的命令

## 从Adobe I/O Runtime访问日志{#aio-app-logs}

[AEMas a Cloud Service通过处理用户档案来利用Asset compute工作人员](../deploy/processing-profiles.md) 直接在Adobe I/O Runtime中调用它们。 由于这些调用不涉及本地开发，因此无法使用本地工具(如Asset compute开发工具或wskdebug)调试其执行。 相反，Adobe I/OCLI可用于从Adobe I/O Runtime中特定工作区执行的worker中提取日志。

1. 确保 [工作区特定的环境变量](../deploy/runtime.md) 通过以下方式设置 `AIO_runtime_namespace` 和 `AIO_runtime_auth`，基于需要调试的工作区。
1. 从命令行中，执行 `aio app logs`
   + 如果工作区产生大量流量，请通过 `--limit` 标志：
     `$ aio app logs --limit=25`
1. 最近(截至提供的 `--limit`)激活日志作为命令输出返回，以供审查。

   ![aio应用程序日志](./assets/debug/aio-app-logs.png)

## 疑难解答

+ [调试器未附加](../troubleshooting.md#debugger-does-not-attach)
+ [断点未暂停](../troubleshooting.md#breakpoints-no-pausing)
+ [未附加VS代码调试器](../troubleshooting.md#vs-code-debugger-not-attached)
+ [辅助进程执行开始后附加的VS代码调试器](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Worker在调试时超时](../troubleshooting.md#worker-times-out-while-debugging)
+ [无法终止调试器进程](../troubleshooting.md#cannot-terminate-debugger-process)
