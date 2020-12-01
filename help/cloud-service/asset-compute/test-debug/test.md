---
title: 测试Asset compute工作者
description: asset compute项目定义了一种模式，用于轻松创建和执行Asset compute工作者的测试。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 0%

---


# 测试Asset compute工作者

asset compute项目定义一种模式，用于轻松创建和执行Asset compute工作者的[测试。](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html)

## 工人测试剖析

asset compute工作者的测试被分为测试套件，在每个测试套件中，有一个或多个测试用例声明要测试的条件。

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

每个测试转播都可以包含以下文件：

+ `file.<extension>`
   + 要测试的源文件（扩展名可以是`.link`以外的任何内容）
   + 必填
+ `rendition.<extension>`
   + 预期再现
   + 必需，错误测试除外
+ `params.json`
   + 单个再现JSON说明
   + 可选
+ `validate`
   + 将预期和实际再现文件路径作为参数并且如果结果正确，必须返回退出代码0的脚本；如果验证或比较失败，则返回非零退出代码。
   + 可选，默认为`diff`命令
   + 使用外壳脚本，它为使用不同的验证工具包裹Docker运行命令
+ `mock-<host-name>.json`
   + 模拟外部服务调用[的JSON格式化HTTP响应。](https://www.mock-server.com/mock_server/creating_expectations.html)
   + 可选，仅在工作代码发出其自己的HTTP请求时使用

## 编写测试用例

此测试用例声明输入文件(`file.jpg`)的参数化输入(`params.json`)生成预期的PNG再现(`rendition.png`)。

1. 首先删除自动生成的`simple-worker`测试案例，因为这无效，因为我们的工作人员不再简单地将源复制到再现。`/test/asset-compute/simple-worker`
1. 在`/test/asset-compute/worker/success-parameterized`处新建一个测试用例文件夹，以测试生成PNG再现的工作器是否成功执行。
1. 在`success-parameterized`文件夹中，添加此测试用例的测试[输入文件](./assets/test/success-parameterized/file.jpg)并将其命名为`file.jpg`。
1. 在`success-parameterized`文件夹中，添加一个名为`params.json`的新文件，该文件定义工作器的输入参数：

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   这些键／值与传入[开发工具的Asset compute用户档案定义](../develop/development-tool.md)的键／值相同，小于`worker`键。
1. 将预期的[再现文件](./assets/test/success-parameterized/rendition.png)添加到此测试用例，并将其命名为`rendition.png`。 此文件表示给定输入`file.jpg`的工作器的预期输出。
1. 在命令行中，通过执行`aio app test`来运行项目根
   + 确保安装并启动[Docker Desktop](../set-up/development-environment.md#docker)和支持Docker映像
   + 终止任何正在运行的开发工具实例

![测试——成功  ](./assets/test/success-parameterized/result.png)

## 编写错误检查测试用例

此测试用例测试以确保当`contrast`参数设置为无效值时，该工作器抛出相应错误。

1. 在`/test/asset-compute/worker/error-contrast`新建一个测试用例文件夹，以测试由于无效的`contrast`参数值导致的该工作器的重新执行。
1. 在`error-contrast`文件夹中，添加此测试用例的测试[输入文件](./assets/test/error-contrast/file.jpg)并将其命名为`file.jpg`。 此文件的内容对此测试并不重要，它只需要存在才能通过“损坏源”检查，才能达到`rendition.instructions`有效性检查，此测试用例是有效的。
1. 在`error-contrast`文件夹中，添加一个名为`params.json`的新文件，该文件定义了具有以下内容的工作器的输入参数：

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + 将`contrast`参数设置为`10`，这是一个无效值，因为对比度必须介于-1和1之间，以抛出`RenditionInstructionsError`。
   + 通过将`errorReason`键设置为与预期错误关联的“reason”，在测试中引发相应错误。 此无效的对比度参数将引发[自定义错误](../develop/worker.md#errors)、`RenditionInstructionsError`，因此将`errorReason`设置为此错误的原因，或将`rendition_instructions_error`设置为声明引发该错误。

1. 由于在执行重新循环期间不应生成任何再现，因此不需要`rendition.<extension>`文件。
1. 通过执行命令`aio app test`从项目的根运行测试套件
   + 确保安装并启动[Docker Desktop](../set-up/development-environment.md#docker)和支持Docker映像
   + 终止任何正在运行的开发工具实例

![测试——错误对比度](./assets/test/error-contrast/result.png)

## Github的测试用例

Github上提供的最终测试用例有：

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## 疑难解答

+ [测试执行过程中未生成任何再现](../troubleshooting.md#test-no-rendition-generated)
+ [测试生成不正确的再现](../troubleshooting.md#tests-generates-incorrect-rendition)
