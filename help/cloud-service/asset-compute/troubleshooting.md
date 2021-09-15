---
title: 对AEM Assets的Asset compute可扩展性进行故障诊断
description: 以下是开发和部署AEM Assets自定义Asset compute工作程序时可能遇到的常见问题和错误以及相关解决方案的索引。
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# asset compute可扩展性故障诊断

以下是开发和部署AEM Assets自定义Asset compute工作程序时可能遇到的常见问题和错误以及相关解决方案的索引。

## 开发{#develop}

### 返回部分绘制/损坏的呈现版本{#rendition-returned-partially-drawn-or-corrupt}

+ __错误__:演绎版呈现不完整（当图像时）或已损坏且无法打开。

   ![返回部分绘制的呈现版本](./assets/troubleshooting/develop__await.png)

+ __原因__:工作者的函 `renditionCallback` 数在再现完全写入之前退出 `rendition.path`。
+ __解决办法__:查看自定义工作程序代码，并确保使用同步执行所有异步调 `await`用。

## 开发工具{#development-tool}

### asset compute项目中缺少Console.json文件{#missing-console-json}

+ __错误：__ 错误：验证时缺少必需文件(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.:XX:jsYY)（异步）setupAssetCompute(.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.:XX:jsYY)
+ __原因：__ 文 `console.json` 件在Asset compute项目的根中缺失
+ __解决方案：__ 从您的Adobe I/O `console.json` 项目下载新表单
   1. 在console.adobe.io中，打开Adobe I/O项目，该Asset compute项目配置为使用
   1. 点按右上方的&#x200B;__Download__&#x200B;按钮
   1. 使用文件名`console.json`将下载的文件保存到Asset compute项目的根目录中

### manifest.yml中的YAML缩进不正确{#incorrect-yaml-indentation}

+ __错误：__ YAMLException:在X，列Y：（通过从命令中标准退出）处映射条目的缩进 `aio app run` 错误
+ __原因：__ Yaml文件在空格上比较敏感，可能是因为缩进不正确。
+ __分辨率：__ 查看并 `manifest.yml` 确保所有缩进均正确。

### memorySize限制设置过低{#memorysize-limit-is-set-too-low}

+ __错误：__  本地开发服务器OpenWhiskError:PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true返回HTTP 400（错误请求） — > &quot;请求内容的格式错误：requirement失败：内存低于允许的134217728 B&quot;阈值64 MB
+ __原因：__ 中 `memorySize` 工作程序的限制设 `manifest.yml` 置为低于错误消息报告的允许的最小阈值（以字节为单位）。
+ __解决办法：__  查看 `memorySize` 中的限 `manifest.yml` 制，并确保这些限制都大于允许的最低阈值。

### 由于缺少private.key，开发工具无法启动{#missing-private-key}

+ __错误：__ 本地开发服务器错误：在validatePrivateKeyFile中缺少必需文件…….（通过`aio app run`命令中的标准输出）
+ __原因：__ 文 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 件 `.env` 中的值未指 `private.key` 向或 `private.key` 当前用户无法读取。
+ __解决办法：__ 查看文 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 件 `.env` 中的值，并确保它包含文件系统上 `private.key` 的完整绝对路径。

### 源文件下拉列表不正确{#source-files-dropdown-incorrect}

asset compute开发工具可能会进入其提取过时数据的状态，在显示错误项的&#x200B;__源文件__&#x200B;下拉列表中最为明显。

+ __错误：__ 源文件下拉列表显示错误项目。
+ __原因：__ 缓存的浏览器状态过时导致
+ __解决办法：__ 在浏览器中完全清除浏览器选项卡的“应用程序状态”、浏览器缓存、本地存储和服务工作程序。

### devToolToken查询参数缺失或无效{#missing-or-invalid-devtooltoken-query-parameter}

+ __错误：__ Asset compute开发工具中的“未授权”通知
+ __原因：__ `devToolToken` 缺失或无效
+ __解决办法：__ 关闭“Asset compute开发工具”浏览器窗口，终止通过命令启动的任何正在运行的开发工具 `aio app run` 进程，然后重新启动开发工具(使用 `aio app run`)。

### 无法删除源文件{#unable-to-remove-source-files}

