---
title: 构建和部署
description: AdobeCloud Manager有助于代码构建和部署到AEMas a Cloud Service。 在构建过程中的步骤中可能会发生故障，需要采取措施来解决这些故障。 本指南将逐步引导您了解部署中的常见故障，以及如何以最佳方式解决这些故障。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2523'
ht-degree: 0%

---

# 调试AEMas a Cloud Service构建和部署

AdobeCloud Manager有助于代码构建和部署到AEMas a Cloud Service。 在构建过程中的步骤中可能会发生故障，需要采取措施来解决这些故障。 本指南将逐步引导您了解部署中的常见故障，以及如何以最佳方式解决这些故障。

![云管理构建管道](./assets/build-and-deployment/build-pipeline.png)

## 验证

验证步骤只需确保基本Cloud Manager配置有效。 常见的验证失败包括：

### 环境处于无效状态

+ __错误消息：__ 环境处于无效状态。
   ![环境处于无效状态](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__ 管道的目标环境处于过渡状态，此时无法接受新内部版本。
+ __分辨率：__ 等待状态解析为正在运行（或更新可用）状态。 如果要删除环境，请重新创建环境，或选择要生成的其他环境。

### 找不到与管道关联的环境

+ __错误消息：__ 该环境被标记为已删除。
   ![环境标记为已删除](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__ 管道配置为使用的环境已被删除。
即使重新创建了具有相同名称的新环境，Cloud Manager也不会自动将管道重新关联到该同名环境。
+ __分辨率：__ 编辑管道配置，并重新选择要部署到的环境。

### 找不到与管道关联的Git分支

+ __错误消息：__ 无效的管道： XXXXXX。 在存储库中找不到Reason=Branch=xxxx。
   ![无效的管道： XXXXXX。 在存储库中找不到Reason=Branch=xxxx](./assets/build-and-deployment/validation__branch-not-found.png)
+ __原因：__ 管道配置为使用的Git分支已被删除。
+ __分辨率：__ 使用完全相同的名称重新创建缺失的Git分支，或重新配置管道以从其他现有分支构建。

## 构建和单元测试

![构建和单元测试](./assets/build-and-deployment/build-and-unit-testing.png)

构建和单元测试阶段执行Maven构建(`mvn clean package`)从管道配置的Git分支签出的项目。

此阶段中发现的错误应该可以在本地重新生成项目，但以下情况除外：

+ Maven依赖项不可用于 [Maven Central](https://search.maven.org/) 使用，并且包含依赖项的Maven存储库满足以下任一条件：
   + 无法从Cloud Manager访问，例如专用内部Maven存储库或Maven存储库需要身份验证，并且提供的凭据不正确。
   + 未在项目的 `pom.xml`. 请注意，建议不要包含Maven存储库，因为它会增加构建时间。
+ 由于计时问题，单元测试失败。 当单元测试对时间敏感时，可能会发生这种情况。 一个强有力的指标所依赖的 `.sleep(..)` 在测试代码中。
+ 使用不支持的Maven插件。

## 代码扫描

![代码扫描](./assets/build-and-deployment/code-scanning.png)

代码扫描会结合使用Java和特定于AEM的最佳实践来执行静态代码分析。

如果代码中存在严重安全漏洞，则代码扫描会导致生成失败。 可以覆盖较小的违规，但建议修复这些违规。 请注意，代码扫描并不完善，可能会导致 [误报](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives).

要解决代码扫描问题，请下载Cloud Manager通过 **下载详细信息** 按钮并查看任何条目。

有关更多详细信息，请参阅AEM特定规则，请参阅Cloud Manager文档 [自定义AEM特定的代码扫描规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## 生成图像

![生成图像](./assets/build-and-deployment/build-images.png)

构建图像负责将在“构建和单元测试”步骤中创建的构建代码工件与AEM版本相结合，以形成一个可部署工件。

虽然在构建和单元测试期间发现任何代码构建和编译问题，但在尝试将自定义构建工件与AEM版本结合时，可能会发现配置或结构问题。

### 复制OSGi配置

当目标AEM环境通过运行模式解析多个OSGi配置时，构建图像步骤失败并出现错误：

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### 原因1

+ __原因：__ AEM项目的all包包含多个代码包，并且多个代码包提供了相同的OSGi配置，这会导致冲突，从而导致“生成图像”步骤无法决定应该使用哪个代码包，进而导致生成失败。 请注意，这不适用于OSGi工厂配置，只要它们具有唯一的名称。
+ __分辨率：__ 查看作为AEM应用程序的一部分部署的所有代码包（包括任何包含的第三方代码包），查找通过运行模式解析到目标环境的重复OSGi配置。 在AEM as a Cloud Service中不可能使用错误消息的“将mergeConfigurations标志设置为true”指南，应当忽略该指南。

#### 原因2

+ __原因：__ AEM项目错误地包含两次相同的代码包，从而导致重复所述包中包含的任何OSGi配置。
+ __分辨率：__ 查看嵌入到所有项目中的所有pom.xml包，并确保它们具有 `filevault-package-maven-plugin` [配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) 设置为 `<cloudManagerTarget>none</cloudManagerTarget>`.

### 格式错误的repoinit脚本

Repoinit脚本定义基线内容、用户、ACL等。 在AEMas a Cloud Service中，repoinit脚本在构建图像期间应用，但在AEM SDK的本地快速启动中，这些脚本在激活OSGi repoinit工厂配置时应用。 因此，Repoinit脚本可能会在AEM SDK的本地快速启动中悄然失败（进行日志记录），但会导致“生成图像”步骤失败，从而停止部署。

+ __原因：__ repoinit脚本的格式不正确。 这可能会使存储库处于不完整状态，因为不会针对存储库执行失败脚本之后的任何repoinit脚本。
+ __分辨率：__ 在部署repoinit脚本OSGi配置时，查看AEM SDK的本地快速启动，以确定错误是否及其内容。

### 不满意的repoinit内容依赖性

Repoinit脚本定义基线内容、用户、ACL等。 在AEM SDK的本地快速入门中，当激活repoinit OSGi工厂配置时，或者在存储库处于活动状态且可能直接或通过内容包引起内容更改后，会应用repoinit脚本。 在AEMas a Cloud Service中，在生成图像期间对repoinit脚本可能不包含repoinit脚本所依赖内容的存储库应用repoinit脚本。

+ __原因：__ repoinit脚本依赖于不存在的内容。
+ __分辨率：__ 确保repoinit脚本所依赖的内容存在。 通常，这表示未充分定义的repoinit脚本缺少定义这些缺失但必需的内容结构的指令。 通过删除AEM、解压缩Jar并将包含repoinit脚本的repoinit OSGi配置添加到安装文件夹并启动AEM，可以在本地重现这种情况。 该错误将出现在AEM SDK本地快速入门的error.log中。


### 应用程序的核心组件版本高于部署的版本

_此问题仅影响不会自动更新到最新AEM版本的非生产环境。_

AEMas a Cloud Service会在每个AEM版本中自动包含最新的核心组件版本，这意味着AEMas a Cloud Service环境在自动或手动更新后才会部署最新版本的核心组件。

在以下情况下，“生成图像”步骤可能会失败：

+ 部署应用程序会更新中的核心组件maven依赖项版本 `core` （OSGi包）项目
+ 然后，部署应用程序将部署到沙盒（非生产）AEMas a Cloud Service环境，该环境尚未更新为使用包含新核心组件版本的AEM版本。

为防止出现此故障，只要有AEMas a Cloud Service环境的更新可用，就应将该更新作为下一个构建/部署的一部分包括在内，并始终确保在应用程序代码库中增加核心组件版本后包含这些更新。

+ __症状：__
“构建映像”步骤失败，并出现一个ERROR报告 
`com.adobe.cq.wcm.core.components...` 特定版本范围内的包无法由导入 `core` 项目。

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __原因：__  应用程序的OSGi包(在 `core` 项目)从核心组件核心依赖项导入Java类，但版本级别与部署到AEMas a Cloud Service的版本级别不同。
+ __解决方法:__
   + 使用Git，还原到核心组件版本增量之前存在的工作提交。 将此承诺推送到Cloud Manager Git分支，并从该分支执行环境更新。 此操作会将AEMas a Cloud Service升级到最新的AEM版本，该版本将包括较新的核心组件版本。 在将AEMas a Cloud Service更新到最新的AEM版本（将具有最新的核心组件版本）后，重新部署最初失败的代码。
   + 要在本地重现此问题，请确保AEM SDK版本与AEMas a Cloud Service环境使用的AEM发行版本相同。


### 创建Adobe支持案例

如果上述故障诊断方法不能解决问题，请通过以下方式创建Adobe支持案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支持选项卡>创建案例

   _如果您是多个Adobe组织的成员，请确保在创建案例之前在Adobe组织切换器中选择具有失败管道的Adobe组织。_

## 部署到

“部署到”步骤负责获取在构建映像中生成的代码构件，使用该构件启动新的AEM创作和发布服务，并在成功后删除任何旧的AEM创作和发布服务。 在此步骤中还会安装和更新可变内容包和索引。

熟悉以下内容 [AEMas a Cloud Service日志](./logs.md) 在调试“部署到”步骤之前。 此 `aemerror` 日志包含有关pod启动和关闭的信息，这些信息可能与部署到问题有关。 请注意，通过Cloud Manager的部署到步骤中的下载日志按钮可用的日志不是 `aemerror` 日志，但不包含与您的应用程序启动相关的详细信息。

![部署到](./assets/build-and-deployment/deploy-to.png)

“部署到”步骤可能失败的三个主要原因是：

### Cloud Manager管道包含旧的AEM版本

+ __原因：__ Cloud Manager管道包含的AEM版本比部署到目标环境的版本旧。 当重复使用管道并指向运行更高版本AEM的新环境时，可能会发生这种情况。 可以通过检查环境的AEM版本是否大于管道的AEM版本来识别它。
   ![Cloud Manager管道包含旧的AEM版本](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __解决方法:__
   + 如果目标环境具有可用更新，请从环境的操作中选择更新，然后重新运行内部版本。
   + 如果目标环境没有可用更新，则意味着它运行的是最新版本的AEM。 要解决此问题，请删除管道并重新创建它。


### Cloud Manager超时

在新部署的AEM服务启动期间运行的代码需要很长时间，以至于Cloud Manager在部署完成之前超时。 在这些情况下，部署可能最终成功，即使认为Cloud Manager状态报告为失败。

+ __原因：__ 自定义代码可以执行在OSGi捆绑包或组件生命周期早期触发的操作（如大型查询或内容遍历），这会显着延迟AEM的启动时间。
+ __分辨率：__ 查看在OSGi捆绑包的生命周期早期运行的代码实施，并查看 `aemerror` Cloud Manager显示的故障时间（以GMT为单位记录时间）前后的AEM创作和发布服务的日志，并查找指示任何自定义日志运行进程的日志消息。

### 不兼容的代码或配置

大多数代码和配置违规是在内部版本中的早期捕获的，但是，自定义代码或配置可能与AEMas a Cloud Service不兼容，并且在容器中执行之前无法检测到。

+ __原因：__ 自定义代码可能会调用很长的操作，例如大型查询或内容遍历，这些操作在OSGi捆绑包或组件生命周期的早期触发时会显着延迟AEM的启动时间。
+ __分辨率：__ 查看 `aemerror` Cloud Manager显示的失败时间（以GMT为单位记录时间）前后的AEM创作和发布服务的日志。
   1. 在日志中查看由自定义应用程序提供的Java类引发的任何错误。 如果发现任何问题，请解决，推送修复的代码，然后重新构建管道。
   1. 查看日志中有关您在自定义应用程序中扩展/交互的AEM方面报告的任何“错误”，并调查这些错误；这些“错误”可能不是直接归因于Java类。 如果发现任何问题，请解决，推送修复的代码，然后重新构建管道。

### 在内容包中包含/var

`/var` 可变，包含各种瞬态运行时内容。 包括 `/var` 在内容包中(例如， `ui.content`)通过Cloud Manager部署可能会导致部署步骤失败。

此问题很难识别，因为它不会导致初始部署失败，而只会导致后续部署。 显着症状包括：

+ 初始部署成功，但AEM Publish服务上似乎不存在作为部署一部分的新增或更改的可变内容。
+ 已阻止AEM创作中的内容激活/停用
+ 后续部署在“部署到”步骤中失败，且“部署到”步骤在大约60分钟后失败。

验证此问题是导致失败行为的原因：

1. 确定作为部署一部分的至少一个内容包写入 `/var`.
1. 验证主（粗体）分发队列是否在以下位置被阻止：
   + AEM创作>工具>部署>分发
      ![已阻止的分发队列](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 后续部署失败时，使用下载日志按钮下载Cloud Manager的“部署到”日志：

   ![将部署下载到日志](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ...并验证日志语句之间是否间隔了约60分钟：

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... 和 ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   请注意，此日志不会包含有关报告为成功的初始部署的这些指标，而只会在后续失败的部署中包含。

+ __原因：__ 用于将内容包部署到AEM Publish服务的AEM复制服务用户无法写入 `/var` 在AEM发布上。 这会导致将内容包部署到AEM Publish服务失败。
+ __分辨率：__ 以下是解决此问题的方法按优先顺序列出：
   1. 如果 `/var` 资源不是必需的，请删除下的任何资源 `/var` 从作为应用程序的一部分部署的内容包中。
   2. 如果 `/var` 资源是必需的，请使用定义节点结构 [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). 可以通过OSGi运行模式，将Repoinit脚本定位到AEM创作、AEM发布或两者。
   3. 如果 `/var` 资源仅在AEM author上需要，不能使用进行合理建模 [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)，将它们移动到仅由安装在AEM Author上的离散内容包 [嵌入](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) 它位于 `all` 打包到AEM Author runmode文件夹中(`<target>/apps/example-packages/content/install.author</target>`)。
   4. 向提供适当的ACL `sling-distribution-importer` 服务用户，如本中所述 [Adobe知识库](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### 创建Adobe支持案例

如果上述故障诊断方法不能解决问题，请通过以下方式创建Adobe支持案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支持选项卡>创建案例

   _如果您是多个Adobe组织的成员，请确保在创建案例之前在Adobe组织切换器中选择具有失败管道的Adobe组织。_
