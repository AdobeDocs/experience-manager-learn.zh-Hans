---
title: AEM Assets的Asset compute可扩展性故障诊断
description: 以下是为AEM Assets开发和部署自定义Asset compute工作程序时可能遇到的常见问题和错误以及解决方案的索引。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 335
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 0%

---

# asset compute可扩展性故障诊断

以下是为AEM Assets开发和部署自定义Asset compute工作程序时可能遇到的常见问题和错误以及解决方案的索引。

## 开发{#develop}

### 返回的部分已绘制/损坏{#rendition-returned-partially-drawn-or-corrupt}

+ __错误__：演绎版未完全呈现（图像时）或已损坏，无法打开。

  ![已返回部分绘制的节目](./assets/troubleshooting/develop__await.png)

+ __原因__：工作人员的 `renditionCallback` 函数退出，然后才能将格式副本完全写入 `rendition.path`.
+ __分辨率__：查看自定义工作进程代码，并确保使用同步执行所有异步调用。 `await`.

## 开发工具{#development-tool}

### asset compute项目中缺少Console.json文件{#missing-console-json}

+ __错误：__ 错误：验证(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js)时缺少必需的文件:XX:YY)在async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__ 此 `console.json` asset compute项目的根目录中缺少文件
+ __分辨率：__ 下载新的 `console.json` 形成您的Adobe I/O项目
   1. 在console.adobe.io中，打开将Asset compute项目配置为使用的Adobe I/O项目
   1. 点按 __下载__ 右上角的按钮
   1. 使用文件名将下载的文件保存到Asset compute项目的根目录下 `console.json`

### manifest.yml中的YAML缩进不正确{#incorrect-yaml-indentation}

+ __错误：__ YAMLException：第X行、第Y列的映射条目缩进错误：(通过标准输出自 `aio app run` 命令)
+ __原因：__ Yaml文件具有白空格敏感性，您的缩进可能不正确。
+ __分辨率：__ 查看您的 `manifest.yml` 并确保所有缩进均正确。

### memorySize限制设置过低{#memorysize-limit-is-set-too-low}

+ __错误：__  本地开发服务器OpenWiskError：PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true返回HTTP 400（错误请求） —>“请求内容格式错误：请求失败：内存比允许的阈值134217728 B低64 MB”
+ __原因：__ A `memorySize` 以下位置中工作人员的限制： `manifest.yml` 设置为低于错误消息报告的最小允许阈值（字节）。
+ __分辨率：__  查看 `memorySize` 中的限制 `manifest.yml` 并确保它们都大于允许的最小阈值。

### 由于缺少private.key，开发工具无法启动{#missing-private-key}

+ __错误：__ 本地Dev ServerError：在validatePrivateKeyFile中缺少必需的文件.... (通过标准输出自 `aio app run` 命令)
+ __原因：__ 此 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 中的值 `.env` 文件，未指向 `private.key` 或 `private.key` 当前用户无法读取。
+ __分辨率：__ 查看 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 中的值 `.env` 文件，并确保它包含到 `private.key` 文件系统上的。

### 源文件下拉列表不正确{#source-files-dropdown-incorrect}

asset compute开发工具可能会进入提取陈旧数据的状态，这种情况在 __源文件__ 显示错误项目的下拉列表。

+ __错误：__ 源文件下拉列表显示不正确的项目。
+ __原因：__ 过时的缓存浏览器状态导致
+ __分辨率：__ 在浏览器中完全清除浏览器选项卡的“应用程序状态”、浏览器缓存、本地存储和Service Worker。

### devToolToken查询参数缺失或无效{#missing-or-invalid-devtooltoken-query-parameter}

+ __错误：__ asset compute开发工具中的“未授权”通知
+ __原因：__ `devToolToken` 缺失或无效
+ __分辨率：__ 关闭“Asset compute开发工具”浏览器窗口，终止通过启动的任何正在运行的开发工具进程。 `aio app run` 命令，然后重新启动开发工具(使用 `aio app run`)。

### 无法删除源文件{#unable-to-remove-source-files}

