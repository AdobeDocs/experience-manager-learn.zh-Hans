---
title: 对AEM Assets的Asset compute可扩展性进行疑难解答
description: 以下是开发和部署AEM Assets的自定义Asset compute工作器时可能遇到的常见问题和错误以及解决办法的索引。
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 649d971ecaa67c0d1dd2636f3c212bfee3d13561
workflow-type: tm+mt
source-wordcount: '1241'
ht-degree: 0%

---


# asset compute可扩展性疑难解答

以下是开发和部署AEM Assets的自定义Asset compute工作器时可能遇到的常见问题和错误以及解决办法的索引。

## 开发{#develop}

### 再现部分被绘制／损坏{#rendition-returned-partially-drawn-or-corrupt}

+ __错误__:再现呈现不完整（当图像时）或损坏且无法打开。

   ![返回部分绘制的再现](./assets/troubleshooting/develop__await.png)

+ __原因__:在完全写入再 `renditionCallback` 现之前，工作者的函数正在退出 `rendition.path`。
+ __解决方案__:查看自定义工作器代码，并确保所有异步调用都使用同步 `await`。

## 开发工具{#development-tool}

### asset compute项目{#missing-console-json}中缺少Console.json文件

+ __错误：__ 错误：验证时缺少所需文件(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY)(异步设置AssetCompute(.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__ Asset compute `console.json` 项目的根中缺少该文件
+ __解决方案：__ 从您的Adobe I/O `console.json` 项目下载新表单
   1. 在console.adobe.io中，打开配置为使用的Adobe I/O项目Asset compute项目
   1. 点按右上方的&#x200B;__下载__&#x200B;按钮
   1. 使用文件名`console.json`将下载的文件保存到Asset compute项目的根目录

### manifest.yml{#incorrect-yaml-indentation}中的YAML缩进不正确

+ __错误：__ YAMLException:在X，列Y:（通过标准从命令中取消）行对映射项进行 `aio app run` 缩进错误
+ __原因：__ Yaml文件是白色间隔区分的，您的缩进可能不正确。
+ __分辨率：__ 检查 `manifest.yml` 并确保所有缩进正确。

### memorySize限制设置为太低{#memorysize-limit-is-set-too-low}

+ __错误：本__  地开发服务器OpenWhiskError:PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true返回HTTP 400（错误请求）—> &quot;请求内容格式不正确：要求失败：内存低于允许的阈值64 MB 134217728 B”
+ __原因：__ 中 `memorySize` 的工作器限制 `manifest.yml` 设置为低于错误消息报告的允许的最小阈值（以字节为单位）。
+ __解决方__  案： `memorySize` 查看限制 `manifest.yml` 并确保它们均大于允许的最小阈值。

### 由于缺少private.key{#missing-private-key}，开发工具无法开始

+ __错误：本__ 地开发服务器错误：验证私钥文件中缺少所需文件…….（通过从`aio app run`命令输出的标准输出）
+ __原因：__ 文 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 件 `.env` 中的值不指向 `private.key` 当前用 `private.key` 户或当前用户不可读。
+ __分辨率：__ 查看文 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 件中 `.env` 的值，并确保它包含文件系统上完整的 `private.key` 绝对路径。

### 源文件下拉列表不正确{#source-files-dropdown-incorrect}

asset compute开发工具可能会进入其提取旧数据的状态，在显示错误项的&#x200B;__源文件__&#x200B;下拉菜单中最为明显。

+ __错误：源文__ 件下拉框显示不正确的项目。
+ __原因：过__ 时的缓存浏览器状态导致
+ __解决方__ 法：在浏览器中，完全清除浏览器选项卡的“应用程序状态”、浏览器缓存、本地存储和服务工作者。

### 缺少或无效的devToolToken查询参数{#missing-or-invalid-devtooltoken-query-parameter}

+ __错误：__ Asset compute开发工具中的“未授权”通知
+ __原因：__ `devToolToken` 缺失或无效
+ __解决方__ 案：关闭Asset compute开发工具浏览器窗口，终止通过命令启动的任何正在运行的 `aio app run` 开发工具进程，并重新开始开发工具(使用 `aio app run`)。

### 无法删除源文件{#unable-to-remove-source-files}

+ __错误：__ 无法从开发工具UI中删除添加的源文件
+ __原因：__ 此功能尚未实现
+ __解决方__ 式：使用中定义的凭据登录云存储提供 `.env`商。找到开发工具使用的容器（也在`.env`中指定），导览至&#x200B;__source__&#x200B;文件夹，并删除任何源图像。 如果删除的源文件继续显示在下拉列表中，因为它们可能在本地缓存在开发工具“应用程序状态”中，则您可能需要执行[源文件下拉列表不正确](#source-files-dropdown-incorrect)中概述的步骤。

   ![Microsoft Azure Blob存储](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 测试{#test}

### 测试执行过程中未生成任何再现{#test-no-rendition-generated}

+ __错误：__ 失败：未生成任何再现。
+ __原因：__ 由于意外错误（如JavaScript语法错误），工作者无法生成再现。
+ __解决方__ 案：查看测试执行 `test.log` 位置 `/build/test-results/test-worker/test.log`。找到此文件中与失败测试用例对应的部分，并查看错误。

   ![疑难解答——无生成再现](./assets/troubleshooting/test__no-rendition-generated.png)

### 测试生成错误的再现，导致测试失败{#tests-generates-incorrect-rendition}

+ __错误：__ 失败：再现“rendition.xxx”不如预期。
+ __原因：__ 工作器输出的再现与测试用例中 `rendition.<extension>` 提供的再现不同。
   + 如果预期的`rendition.<extension>`文件创建方式与测试用例中本地生成的再现方式不完全相同，则测试可能会失败，因为位中可能存在一些差异。 例如，如果Asset compute工作者使用API更改对比度，并通过调整Adobe Photoshop CC的对比度来创建预期结果，则文件可能显示相同，但位的细微变化可能不同。
+ __分辨率：__ 通过导航到来检查从测试中输出的再现， `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`并将它与测试用例中的预期再现文件进行比较。要创建准确的预期资产，请执行以下操作之一：
   + 使用开发工具生成再现，验证其正确性，并将其用作预期的再现文件
   + 或者，验证在`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`生成的测试文件，验证其正确性，并将其用作预期的再现文件

## 调试

### 调试器未连接{#debugger-does-not-attach}

+ __错误__:处理启动时出错：错误：无法连接到调试目标...
+ __原因__:Docker Desktop未在本地系统上运行。通过查看VS代码调试控制台(视图>调试控制台)验证此错误，确认报告了此错误。
+ __解决方案__:开始 [Docker Desktop并确认安装了必需的Docker图像](./set-up/development-environment.md#docker)。

### 断点不会暂停{#breakpoints-no-pausing}

+ __错误__:当从可调试的开发工具运行Asset compute工作器时，VS代码不会在断点暂停。

#### VS代码调试器未连接{#vs-code-debugger-not-attached}

+ __原因：__ VS代码调试器已停止／断开连接。
+ __解决方__ 案：重新启动VS代码调试器，并通过观看VS代码调试输出控制台(“视图”>“调试控制台”)验证它是否连接

#### VS在工作器执行开始后附加的代码调试器{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ 在点击Runin Development Tool之前，VS代码调试 ____ 器未连接。
+ __解决方__ 案：通过查看VS代码的调试控制台(视图>调试控制台)，确保已附加调试器，然后从开发工具重新运行Asset compute工作器。

### 调试{#worker-times-out-while-debugging}时工作器超时

+ __错误__:调试控制台报告“操作将以-XXX毫秒为单位超时”或 [Asset compute开发工具的](./develop/development-tool.md) 再现预览无限旋转或
+ __原因__:调试期间超出清单。 [ymlis中](./develop/manifest.md) 定义的工作超时。
+ __解决方案__:临时增加清单。ymlor中工作者的超 [时时间](./develop/manifest.md) 可加快调试活动。

### 无法终止调试器进程{#cannot-terminate-debugger-process}

+ __错误__: `Ctrl-C` 命令行不终止调试器进程(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3. `@adobe/aio-cli-plugin-asset-compute` x中出现错误，导致 `Ctrl-C` 无法识别为终止命令。
+ __解决方案__:更 `@adobe/aio-cli-plugin-asset-compute` 新到版本1.4.1+

   ```
   $ aio update
   ```

   ![疑难解答-aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### AEM{#custom-rendition-missing-from-asset}中的资产中缺少自定义再现

+ __错误：__ 新资产和重新处理的资产处理成功，但缺少自定义演绎版

#### 处理用户档案未应用于上级文件夹

+ __原因：__ 资产在具有使用自定义工作器的处理用户档案的文件夹下不存在
+ __解决方__ 式：将处理用户档案应用到资产的上级文件夹

#### 处理用户档案由较低的处理用户档案替代

+ __原因：__ 资产存在于应用了自定义工作者处理用户档案的文件夹下，但在该文件夹和资产之间已应用了不使用客户工作者的其他处理用户档案。
+ __解决方__ 式：合并或以其他方式协调两个处理用户档案并删除中间处理用户档案

### AEM{#asset-processing-fails}中的资产处理失败

+ __错误：资产__ 处理失败标记显示在资产上
+ __原因：__ 执行自定义工作器时出错
+ __分辨率：按__ 照使用调试Adobe I/O Runtime [活动](./test-debug/debug.md#aio-app-logs) 的说明操作 `aio app logs`。


