---
title: 测试Asset compute工作程序
description: asset compute项目定义了一种模式，用于轻松创建和执行Asset compute工作程序的测试。
feature: asset compute微服务
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: 集成、开发
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 0%

---


# 测试Asset compute工作程序

asset compute项目定义了一种模式，用于轻松创建和执行Asset compute工作程序](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html)的测试。[

## 工人考试剖析

asset compute工作者的测试被划分为多个测试包，并且在每个测试包中，有一个或多个测试案例声明要测试的条件。

asset compute项目中的测试结构如下：

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

每个测试转换都可以具有以下文件：

+ `file.<extension>`
   + 要测试的源文件（扩展名可以是`.link`以外的任何内容）
   + 必填
+ `rendition.<extension>`
   + 预期演绎版
   + 必需，错误测试除外
+ `params.json`
   + 单个呈现版本JSON说明
   + 可选
+ `validate`
   + 将预期和实际的再现文件路径作为参数且在结果确定时必须返回退出代码0的脚本，或在验证或比较失败时返回非零退出代码的脚本。
   + 可选，默认为`diff`命令
   + 使用外壳脚本来封装Docker run命令以使用不同的验证工具
+ `mock-<host-name>.json`
   + [模拟外部服务调用](https://www.mock-server.com/mock_server/creating_expectations.html)的JSON格式HTTP响应。
   + 可选，仅当工作代码自行发出HTTP请求时才使用

## 编写测试用例

此测试案例声明输入文件(`file.jpg`)的参数化输入(`params.json`)生成预期的PNG呈现(`rendition.png`)。

1. 首先删除在`/test/asset-compute/simple-worker`自动生成的`simple-worker`测试案例，因为该测试案例无效，因为我们的工作人员不再只是将源复制到演绎版。
1. 在`/test/asset-compute/worker/success-parameterized`处创建新的测试用例文件夹，以测试生成PNG呈现版本的工作程序是否成功执行。
1. 在`success-parameterized`文件夹中，为此测试案例添加测试[输入文件](./assets/test/success-parameterized/file.jpg)，并将其命名为`file.jpg`。
1. 在`success-parameterized`文件夹中，添加一个名为`params.json`的新文件，该文件定义工作程序的输入参数：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   这些键/值与传递到[开发工具的Asset compute配置文件定义](../develop/development-tool.md)的键/值相同，减去`worker`键。

1. 将预期的[呈现文件](./assets/test/success-parameterized/rendition.png)添加到此测试案例中，并将其命名为`rendition.png`。 此文件表示工作程序对给定输入`file.jpg`的预期输出。
1. 在命令行中，通过执行`aio app test`来运行项目根
   + 确保安装并启动[Docker Desktop](../set-up/development-environment.md#docker)和支持Docker映像
   + 终止任何正在运行的开发工具实例

![测试 — 成功  ](./assets/test/success-parameterized/result.png)

## 编写错误检查测试用例

此测试案例旨在确保当`contrast`参数设置为无效值时，工作程序会引发相应错误。

1. 在`/test/asset-compute/worker/error-contrast`处创建新的测试用例文件夹，以测试因`contrast`参数值无效而导致的工作器重新执行。
1. 在`error-contrast`文件夹中，为此测试案例添加测试[输入文件](./assets/test/error-contrast/file.jpg)，并将其命名为`file.jpg`。 此文件的内容对此测试不重要，它只需存在即可通过“损坏的源”检查，才能达到`rendition.instructions`有效性检查，此测试案例将验证。
1. 在`error-contrast`文件夹中，添加一个名为`params.json`的新文件，该文件定义工作程序的输入参数，其内容如下：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 将`contrast`参数设置为`10`，这是一个无效值，因为对比度必须介于–1和1之间，才能引发`RenditionInstructionsError`。
   + 通过将`errorReason`键设置为与预期错误关联的“reason”，断言测试中会引发相应错误。 此无效的对比度参数会引发[自定义错误](../develop/worker.md#errors)、`RenditionInstructionsError`，因此会将`errorReason`设置为此错误的原因，或将`rendition_instructions_error`设置为声明引发该错误。

1. 由于在执行错误期间不应生成任何呈现版本，因此不需要`rendition.<extension>`文件。
1. 通过执行命令`aio app test`从项目的根运行测试包
   + 确保安装并启动[Docker Desktop](../set-up/development-environment.md#docker)和支持Docker映像
   + 终止任何正在运行的开发工具实例

![测试 — 错误对比度](./assets/test/error-contrast/result.png)

## Github上的测试用例

Github上提供了最终的测试案例，具体网址为：

+ [aem-guides-wknd-asset-compute/test/asset-compute-worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑难解答

+ [测试执行期间未生成任何呈现版本](../troubleshooting.md#test-no-rendition-generated)
+ [测试生成不正确的呈现版本](../troubleshooting.md#tests-generates-incorrect-rendition)