+ __错误：__ 无法从开发工具UI中删除已添加的源文件
+ __原因：__ 此功能尚未实施
+ __分辨率：__ 使用中定义的凭据登录您的云存储提供商 `.env`. 找到开发工具使用的容器(也在 `.env`)，导航到 __源__ 文件夹，并删除所有源图像。 您可能需要执行中概述的步骤 [源文件下拉列表不正确](#source-files-dropdown-incorrect) 如果删除的源文件继续显示在下拉列表中，因为它们可能缓存在开发工具“应用程序状态”的本地中。

  ![Microsoft Azure Blob存储](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 测试{#test}

### 测试执行期间未生成演绎版{#test-no-rendition-generated}

+ __错误：__ 失败：未生成节目。
+ __原因：__ 辅助进程由于意外错误（如JavaScript语法错误）未能生成演绎版。
+ __分辨率：__ 查看测试执行的 `test.log` 在 `/build/test-results/test-worker/test.log`. 找到此文件中与测试失败案例对应的部分，并检查错误。

  ![疑难解答 — 未生成演绎版](./assets/troubleshooting/test__no-rendition-generated.png)

### 测试生成错误的演绎版，导致测试失败{#tests-generates-incorrect-rendition}

+ __错误：__ 失败：演绎版“rendition.xxx”未按预期运行。
+ __原因：__ 辅助进程输出与不相同的演绎版 `rendition.<extension>` 在测试用例中提供。
   + 如果预期 `rendition.<extension>` 文件创建方式与测试用例中本地生成的格式副本创建方式不完全相同，测试可能会失败，因为比特之间存在一些差异。 例如，如果Asset compute工作进程使用API更改对比度，并通过调整Adobe Photoshop CC中的对比度创建预期结果，则文件可能显示相同，但位中的细微变化可能不同。
+ __分辨率：__ 通过导航到，查看测试中的演绎版输出 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`，并将其与测试用例中的预期演绎版文件进行比较。 要创建完全预期的资产，请执行以下操作：
   + 使用开发工具生成演绎版，验证其是否正确，并将其用作预期的演绎版文件
   + 或者，验证测试生成的文件位于 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`，验证它是否正确，并将其用作预期的演绎版文件

## 调试

### 调试器未附加{#debugger-does-not-attach}

+ __错误__：处理启动项时出错：错误：无法连接到位于……的调试目标
+ __原因__：Docker Desktop未在本地系统上运行。 通过查看VS代码调试控制台（“查看”>“调试控制台”）来验证此设置，确认报告了此错误。
+ __分辨率__：开始 [Docker Desktop并确认安装了必需的Docker映像](./set-up/development-environment.md#docker).

### 断点未暂停{#breakpoints-no-pausing}

+ __错误__：从可调试的开发工具运行Asset compute工作进程时，VS Code不会在断点处暂停。

#### 未附加VS代码调试器{#vs-code-debugger-not-attached}

+ __原因：__ VS代码调试器已停止/断开连接。
+ __分辨率：__ 重新启动VS代码调试器，并通过观看VS代码调试输出控制台（“查看”>“调试控制台”）来验证其是否附加

#### 辅助进程执行开始后附加的VS代码调试器{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ VS代码调试器在点击之前未连接 __运行__ 在开发工具中。
+ __分辨率：__ 通过查看VS Code的Debug Console（“查看”>“调试控制台”），然后从“开发工具”重新运行Asset compute工作程序，确保已附加调试器。

### Worker在调试时超时{#worker-times-out-while-debugging}

+ __错误__：调试控制台报告“操作将在 — XXX毫秒内超时”或 [asset compute开发工具的](./develop/development-tool.md) 演绎版预览无限期旋转或
+ __原因__：中定义的Worker超时 [manifest.yml](./develop/manifest.md) 调试期间超出范围。
+ __分辨率__：临时增加工作线程的超时，超时时间： [manifest.yml](./develop/manifest.md) 或加速调试活动。

### 无法终止调试器进程{#cannot-terminate-debugger-process}

+ __错误__： `Ctrl-C` 在命令行上不会终止调试器进程(`npx adobe-asset-compute devtool`)。
+ __原因__：中的错误 `@adobe/aio-cli-plugin-asset-compute` 1.3.x，结果 `Ctrl-C` 未被识别为终止命令。
+ __分辨率__：更新 `@adobe/aio-cli-plugin-asset-compute` 到版本1.4.1+

  ```
  $ aio update
  ```

  ![故障排除 — aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### AEM中的资源缺少自定义演绎版{#custom-rendition-missing-from-asset}

+ __错误：__ 已成功新建和重新处理资源，但缺少自定义演绎版

#### 处理配置文件未应用于上级文件夹

+ __原因：__ 资产不存在于具有使用自定义工作线程的处理配置文件的文件夹下
+ __分辨率：__ 将处理配置文件应用到资源的上级文件夹

#### 处理配置文件被较低的处理配置文件取代

+ __原因：__ 资产存在于应用了自定义工作进程配置文件的文件夹下，但已在该文件夹和资产之间应用了未使用客户工作进程的其他处理配置文件。
+ __分辨率：__ 合并或以其他方式协调两个处理配置文件，并删除中间处理配置文件

### AEM中的资源处理失败{#asset-processing-fails}

+ __错误：__ 资产上显示“资产处理失败”标记
+ __原因：__ 执行自定义工作进程时出错
+ __分辨率：__ 请按照 [调试Adobe I/O Runtime激活](./test-debug/debug.md#aio-app-logs) 使用 `aio app logs`.
