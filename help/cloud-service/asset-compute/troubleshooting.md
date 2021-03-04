---
title: 解决AEM Assets的Asset compute可扩展性问题
description: 以下是开发和部署AEM Assets的自定义Asset computeWorker时可能遇到的常见问题和错误以及解决方法的索引。
feature: asset compute Microservices
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 集成、开发
role: 开发人员
level: 中级，经验丰富的
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1249'
ht-degree: 0%

---


# 解决Asset compute可扩展性问题

以下是开发和部署AEM Assets的自定义Asset computeWorker时可能遇到的常见问题和错误以及解决方法的索引。

## 开发{#develop}

### 返回部分绘制/损坏的演绎版{#rendition-returned-partially-drawn-or-corrupt}

+ __错误__:演绎版呈现不完整（当图像时）或已损坏，无法打开。

   ![返回部分绘制的再现](./assets/troubleshooting/develop__await.png)

+ __原因__:在完全写入再 `renditionCallback` 现之前，工作者的函数正在退出 `rendition.path`。
+ __解决方案__:查看自定义worker代码并确保所有异步调用都使用同步 `await`。

## 开发工具{#development-tool}

### asset compute项目{#missing-console-json}中缺少Console.json文件

+ __错误：__ 错误：验证时缺少所需文件(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY)(异步设置AssetCompute(.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__ 文 `console.json` 件在Asset compute项目的根中缺失
+ __解决方案：__ 从您的Adobe I/O项 `console.json` 目下载新表单
   1. 在console.adobe.io中，打开配置为使用的Adobe I/O项目
   1. 点按右上角的&#x200B;__下载__&#x200B;按钮
   1. 使用文件名`console.json`将下载的文件保存到Asset compute项目的根目录

### manifest.yml{#incorrect-yaml-indentation}中的YAML缩进不正确

+ __错误：__ YAMLException:在行X，列Y：（通过从命令中标准输出）处对映射条目进行 `aio app run` 的缩进
+ __原因：__ Yaml文件是白色间隔的敏感文件，您的缩进可能不正确。
+ __分辨率：__ 查看 `manifest.yml` 并确保所有缩进正确。

### memorySize限制设置得太低{#memorysize-limit-is-set-too-low}

+ __错误：本__  地开发服务器OpenWhiskError:PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true返回HTTP 400（错误请求） — > &quot;请求内容格式不正确：要求失败：内存低于允许的134217728 B阈值64 MB
+ __原因：__ 中 `memorySize` 的worker限制设置 `manifest.yml` 为低于错误消息报告的允许的最小阈值（以字节为单位）。
+ __解决方__  案：查 `memorySize` 看中的 `manifest.yml` 限制，并确保这些限制都大于允许的最小阈值。

### 由于缺少private.key{#missing-private-key}，开发工具无法开始

+ __错误：本__ 地开发服务器错误：在validatePrivateKeyFile中缺少所需文件…….（通过`aio app run`命令的标准输出）
+ __原因：__ 文 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 件 `.env` 中的值不指向 `private.key` 当 `private.key` 前用户或当前用户不可读。
+ __分辨率：__ 查看文 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 件 `.env` 中的值，并确保它包含指向文件系统 `private.key` 的完整绝对路径。

### 源文件下拉列表不正确{#source-files-dropdown-incorrect}

asset compute开发工具可能会进入其提取陈旧数据的状态，在显示错误项的&#x200B;__源文件__&#x200B;下拉列表中最为明显。

+ __错误：源__ 文件下拉列表显示不正确的项目。
+ __原因：缓__ 存的浏览器状态过时导致
+ __解决方__ 案：在浏览器中，完全清除浏览器选项卡的“应用程序状态”、浏览器缓存、本地存储和服务工作者。

### 缺少或无效devToolToken查询参数{#missing-or-invalid-devtooltoken-query-parameter}

+ __错误：__ Asset compute Development Tool中的“未授权”通知
+ __原因：__ `devToolToken` 缺失或无效
+ __解决方__ 案：关闭Asset compute开发工具浏览器窗口，终止通过命令启动的任何正在运行的开发工 `aio app run` 具进程，并重新开始开发工具(使用 `aio app run`)。

### 无法删除源文件{#unable-to-remove-source-files}

+ __错误：__ 无法从开发工具UI中删除添加的源文件
+ __原因：__ 尚未实现此功能
+ __解决方__ 式：使用中定义的凭据登录到云存储提供 `.env`程序。找到开发工具使用的容器（也在`.env`中指定），导览至&#x200B;__source__&#x200B;文件夹，并删除任何源图像。 如果已删除的源文件继续显示在下拉列表中，因为它们可能在开发工具“应用程序状态”中本地缓存，则您可能需要执行[源文件下拉列表中概述的步骤不正确](#source-files-dropdown-incorrect)。

   ![Microsoft Azure Blob存储](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 测试{#test}

### 在测试执行过程中未生成任何再现{#test-no-rendition-generated}

+ __错误：__ 失败：未生成任何再现。
+ __原因：__ 由于意外错误（如JavaScript语法错误），worker无法生成再现。
+ __解决方__ 案：查看测试执行 `test.log` 位置 `/build/test-results/test-worker/test.log`。找到此文件中与失败测试用例相对应的部分，并查看错误。

   ![疑难解答 — 未生成再现](./assets/troubleshooting/test__no-rendition-generated.png)

### 测试生成不正确的再现，导致测试失败{#tests-generates-incorrect-rendition}

+ __错误：__ 失败：再现“rendition.xxx”不如预期。
+ __原因：__ 工作器输出与测试用例中提供的 `rendition.<extension>` 格式副本不同的格式副本。
   + 如果创建的预期`rendition.<extension>`文件与测试用例中本地生成的再现的方式不完全相同，则测试可能会失败，因为位中可能存在一些差异。 例如，如果Asset compute工作者使用API更改对比度，并通过调整Adobe Photoshop CC中的对比度来创建预期结果，则文件可能显示相同，但位中的细微变化可能不同。
+ __分辨率：__ 通过导航到查看测试中输出的再现， `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`并将其与测试用例中的预期再现文件进行比较。要创建准确的预期资产，请执行以下任一操作：
   + 使用开发工具生成再现，验证其正确性，并将其用作预期的再现文件
   + 或者，在`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`验证测试生成的文件，验证其正确性，然后将该文件用作预期的再现文件

## 调试

### 调试器未连接{#debugger-does-not-attach}

+ __错误__:处理启动项时出错：错误：无法连接到调试目标...
+ __原因__:Docker Desktop未在本地系统上运行。通过查看VS代码调试控制台(“视图”>“调试控制台”)来验证此错误，确认报告此错误。
+ __解决方案__:开始  [Docker Desktop并确认安装了必需的Docker图像](./set-up/development-environment.md#docker)。

### 断点不会暂停{#breakpoints-no-pausing}

+ __错误__:当从可调试的开发工具运行Asset computeWorker时，VS代码不会在断点处暂停。

#### VS代码调试器未连接{#vs-code-debugger-not-attached}

+ __原因：__ VS代码调试器已停止/断开连接。
+ __解决方__ 案：重新启动VS代码调试器，并通过观看VS代码调试输出控制台(“视图”>“调试控制台”)验证它是否连接

#### VS代码调试器在worker执行开始{#vs-code-debugger-attached-after-worker-execution-began}后附加

+ __原因：__ 在点击Runin Development Tool之前，VS代码调试器 ____ 未连接。
+ __解决方__ 案：通过查看VS代码的调试控制台(“视图”>“调试控制台”)，确保已附加调试器，然后从开发工具重新运行Asset compute工作器。

### 调试{#worker-times-out-while-debugging}时Worker超时

+ __错误__:Debug Console报告“操作将以 — XXX毫秒为单位超时”，或者 [Asset compute Development Tool的](./develop/development-tool.md) 再现预览无限旋转或
+ __原因__:调试期间超出了manifest. [ymlis中](./develop/manifest.md) 定义的工作超时。
+ __解决方案__:临时增加manifest.ymlor中工作者的超时 [时间，](./develop/manifest.md) 加快调试活动。

### 无法终止调试器进程{#cannot-terminate-debugger-process}

+ __错误__: `Ctrl-C` 命令行上不终止调试器进程(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3. `@adobe/aio-cli-plugin-asset-compute` x中出现错误，导致 `Ctrl-C` 无法识别为终止命令。
+ __解决方案__:更 `@adobe/aio-cli-plugin-asset-compute` 新到版本1.4.1+

   ```
   $ aio update
   ```

   ![疑难解答 — aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### AEM{#custom-rendition-missing-from-asset}中的资产中缺少自定义再现

+ __错误：__ 新资产和重新处理的资产处理成功，但缺少自定义演绎版

#### 处理用户档案未应用到祖先文件夹

+ __原因：__ 资产在具有使用自定义worker的处理用户档案的文件夹下不存在
+ __解决方__ 式：将处理用户档案应用到资产的父文件夹

#### 处理用户档案由较低处理用户档案替代

+ __原因：资__ 产存在于应用了自定义Worker处理用户档案的文件夹下，但在该文件夹与资产之间已应用了不使用Customer Worker的其他处理用户档案。
+ __解决方案：__ 合并或以其他方式协调两个处理用户档案并删除中间处理用户档案

### 在AEM{#asset-processing-fails}中资产处理失败

+ __错误：资__ 产处理失败徽章显示在资产上
+ __原因：__ 执行自定义工作器时出错
+ __分辨率：__ 按照使用调试Adobe I/O Runtime [活动](./test-debug/debug.md#aio-app-logs) 的说明操 `aio app logs`作。