+ __错误：__ 无法从开发工具UI中删除添加的源文件
+ __原因：__ 尚未实施此功能
+ __解决方案：__ 使用中定义的凭据登录云存储提供商 `.env`。找到开发工具使用的容器（也在`.env`中指定），导航到&#x200B;__source__&#x200B;文件夹，然后删除任何源图像。 如果已删除的源文件继续显示在下拉菜单中，则您可能需要执行[源文件下拉列表中所述的步骤（错误](#source-files-dropdown-incorrect)），因为它们可能会缓存在开发工具“应用程序状态”中。

   ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 测试{#test}

### 测试执行期间未生成任何呈现版本{#test-no-rendition-generated}

+ __错误：__ 失败：未生成演绎版。
+ __原因：__ 工作程序由于意外错误（如JavaScript语法错误）而无法生成演绎版。
+ __解决办法：__ 在上查看测试 `test.log` 执 `/build/test-results/test-worker/test.log`行。在此文件中找到与失败测试用例对应的部分，并检查错误。

   ![故障诊断 — 未生成任何演绎版](./assets/troubleshooting/test__no-rendition-generated.png)

### 测试生成不正确的演绎版，导致测试失败{#tests-generates-incorrect-rendition}

+ __错误：__ 失败：格式副本“rendition.xxx”不如预期。
+ __原因：__ 工作程序输出的演绎版与测试用例 `rendition.<extension>` 中提供的不同。
   + 如果预期的`rendition.<extension>`文件的创建方式与测试案例中本地生成的再现的创建方式不同，则测试可能会失败，因为位中可能存在一些差异。 例如，如果Asset compute工作者使用API更改对比度，并通过调整Adobe Photoshop CC中的对比度来创建预期结果，则文件可能显示相同，但位中的细微变化可能不同。
+ __分辨率：__ 通过导航到来查看测试中的呈现版本输出， `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`并将其与测试案例中的预期呈现版本文件进行比较。要创建确切的预期资产，请执行以下任一操作：
   + 使用开发工具生成演绎版，验证其正确性，并将其用作预期的演绎版文件
   + 或者，在`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`验证测试生成的文件，验证其正确性，并将该文件用作预期的呈现版本文件

## 调试

### 调试器未附加{#debugger-does-not-attach}

+ __错误__:处理启动项时出错：错误：无法在……连接到调试目标
+ __原因__:Docker Desktop未在本地系统上运行。查看VS代码调试控制台（查看>调试控制台），确认报告了此错误，以验证此错误。
+ __解决办法__:启 [动Docker Desktop，并确认已安装必需的Docker映像](./set-up/development-environment.md#docker)。

### 断点不会暂停{#breakpoints-no-pausing}

+ __错误__:从可调试开发工具运行Asset compute工作程序时，与代码不会在断点处暂停。

#### 未附加VS代码调试器{#vs-code-debugger-not-attached}

+ __原因：__ VS代码调试器已停止/断开连接。
+ __解决办法：__ 重新启动VS代码调试器，并通过观看VS代码调试输出控制台（查看>调试控制台）来验证它是否附加了

#### 工作程序开始执行后附加的VS代码调试器{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ 在点按运行开发工具之前，VS代码调试 ____ 器未附加。
+ __解决办法：__ 确保已通过查看与代码的调试控制台（查看>调试控制台）附加调试器，然后从开发工具中重新运行Asset compute工作程序。

### 调试时工作程序超时{#worker-times-out-while-debugging}

+ __错误__:Debug Console报告“操作将以 — XXX毫秒为单位超时”，或者Asset compute开 [发工具的](./develop/development-tool.md) 呈现预览将无限期地旋转或
+ __原因__:在调试期间，超出了清单。 [](./develop/manifest.md) 清单中定义的工作器超时。
+ __解决办法__:暂时增加清单中工作程序的超 [时时间。](./develop/manifest.md) 草稿可加快调试活动。

### 无法终止调试器进程{#cannot-terminate-debugger-process}

+ __错误__: `Ctrl-C` 在命令行中，不会终止调试器进程(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3. `@adobe/aio-cli-plugin-asset-compute` x中出现错误，导致 `Ctrl-C` 无法识别为终止命令。
+ __解决办法__:更 `@adobe/aio-cli-plugin-asset-compute` 新至版本1.4.1+

   ```
   $ aio update
   ```

   ![故障诊断 — aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### AEM资产中缺少自定义演绎版{#custom-rendition-missing-from-asset}

+ __错误：__ 已成功处理新资产和重新处理资产，但缺少自定义演绎版

#### 处理配置文件未应用于上级文件夹

+ __原因：__ 资产在具有使用自定义工作程序的处理配置文件的文件夹下不存在
+ __解决方案：__ 将处理配置文件应用到资产的上级文件夹

#### 处理配置文件由下级处理配置文件取代

+ __原因：__ 资产存在于应用了自定义工作程序处理配置文件的文件夹下方，但该文件夹与资产之间已应用了不使用客户工作程序的其他处理配置文件。
+ __解决办法：__ 合并或以其他方式协调两个处理配置文件，并删除中间处理配置文件

### 资产处理在AEM中失败{#asset-processing-fails}

+ __错误：__ 资产上显示“资产处理失败”标记
+ __原因：__ 执行自定义工作程序时出错
+ __解决办法：__ 按照有关使用调试 [Adobe I/O Runtime](./test-debug/debug.md#aio-app-logs) 活动的说 `aio app logs`明操作。
