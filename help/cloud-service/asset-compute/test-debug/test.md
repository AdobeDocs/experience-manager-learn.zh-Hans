---
title: 测试资产计算工作者
description: 资产计算项目定义一种模式，用于轻松创建和执行资产计算工作程序的测试。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---


# 测试资产计算工作者

资产计算项目定义一种模式，用于轻松创建和执 [行资产计算工作者的测试](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)。

## 工人测试剖析

资产计算工作者的测试被分为测试套件，每个测试套件中都有一个或多个测试用例声明要测试的条件。

资产计算项目中的测试结构如下：

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

每个测试转播都可以包含以下文件：

+ `file.<extension>`
   + 要测试的源文件(扩展名可以是除外的任何 `.link`内容
   + 必填
+ `rendition.<extension>`
   + 预期再现
   + 必需，错误测试除外
+ `params.json`
   + 单个再现JSON说明
   + 可选
+ `validate`
   + 将预期和实际再现文件路径作为参数并且如果结果正确，必须返回退出代码0的脚本；如果验证或比较失败，则返回非零退出代码。
   + 可选，默认为命 `diff` 令
   + 使用外壳脚本，它为使用不同的验证工具包裹Docker运行命令
+ `mock-<host-name>.json`
   + JSON格式化的HTTP响应， [用于模仿外部服务调用](https://www.mock-server.com/mock_server/creating_expectations.html)。
   + 可选，仅在工作代码发出其自己的HTTP请求时使用

## 编写测试用例

此测试用例声明输入文件()`params.json`的参数化输入()`file.jpg`生成预期的PNG再现(`rendition.png`)。

1. 首先删除自动生成的 `simple-worker` 测试案例， `/test/asset-compute/simple-worker` 因为该测试案例无效，因为我们的工作人员不再简单地将源复制到再现。
1. 在上新建一个测试用例文 `/test/asset-compute/worker/success-parameterized` 件夹，以测试生成PNG再现的工作器是否成功执行。
1. 在文 `success-parameterized` 件夹中，添加此 [测试用例](./assets/test/success-parameterized/file.jpg) 的测试输入文件并命名它 `file.jpg`。
1. 在该文 `success-parameterized` 件夹中，添加一个名 `params.json` 为的新文件，它定义工作者的输入参数：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   这些键／值与传入开发工具的 [资产计算用户档案定义的键](../develop/development-tool.md)/值相同 `worker` ，减去键。
1. 将所需的再 [现文件](./assets/test/success-parameterized/rendition.png) 添加到此测试用例并命名它 `rendition.png`。 此文件表示工作器对给定输入的预期输出 `file.jpg`。
1. 在命令行中，通过执行 `aio app test`
   + 确保 [安装和启](../set-up/development-environment.md#docker) 动Docker Desktop和支持Docker映像
   + 终止任何正在运行的开发工具实例

![测试——成功 ](./assets/test/success-parameterized/result.png)

## 编写错误检查测试用例

此测试用例测试以确保当参数设置为无效值时 `contrast` 工作器抛出相应错误。

1. 在上新建一个测试用例文 `/test/asset-compute/worker/error-contrast` 件夹，以测试由于参数值无效而导致的该工作 `contrast` 器的重新执行。
1. 在文 `error-contrast` 件夹中，添加此 [测试用例](./assets/test/error-contrast/file.jpg) 的测试输入文件并命名它 `file.jpg`。 此文件的内容对本测试并不重要，它只需要存在，才能通过“损坏源”检查，才能达到有效性检查，此测试用例 `rendition.instructions` 是有效的。
1. 在该文 `error-contrast` 件夹中，添加一个名 `params.json` 为的新文件，该新文件定义工作者的输入参数及内容：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 将参 `contrast` 数设 `10`置为无效值，因为对比度必须介于-1和1之间，以抛出 `RenditionInstructionsError`。
   + 通过将键设置为与预期错误关联的“ `errorReason` 原因”，在测试中引发相应错误。 此无效的对比度参 [数将引发自定义错](../develop/worker.md#errors)误 `RenditionInstructionsError`，因此将 `errorReason` 该错误的原因设置为，或者`rendition_instructions_error` ，将引发该错误。

1. 由于在执行重新循环期间不应生成任何再现，因此 `rendition.<extension>` 无需任何文件。
1. 通过执行命令从项目的根运行测试套件 `aio app test`
   + 确保 [安装和启](../set-up/development-environment.md#docker) 动Docker Desktop和支持Docker映像
   + 终止任何正在运行的开发工具实例

![测试——错误对比度](./assets/test/error-contrast/result.png)

## Github的测试用例

Github上提供的最终测试用例有：

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑难解答

### 未生成任何再现

测试用例在不生成再现的情况下失败。

+ __错误：__ 失败：未生成任何再现。
+ __原因：__ 由于意外错误（如JavaScript语法错误），该工作器无法生成再现。
+ __解决方案：__ 查看测试执行 `test.log` 的 `/build/test-results/test-worker/test.log`。 找到此文件中与失败测试用例对应的部分，并查看错误。

   ![疑难解答——无生成再现](./assets/test/troubleshooting__no-rendition-generated.png)

### 测试生成不正确的再现

测试用例无法生成不正确的再现。

+ __错误：__ 失败：再现“rendition.xxx”不如预期。
+ __原因：__ 该工作器输出的再现与测试用例 `rendition.<extension>` 中提供的再现不同。
   + 如果预期文 `rendition.<extension>` 件的创建方式与测试用例中本地生成的再现的创建方式不完全相同，则测试可能会失败，因为位可能有一些不同。 如果测试用例中的预期再现是从开发工具中保存的(即在Adobe I/O Runtime内生成)，则位在技术上可能不同，导致测试失败，即使从人的角度看，预期和实际再现文件是相同的。
+ __解决方案：__ 导航到，检查测试中的再现输 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`出，并将其与测试用例中的预期再现文件进行比较。
