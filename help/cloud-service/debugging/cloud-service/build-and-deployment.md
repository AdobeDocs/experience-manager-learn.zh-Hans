---
title: 内部版本和部署
description: AdobeCloud Manager可帮助构建代码并将其部署到AEMas a Cloud Service。 在构建过程中的步骤中可能会发生失败，需要采取操作来解决这些问题。 本指南将指导您逐步了解部署中的常见故障，以及如何以最佳方式处理这些故障。
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
source-git-commit: 7a4585146b52d14f32645c6889c9c015e9991809
workflow-type: tm+mt
source-wordcount: '2524'
ht-degree: 0%

---

# 调试AEMas a Cloud Service内部版本和部署

AdobeCloud Manager可帮助构建代码并将其部署到AEMas a Cloud Service。 在构建过程中的步骤中可能会发生失败，需要采取操作来解决这些问题。 本指南将指导您逐步了解部署中的常见故障，以及如何以最佳方式处理这些故障。

![云管理构建管道](./assets/build-and-deployment/build-pipeline.png)

## 验证

验证步骤只需确保基本的Cloud Manager配置有效即可。 常见验证失败包括：

### 环境处于无效状态

+ __错误消息：__ 环境处于无效状态。
   ![环境处于无效状态](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__ 管道的目标环境处于过渡状态，此时它无法接受新内部版本。
+ __解决办法：__ 等待状态解析为正在运行（或更新可用）状态。 如果删除了环境，请重新创建该环境，或选择要构建到的其他环境。

### 找不到与管道关联的环境

+ __错误消息：__ 该环境被标记为已删除。
   ![环境被标记为已删除](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__ 已删除配置为使用的管道环境。
即使重新创建具有相同名称的新环境，Cloud Manager也不会自动将管道重新关联到该同名环境。
+ __解决办法：__ 编辑管道配置，然后重新选择要部署到的环境。

### 找不到与管道关联的Git分支

+ __错误消息：__ 无效的管道：XXXXX。 在存储库中未找到Reason=Branch=xxxx。
   ![无效的管道：XXXXX。 在存储库中未找到Reason=Branch=xxxx](./assets/build-and-deployment/validation__branch-not-found.png)
+ __原因：__ 已删除管道配置为使用的Git分支。
+ __解决办法：__ 使用完全相同的名称重新创建缺少的Git分支，或重新配置管道以从其他现有分支构建。

## 构建和单元测试

![构建和单元测试](./assets/build-and-deployment/build-and-unit-testing.png)

构建和单元测试阶段执行Maven构建(`mvn clean package`)从管道配置的Git分支中签出的项目。

在此阶段发现的错误应可在本地重建项目，但以下情况除外：

+ Maven依赖项不可用 [中马文](https://search.maven.org/) ，且包含依赖项的Maven存储库为：
   + 无法从Cloud Manager访问（如私有内部Maven存储库），或者Maven存储库需要身份验证且提供的凭据不正确。
   + 未在项目的 `pom.xml`. 请注意，不鼓励包括Maven存储库，因为它会增加构建时间。
+ 由于计时问题，设备测试失败。 当单元测试对时间敏感时，可能会发生这种情况。 强劲指标正依赖 `.sleep(..)` 中。
+ 使用不支持的Maven插件。

## 代码扫描

![代码扫描](./assets/build-and-deployment/code-scanning.png)

代码扫描使用特定于Java和AEM的最佳实践组合来执行静态代码分析。

如果代码中存在关键安全漏洞，则代码扫描会导致生成失败。 可以覆盖较小的违规，但建议修复这些违规。 请注意，代码扫描不完善，可能导致 [误报](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives).

要解决代码扫描问题，请通过 **下载详细信息** 按钮并查看所有条目。

有关更多详细信息，请参阅AEM特定规则，请参阅Cloud Manager文档 [自定义AEM特定代码扫描规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## 构建图像

![构建图像](./assets/build-and-deployment/build-images.png)

构建图像负责将构建和单元测试步骤中创建的构建代码对象与AEM版本相结合，以形成单个可部署的对象。

虽然在构建和单元测试期间发现任何代码构建和编译问题，但在尝试将自定义构建对象与AEM版本组合时，可能会发现配置或结构问题。

### 复制OSGi配置

当多个OSGi配置通过目标AEM环境的运行模式解析时，“生成图像”步骤会失败，并出现以下错误：

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### 原因1

+ __原因：__ AEM项目的所有包中包含多个代码包，并且多个代码包中提供了相同的OSGi配置，从而导致冲突，从而导致生成图像步骤无法确定应使用哪个包，从而导致生成失败。 请注意，这不适用于OSGi工厂配置，只要它们具有唯一的名称。
+ __解决办法：__ 查看作为AEM应用程序一部分部署的所有代码包（包括任何包含的第三方代码包），查找可通过运行模式解析到目标环境的重复OSGi配置。 在AEM as a Cloud Service中，错误消息的“将mergeConfigurations标志设置为true”指导不可能，应当忽略该指南。

#### 原因2

+ __原因：__ AEM项目错误地包含同一代码包两次，从而导致复制包中包含的任何OSGi配置。
+ __解决办法：__ 查看所有项目中嵌入的包的所有pom.xml，并确保它们具有 `filevault-package-maven-plugin` [配置](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) 设置为 `<cloudManagerTarget>none</cloudManagerTarget>`.

### 错误的重新指向脚本

重新指向脚本定义基线内容、用户、ACL等。 在AEMas a Cloud Service中，重新指示脚本在构建映像期间应用，但在AEM SDK的本地快速启动中，在激活OSGi重新指示工厂配置时会应用重新指示脚本。 因此，AEM SDK的本地快速启动脚本可能会静悄悄地失败（带有日志记录），但会导致“生成映像”步骤失败，从而停止部署。

+ __原因：__ 重新指向脚本的格式错误。 请注意，这可能会使您的存储库处于不完整状态，因为在对存储库执行失败脚本后，将会执行任何重新指向脚本。
+ __解决办法：__ 部署重新指向脚本OSGi配置时，请查看AEM SDK的本地快速启动，以确定错误是否以及错误是什么。

### 未满足的重定向内容依赖项

重新指向脚本定义基线内容、用户、ACL等。 在AEM SDK的本地快速启动中，在激活重新指向OSGi工厂配置时，或者换句话说，在存储库处于活动状态后（可能直接或通过内容包发生了内容更改）应用重新指向脚本。 在AEMas a Cloud Service中，在生成图像期间，会针对某个存储库应用重新指向脚本，该存储库可能不包含重新指向脚本所依赖的内容。

+ __原因：__ 重新指向脚本取决于不存在的内容。
+ __解决办法：__ 确保重定向脚本所依赖的内容存在。 通常，这表示定义不充分的重新指向脚本，该脚本缺少定义这些缺失但必需的内容结构的指令。 这可以通过以下方法在本地重现：删除AEM、解包Jar并将包含重新指向脚本的重新指向OSGi配置添加到安装文件夹，然后启动AEM。 该错误本身将出现在AEM SDK本地快速启动的error.log中。


### 应用程序的核心组件版本大于已部署的版本

_此问题仅会影响未自动更新至最新AEM版本的非生产环境。_

AEM as a Cloud Service会在每个AEM版本中自动包含最新的核心组件版本，这意味着在自动或手动更新AEMas a Cloud Service环境后，会将最新版本的核心组件部署到该环境。

在以下情况下，生成图像步骤可能会失败：

+ 部署应用程序会更新 `core` （OSGi包）项目
+ 然后，部署的应用程序将部署到沙盒（非生产）AEMas a Cloud Service环境，该环境尚未更新为使用包含该新核心组件版本的AEM版本。

为防止出现此故障，每当AEMas a Cloud Service环境的更新可用时，都应将更新包含在下一个生成/部署中，并始终确保在应用程序代码库中增加核心组件版本后再包含更新。

+ __症状：__
“生成图像”步骤失败，并出现错误报告，该错误显示 
`com.adobe.cq.wcm.core.components...` 无法导入特定版本范围中的包 `core` 项目。

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __原因：__  应用程序的OSGi包(在 `core` 项目)从核心组件核心依赖项中导入Java类，其版本级别与部署到AEMas a Cloud Service的版本级别不同。
+ __解决方法:__
   + 使用Git，还原到核心组件版本增量之前存在的工作提交。 将此提交推送到Cloud Manager Git分支，然后从此分支中执行环境更新。 这会将AEMas a Cloud Service升级到最新的AEM版本，该版本将包括更高的核心组件版本。 将AEMas a Cloud Service更新到具有最新核心组件版本的最新AEM版本后，请重新部署最初失败的代码。
   + 要在本地重现此问题，请确保AEM SDK版本与AEMas a Cloud Service环境所使用的AEM版本相同。


### 创建Adobe支持案例

如果上述疑难解答方法无法解决问题，请通过以下方式创建Adobe支持案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支持选项卡>创建案例

   _如果您是多个Adobe组织的成员，请在创建案例之前，确保在Adobe组织切换器中选择了管道出现故障的Adobe组织。_

## 部署到

部署到步骤负责获取生成图像中生成的代码对象，使用该对象启动新的AEM创作和发布服务，并在成功后删除任何旧的AEM创作和发布服务。 此步骤中还安装和更新了可变内容包和索引。

熟悉 [AEMas a Cloud Service日志](./logs.md) 在调试部署到步骤之前。 的 `aemerror` 日志包含有关Pod启动和关闭的信息，这些信息可能与“部署到问题”相关。 请注意，通过Cloud Manager的“部署到”步骤中的“下载日志”按钮提供的日志不是 `aemerror` 日志，并且不包含与应用程序启动相关的详细信息。

![部署到](./assets/build-and-deployment/deploy-to.png)

部署到步骤可能失败的三个主要原因：

### Cloud Manager管道包含旧AEM版本

+ __原因：__ 与部署到目标环境的AEM相比，Cloud Manager管道包含旧版本的Analytics。 如果重复使用管道并将管道指向运行更高版本AEM的新环境，则可能会发生这种情况。 可通过检查环境的AEM版本是否大于管道的AEM版本来标识此问题。
   ![Cloud Manager管道包含旧AEM版本](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __解决方法:__
   + 如果目标环境具有可用更新，请从该环境的操作中选择更新，然后重新运行该内部版本。
   + 如果目标环境没有可用的更新，则意味着它运行的是最新版本的AEM。 要解决此问题，请删除管道并重新创建管道。


### Cloud Manager超时

在新部署的AEM服务启动期间运行的代码会花费如此长的时间，以至于Cloud Manager会在部署完成之前超时。 在这些情况下，部署最终可能会成功，即使认为Cloud Manager状态报告为失败。

+ __原因：__ 自定义代码可能会执行在OSGi包或组件生命周期中早期触发的大型查询或内容遍历等操作，这会显着延迟AEM的启动时间。
+ __解决办法：__ 查看在OSGi包生命周期早期运行的代码的实施，并查看 `aemerror` Cloud Manager显示的AEM创作和发布服务在失败时段（以GMT为单位的日志）的日志，并查找指示任何自定义日志运行进程的日志消息。

### 代码或配置不兼容

大多数代码和配置违规都会在内部版本中的早些时候捕获，但是自定义代码或配置可能与AEMas a Cloud Service不兼容，且一直未被检测到，直到它在容器中执行为止。

+ __原因：__ 自定义代码可能会调用冗长的操作，例如在OSGi包或组件生命周期的早期触发的大型查询或内容遍历，这会显着推迟AEM的启动时间。
+ __解决办法：__ 查看 `aemerror` 如Cloud Manager所示，AEM创作和发布服务在失败时间（以GMT为单位的日志）周围的日志。
   1. 查看日志中自定义应用程序提供的Java类引发的任何错误。 如果发现任何问题，请解决问题，推送固定代码，然后重新构建管道。
   1. 查看您在自定义应用程序中扩展/与之交互的AEM的某些方面所报告的任何错误的日志，并调查这些错误；这些错误可能不会直接归因于Java类。 如果发现任何问题，请解决问题，推送固定代码，然后重新构建管道。

### 在内容包中包含/var

`/var` 包含各种临时运行时内容的变量。 包括 `/var` (例如， `ui.content`)，可能会导致部署步骤失败。

此问题很难识别，因为它不会在初始部署（仅在后续部署）时导致失败。 明显的症状包括：

+ 初始部署成功，但是作为部署一部分的新内容或已更改的内容在AEM发布服务中似乎不存在。
+ 阻止在AEM创作中激活/取消激活内容
+ 后续部署在部署到步骤中失败，部署到步骤在大约60分钟后失败。

验证此问题是失败行为的原因：

1. 确定至少一个内容包是部署的一部分，写入 `/var`.
1. 验证主（粗体）分发队列是否在以下位置被阻止：
   + AEM作者>工具>部署>分发
      ![阻止的分发队列](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 如果后续部署失败，请使用下载日志按钮下载Cloud Manager的“部署到”日志：

   ![将部署下载到日志](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ...并验证log语句之间是否间隔约60分钟：

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... 和 ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   请注意，此日志在报告成功的初始部署中不包含这些指标，而只包含在后续失败的部署中。

+ __原因：__ AEM复制服务用户用于将内容包部署到AEM发布服务时，无法写入 `/var` （在AEM发布中）。 这会导致将内容包部署到AEM发布服务失败。
+ __解决办法：__ 下列解决此问题的方法按优先顺序列出：
   1. 如果 `/var` 资源不必删除 `/var` 从作为应用程序一部分部署的内容包中。
   2. 如果 `/var` 资源是必需的，请使用 [重点](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). 可以通过OSGi运行模式将重新指向脚本定位到AEM作者和/或AEM发布。
   3. 如果 `/var` 仅AEM作者需要资源，且无法使用 [重点](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)，将其移到单独的内容包中，该内容包仅安装在AEM Author上，方法是 [嵌入](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) 在 `all` 包（位于AEM创作运行模式文件夹中）(`<target>/apps/example-packages/content/install.author</target>`)。
   4. 为 `sling-distribution-importer` 如下所述 [AdobeKB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### 创建Adobe支持案例

如果上述疑难解答方法无法解决问题，请通过以下方式创建Adobe支持案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支持选项卡>创建案例

   _如果您是多个Adobe组织的成员，请在创建案例之前，确保在Adobe组织切换器中选择了管道出现故障的Adobe组织。_
