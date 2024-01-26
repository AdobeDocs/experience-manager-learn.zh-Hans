---
title: 测试Asset compute工作程序
description: asset compute项目定义了一种模式，用于轻松创建并执行Asset compute工作程序的测试。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 182
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 测试Asset compute工作程序

asset compute项目定义了一种模式，用于轻松创建和执行 [asset compute工作人员的测试](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html).

## 工人考试剖析

asset compute工作人员的测试分为多个测试包，并且在每个测试包中，一个或多个测试用例声明要测试的条件。

asset compute项目中的测试结构如下所示：

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

每个测试转换可以包含以下文件：

+ `file.<extension>`
   + 要测试的源文件(扩展名可以是除 `.link`)
   + 必填
+ `rendition.<extension>`
   + 预期呈现版本
   + 必需，错误测试除外
+ `params.json`
   + 单个演绎版JSON指令
   + 可选
+ `validate`
   + 将预期和实际演绎版文件路径作为参数获取，如果结果正常，则必须返回退出代码0；如果验证或比较失败，则必须返回非零退出代码。
   + 可选，默认为 `diff` 命令
   + 使用封装了Docker run命令的外壳脚本，以使用不同的验证工具
+ `mock-<host-name>.json`
   + 针对以下内容的JSON格式化HTTP响应 [嘲弄外部服务调用](https://www.mock-server.com/mock_server/creating_expectations.html).
   + 可选，仅当辅助代码发出自己的HTTP请求时使用

## 编写测试用例

此测试用例声明参数化输入(`params.json`)作为输入文件(`file.jpg`)生成预期的PNG演绎版(`rendition.png`)。

1. 首先删除自动生成的 `simple-worker` 测试案例位于 `/test/asset-compute/simple-worker` 由于这是无效的，因为我们的工作人员不再只是将源复制到演绎版。
1. 在新建测试用例文件夹 `/test/asset-compute/worker/success-parameterized` 测试生成PNG演绎版的工作程序的成功执行。
1. 在 `success-parameterized` 文件夹，添加测试 [输入文件](./assets/test/success-parameterized/file.jpg) 用于此测试用例并命名它 `file.jpg`.
1. 在 `success-parameterized` 文件夹，添加名为的新文件 `params.json` 用于定义工作程序的输入参数：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   这些是传递到 [开发工具的Asset compute配置文件定义](../develop/development-tool.md)，减去 `worker` 键。

1. 添加预期的 [节目文件](./assets/test/success-parameterized/rendition.png) 到此测试用例并将其命名为 `rendition.png`. 此文件表示给定输入的工作程序的预期输出 `file.jpg`.
1. 从命令行中，通过执行 `aio app test`
   + 确保 [Docker桌面](../set-up/development-environment.md#docker) 已安装并启动支持Docker的映像
   + 终止任何正在运行的开发工具实例

![测试 — 成功 ](./assets/test/success-parameterized/result.png)

## 编写错误检查测试用例

此测试用例测试旨在确保Worker在以下情况下抛出适当的错误： `contrast` 参数设置为无效值。

1. 在新建测试用例文件夹 `/test/asset-compute/worker/error-contrast` 由于无效而测试工作进程执行的错误 `contrast` 参数值。
1. 在 `error-contrast` 文件夹，添加测试 [输入文件](./assets/test/error-contrast/file.jpg) 用于此测试用例并命名它 `file.jpg`. 此文件的内容对此测试并不重要，它只需存在以通过“损坏源”检查，以达到 `rendition.instructions` 有效性检查，此测试用例是否有效。
1. 在 `error-contrast` 文件夹，添加名为的新文件 `params.json` 定义工作程序的输入参数及其内容：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 设置 `contrast` 参数到 `10`，一个无效值，因为对比度必须介于–1和1之间，才能引发 `RenditionInstructionsError`.
   + 通过设置 `errorReason` 键入与预期错误相关的“原因”的键。 此无效的对比度参数会引发 [自定义错误](../develop/worker.md#errors)， `RenditionInstructionsError`，因此请设置 `errorReason` 此错误的原因，或`rendition_instructions_error` 以证明它已被抛弃。

1. 由于在错误执行期间不应生成演绎版，因此 `rendition.<extension>` 文件是必需的。
1. 通过执行命令，从项目的根目录运行测试包 `aio app test`
   + 确保 [Docker桌面](../set-up/development-environment.md#docker) 已安装并启动支持Docker的映像
   + 终止任何正在运行的开发工具实例

![测试 — 错误对比度](./assets/test/error-contrast/result.png)

## Github上的测试案例

Github上提供了最终测试案例，网址为：

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑难解答

+ [测试执行期间未生成演绎版](../troubleshooting.md#test-no-rendition-generated)
+ [测试生成错误的演绎版](../troubleshooting.md#tests-generates-incorrect-rendition)
