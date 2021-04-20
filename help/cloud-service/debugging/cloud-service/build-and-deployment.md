---
title: 构建和部署
description: Adobe Cloud Manager可简化代码构建和部署，将AEM作为Cloud Service。 在构建过程中的步骤中可能发生故障，需要采取措施解决它们。 本指南将指导您了解部署中的常见故障以及如何以最佳方式处理这些故障。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2542'
ht-degree: 0%

---


# 将AEM作为Cloud Service构建和部署进行调试

Adobe Cloud Manager可简化代码构建和部署，将AEM作为Cloud Service。 在构建过程中的步骤中可能发生故障，需要采取措施解决它们。 本指南将指导您了解部署中的常见故障以及如何以最佳方式处理这些故障。

![云管理构建管道](./assets/build-and-deployment/build-pipeline.png)

## 验证

验证步骤只是确保基本的Cloud Manager配置有效。 常见验证失败包括：

### 环境处于无效状态

+ __错误消息：__ 环境处于无效状态。
   ![环境处于无效状态](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__ 管道的目标环境处于过渡状态，此时它无法接受新构建。
+ __解析：__ 等待状态解析为正在运行（或更新可用）状态。如果删除环境，请重新创建环境，或选择要构建到的其他环境。

### 找不到与管线关联的环境

+ __错误消息：__ 环境标记为已删除。
   ![环境被标记为已删除](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__ 已删除配置为使用的环境。即使重新创建了同名的新环境,Cloud Manager也不会自动将管道重新关联到该同名环境。
+ __分辨率：__ 编辑管线配置，然后重新选择要部署到的环境。

### 找不到与管道关联的Git分支

+ __错误消息：__ 管道无效：XXXXXX。Reason=Branch=xxxx未在存储库中找到。
   ![无效的管道：XXXXXX。Reason=Branch=xxxx在存储库](./assets/build-and-deployment/validation__branch-not-found.png)中找不到
+ __原因：__ 已删除管道配置为使用的Git分支。
+ __解决方__ 案：使用完全相同的名称重新创建缺少的Git分支，或重新配置管道以从其他现有分支构建。

## 构建和单元测试

![构建和单元测试](./assets/build-and-deployment/build-and-unit-testing.png)

构建和单元测试阶段执行从管道的已配置Git分支中签出的项目的Maven构建(`mvn clean package`)。

在此阶段发现的错误应该可以在本地构建项目，但以下例外：

+ 使用[Maven Central](https://search.maven.org/)上不可用的主依赖关系，且包含该依赖关系的Maven存储库为：
   + 与Cloud Manager不可到达，例如私有内部Maven存储库，或Maven存储库需要身份验证，且提供的凭据不正确。
   + 未在项目的`pom.xml`中显式注册。 请注意，不鼓励包括Maven存储库，因为它会增加构建时间。
+ 设备测试因时间问题而失败。 当设备测试对时间敏感时，可能会发生这种情况。 强指示符依赖测试代码中的`.sleep(..)`。
+ 使用不支持的Maven插件。

## 代码扫描

![代码扫描](./assets/build-and-deployment/code-scanning.png)

代码扫描使用混合的Java和AEM特定最佳做法执行静态代码分析。

如果代码中存在关键安全漏洞，则代码扫描会导致生成失败。 较小的违规可以覆盖，但建议修复它们。 请注意，代码扫描不完善，可能导致[误报](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/understand-test-results.html#dealing-with-false-positives)。

要解决代码扫描问题，请通过&#x200B;**下载详细信息**&#x200B;按钮下载Cloud Manager提供的CSV格式报告并查看所有条目。

有关详细信息，请参阅AEM特定规则，请参阅Cloud Manager文档的自定义AEM特定代码扫描规则](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html)。[

## 构建图像

![构建图像](./assets/build-and-deployment/build-images.png)

构建映像负责将在构建和单元测试步骤中创建的构建代码对象与AEM发行版相结合，以形成单个可部署的对象。

虽然在构建和单元测试过程中发现任何代码构建和编译问题，但在尝试将自定义构建对象与AEM发行版组合时可能发现配置或结构问题。

### 重复OSGi配置

当多个OSGi配置通过目标 AEM 环境的运行模式解析时，“构建映像”步骤将失败，并出现以下错误：

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### 原因1

+ __原因：__ AEM项目的所有包都包含多个代码包，并且同一OSGi配置是由多个代码包提供的，这会导致冲突，导致生成映像步骤无法决定应使用哪个，从而导致生成失败。请注意，这不适用于OSGi工厂配置，只要它们有唯一的名称。
+ __解决方__ 案：查看作为AEM应用程序的一部分部署的所有代码包（包括任何包含的第三方代码包），查找通过运行模式解析到目标环境的重复 OSGi配置。在AEM作为云服务时，不能提供错误消息“将mergeConfigurations标志设置为true”的指导，应忽略该指导。

#### 原因2

+ __原因：__ AEM项目错误地包含了两次相同的代码包，导致与包中包含的任何OSGi配置重复。
+ __分辨率：__ 查看所有项目中嵌入的包的所有pom.xml，并确保它们的配置 `filevault-package-maven-plugin` [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) 设置为 `<cloudManagerTarget>none</cloudManagerTarget>`。

### 错误的重新指向脚本

重新指向脚本定义基准内容、用户、ACL等 在AEM中，重新指向脚本是在构建映像期间应用的，但在激活OSGi重新指向工厂配置时，将在AEM SDK的本地快速启动中应用它们。 因此，在AEM SDK的本地快速启动中，Repoinit脚本可能会静静地失败（通过日志记录），但会导致“构建映像”步骤失败，从而停止部署。

+ __原因：__ 重点脚本格式不正确。注意，这可能会使您的存储库处于不完整状态，因为在对存储库执行失败脚本之后的任何重新指向脚本。
+ __解决方__ 案：部署重新指向脚本OSGi配置时，请查看AEM SDK的本地快速启动，以确定是否存在错误以及错误是什么。

### 未满足的重新指向内容依赖关系

重新指向脚本定义基准内容、用户、ACL等 在AEM SDK的本地快速启动中，在激活重新指向OSGi工厂配置时，或换句话说，在存储库处于活动状态后应用重新指向脚本，并且可能直接或通过内容包发生内容更改。 在AEM中，重新指向脚本是作为Cloud Service在构建图像期间对某个存储库应用的，该存储库可能不包含重新指向脚本所依赖的内容。

+ __原因：__ 重新指向脚本取决于不存在的内容。
+ __分辨率：__ 确保重新指向脚本所依赖的内容存在。通常，这表示未充分定义的重新指向脚本，这些脚本缺少定义这些缺失但必需的内容结构的指令。 这可以通过删除AEM、解包Jar并将包含重新指向脚本的重新指向OSGi配置添加到安装文件夹并启动AEM，在本地重现。 该错误将出现在AEM SDK本地快速启动的error.log中。


### 应用程序的核心组件版本高于部署的版本

_此问题仅影响不自动更新至最新AEM版本的非生产环境。_

AEM as a Cloud Service会在每个AEM发行版中自动包含最新的核心组件版本，这意味着在AEM a Cloud Service环境自动或手动更新后，其中已部署了最新版本的核心组件。

在以下情况下，生成映像步骤可能会失败：

+ 部署应用程序更新`core`(OSGi bundle)项目中的核心组件主要依赖项版本
+ 然后将部署应用程序部署到沙箱（非生产）AEM作为尚未更新为使用包含新核心组件版本的AEM发行版的Cloud Service环境。

要防止此失败，只要AEM的更新(作为Cloud Service环境)可用，就将更新作为下一个构建/部署的一部分，并始终确保在应用程序代码库中增加核心组件版本后包含更新。

+ __症状：__
生成映像步骤失败，出现错误报告 
`com.adobe.cq.wcm.core.components...` 项目无法导入特定版本范围的包 `core` 。

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __原因：__  应用程序的OSGi捆绑(在项目中定 `core` 义)从核心组件核心依赖关系中导入Java类，版本级别与部署到AEM作为Cloud Service的版本级别不同。
+ __分辨率:__
   + 使用Git，恢复到核心组件版本增量之前存在的工作提交。 将此提交推送到Cloud Manager Git分支，并执行来自此分支的环境更新。 这将将AEM作为Cloud Service升级到最新的AEM版本，其中将包括更高的核心组件版本。 将AEM作为Cloud Service更新到具有最新核心组件版本的最新AEM发行版后，请重新部署最初失败的代码。
   + 要在本地重现此问题，请确保AEM SDK版本与AEM环境使用的AEM版本相同。


### 创建Adobe支持案例

如果上述疑难解答方法未解决问题，请通过以下方式创建Adobe支持案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支持选项卡>创建案例

   _如果您是多个Adobe组织的成员，请确保在创建案例之前，在Adobe组织切换程序中选择了管道出现故障的Adobe组织。_

## 部署到

“部署到”步骤负责获取生成映像中生成的代码伪像，使用它开始新的AEM作者和发布服务，并在成功后删除任何旧的AEM作者和发布服务。 此步骤中还安装和更新可变内容包和索引。

在调试部署到步骤之前，熟悉[AEM作为Cloud Service日志](./logs.md)。 `aemerror`日志包含有关开始启动和关闭窗格的信息，这些信息可能与部署到问题相关。 请注意，通过Cloud Manager的“部署到”步骤中的“下载日志”按钮提供的日志不是`aemerror`日志，不包含与开始应用程序相关的详细信息。

![部署到](./assets/build-and-deployment/deploy-to.png)

部署到步骤可能失败的三个主要原因：

### Cloud Manager管道包含旧的AEM版本

+ __原因：__ Cloud Manager管道包含的AEM版本比部署到目标环境的版本旧。如果管道被重新使用并指向运行更高版本AEM的新环境，则可能会发生这种情况。 可通过检查环境的AEM版本是否大于管道的AEM版本来识别此问题。
   ![Cloud Manager管道包含旧的AEM版本](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __分辨率:__
   + 如果目标环境有可用的更新，请从环境的操作中选择更新，然后重新运行内部版本。
   + 如果目标环境没有可用更新，则表示其运行的是最新版本的AEM。 要解决此问题，请删除管线并重新创建它。


### Cloud Manager超时

在开始新部署的AEM服务期间运行的代码耗时太长，以至于Cloud Manager在部署完成之前超时。 在这些情况下，部署可能最终成功，甚至认为Cloud Manager状态报告为“失败”。

+ __原因：自__ 定义代码可能会执行在OSGi捆绑或组件生命周期早期触发的大查询或内容遍历等操作，这会显着推迟AEM的开始启动时间。
+ __解决方__ 案：查看在OSGi Bundle生命周期早期运行的代码的实施，查看AEM `aemerror` 作者和发布服务的日志（以GMT表示的日志时间），如云管理器所示，并查找指示任何自定义日志运行进程的日志消息。

### 代码或配置不兼容

大多数代码和配置违规在生成中的较早阶段被捕获，但自定义代码或配置可能与AEM作为Cloud Service不兼容，并且直到在容器中执行时才被检测到。

+ __原因：自__ 定义代码可能会调用在OSGi捆绑包或组件生命周期中早期触发的长时间操作，如大查询或内容遍历，这会显着推迟AEM的开始。
+ __解决方__ 案： `aemerror` 查看AEM作者服务和发布服务的日志，时间大约在（以格林尼治标准时间）失败，如云管理器所示。
   1. 查看自定义应用程序提供的Java类引发的任何ERRORS的日志。 如果发现任何问题，请解决，推送修复的代码，然后重新构建管道。
   1. 查看您在自定义应用程序中扩展/与之交互的AEM的各个方面所报告的任何ERRORS的日志，并调查这些错误；这些ERROR不能直接归因于Java类。 如果发现任何问题，请解决，推送修复的代码，然后重新构建管道。

### 在内容包中包括/var

`/var` 是可变的，包含各种临时的运行时内容。在内容包中包括`/var`(例如 `ui.content`)可能导致部署步骤失败。

此问题难以识别，因为它不会导致初始部署失败，只会导致后续部署失败。 明显的症状包括：

+ 初始部署成功，但是，作为部署的一部分的新内容或已更改的可变内容似乎不存在于AEM发布服务中。
+ 激活/取消激活AEM作者中的内容被阻止
+ 后续部署在“部署到步骤”中失败，“部署到步骤”在大约60分钟后失败。

验证此问题是失败行为的原因：

1. 确定至少一个内容包是部署的一部分，写入`/var`。
1. 验证主（粗体）分发队列是否在以下位置被阻止：
   + AEM作者>工具>部署>分发
      ![阻止的分发队列](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 如果后续部署失败，请使用“下载日志”按钮下载Cloud Manager的“部署到”日志：

   ![下载部署到日志](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ...并验证log语句之间大约有60分钟：

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... 和 ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   请注意，此日志不会在报告为成功的初始部署中包含这些指示器，而只包含在后续失败的部署中。

+ __原因：__ 用于将内容包部署到AEM发布服务的AEM复制服务用户无法在AEM发 `/var` 布时写入。这会导致将内容包部署到AEM发布服务失败。
+ __解决方__ 案：以下解决此问题的方法按优先顺序列出：
   1. 如果不需要`/var`资源，请从作为应用程序一部分部署的内容包中删除`/var`下的任何资源。
   2. 如果需要`/var`资源，请使用[ repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)定义节点结构。 通过OSGi运行模式，可将重新指向脚本指向AEM作者、AEM发布或两者。
   3. 如果`/var`资源仅对AEM作者是必需的，并且无法使用[repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)合理建模，请将它们移到离散内容包，该内容包仅由[embedding](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds)安装在AEM作者运行模式文件夹(`<target>/apps/example-packages/content/install.author</target>`)中的`all`包中的AEM作者上。
   4. 按照本[AdobeKB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html)中所述，为`sling-distribution-importer`服务用户提供适当的ACL。

### 创建Adobe支持案例

如果上述疑难解答方法未解决问题，请通过以下方式创建Adobe支持案例：

+ [Adobe Admin Console](https://adminconsole.adobe.com) >支持选项卡>创建案例

   _如果您是多个Adobe组织的成员，请确保在创建案例之前，在Adobe组织切换程序中选择了管道出现故障的Adobe组织。_
