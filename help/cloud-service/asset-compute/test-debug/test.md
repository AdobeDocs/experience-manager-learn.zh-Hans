---
title: 测试Asset Compute Worker
description: Asset Compute项目定义了一种模式，用于轻松创建并执行Asset Compute Worker测试。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 142
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 测试Asset Compute Worker

Asset Compute项目定义了一种模式，用于轻松创建和执行Asset Compute工作程序[测试](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html?lang=zh-Hans)。

## 工人考试剖析

Asset Compute Worker的测试分为多个测试包，并且在每个测试包中，一个或多个测试用例声明要测试的条件。

Asset Compute项目中的测试结构如下所示：

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
   + 要测试的Source文件（扩展名可以是`.link`以外的任何内容）
   + 必填
+ `rendition.<extension>`
   + 预期呈现版本
   + 必需，错误测试除外
+ `params.json`
   + 单个演绎版JSON指令
   + 可选
+ `validate`
   + 将预期和实际演绎版文件路径作为参数获取，如果结果正常，则必须返回退出代码0；如果验证或比较失败，则必须返回非零退出代码。
   + 可选，默认为`diff`命令
   + 使用封装了Docker run命令的外壳脚本，以使用不同的验证工具
+ `mock-<host-name>.json`
   + [模拟外部服务调用](https://www.mock-server.com/mock_server/creating_expectations.html)的JSON格式化HTTP响应。
   + 可选，仅当辅助代码发出自己的HTTP请求时使用

## 编写测试用例

此测试用例声明输入文件(`file.jpg`)的参数化输入(`params.json`)生成预期的PNG格式副本(`rendition.png`)。

1. 首先，删除位于`/test/asset-compute/simple-worker`的自动生成的`simple-worker`测试用例，因为此用例无效，因为我们的辅助进程不再只是将源复制到演绎版。
1. 在`/test/asset-compute/worker/success-parameterized`处新建测试用例文件夹，以测试生成PNG演绎版的工作程序的成功执行。
1. 在`success-parameterized`文件夹中，为此测试用例添加测试[输入文件](./assets/test/success-parameterized/file.jpg)并将其命名为`file.jpg`。
1. 在`success-parameterized`文件夹中，添加名为`params.json`的新文件以定义辅助进程的输入参数：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   这些是传递到[开发工具的Asset Compute配置文件定义](../develop/development-tool.md)中的相同键/值，小于`worker`键。

1. 将预期的[演绎版文件](./assets/test/success-parameterized/rendition.png)添加到此测试用例中，并将其命名为`rendition.png`。 此文件表示给定输入`file.jpg`的辅助进程的预期输出。
1. 从命令行中，通过执行`aio app test`来运行项目根目录测试
   + 确保已安装并启动[Docker Desktop](../set-up/development-environment.md#docker)和支持的Docker映像
   + 终止任何正在运行的开发工具实例

![测试 — 成功](./assets/test/success-parameterized/result.png)

## 编写错误检查测试用例

此测试用例测试以确保Worker在`contrast`参数设置为无效值时引发相应的错误。

1. 在`/test/asset-compute/worker/error-contrast`处新建测试用例文件夹，以测试由于`contrast`参数值无效而导致工作进程执行的错误。
1. 在`error-contrast`文件夹中，为此测试用例添加测试[输入文件](./assets/test/error-contrast/file.jpg)并将其命名为`file.jpg`。 此文件的内容对此测试并不重要，它只是需要存在才能通过“源损坏”检查，为了达到此测试用例验证的`rendition.instructions`有效性检查。
1. 在`error-contrast`文件夹中，添加名为`params.json`的新文件以定义辅助进程的输入参数及其内容：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 将`contrast`参数设置为`10`（无效值，因为对比度必须介于–1和1之间）以引发`RenditionInstructionsError`。
   + 通过将`errorReason`键设置为与预期错误关联的“原因”，声明在测试中抛出相应的错误。 此无效的对比度参数引发[自定义错误](../develop/worker.md#errors) `RenditionInstructionsError`，因此将`errorReason`设置为此错误的原因，或将`rendition_instructions_error`设置为声明引发该错误。

1. 由于在错误执行期间不应生成任何演绎版，因此不需要`rendition.<extension>`文件。
1. 通过执行命令`aio app test`，从项目的根目录运行测试包
   + 确保已安装并启动[Docker Desktop](../set-up/development-environment.md#docker)和支持的Docker映像
   + 终止任何正在运行的开发工具实例

![测试 — 错误对比度](./assets/test/error-contrast/result.png)

## Github上的测试案例

Github上提供了最终测试案例，网址为：

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑难解答

+ [测试执行期间未生成演绎版](../troubleshooting.md#test-no-rendition-generated)
+ [测试生成错误的演绎版](../troubleshooting.md#tests-generates-incorrect-rendition)
