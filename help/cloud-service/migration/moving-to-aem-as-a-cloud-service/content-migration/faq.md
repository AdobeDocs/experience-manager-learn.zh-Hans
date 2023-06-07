---
title: AEMas a Cloud Service内容迁移常见问题解答
description: 获取有关将内容迁移到AEMas a Cloud Service的常见问题解答。
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
source-git-commit: 678ecb99b1e63b9db6c9668adee774f33b2eefab
workflow-type: tm+mt
source-wordcount: '2296'
ht-degree: 0%

---

# AEMas a Cloud Service内容迁移常见问题解答

获取有关将内容迁移到AEMas a Cloud Service的常见问题解答。

## 术语

+ **AEMaaCS**： [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**： [最佳实践分析器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**： [内容传输工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **凸轮**： [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**： [Identity Management System](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**： [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

请在创建与CTT相关的Adobe支持票证时，使用以下模板提供更多详细信息。

![内容迁移Adobe支持票证模板](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## 一般内容迁移问题

### 问：将内容作为Cloud Services迁移到AEM的方法有哪些？

有三种不同的方法可用

+ 使用内容传输工具(AEM 6.3+ → AEMaaCS)
+ 通过包管理器(AEM→AEMaaCS)
+ 开箱即用的资产(S3/Azure→AEMaaCS)批量导入服务

### 问：可以使用CTT传输的内容数量是否有限制？

否. CTT作为一种工具可以从AEM源中提取并摄取到AEMaaCS中。 但是，在迁移之前应考虑对AEMaaCS平台的特定限制。

有关更多信息，请参阅 [云迁移先决条件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### 问：我已从源系统中获得最新的BPA报告，应该如何处理？

将报表导出为CSV，然后将其上传到Cloud Acceleration Manager， [与您的IMS组织关联](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html). 然后作为审阅流程进行审核 [在就绪阶段中概述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

请查看工具提供的代码和内容复杂性评估，并记下导致代码重构积压或云迁移评估的相关操作项。

### 问：是否建议对源作者进行提取，并摄取到AEMaaCS作者和发布？

始终建议在创作层和发布层之间执行1:1提取和摄取。 也就是说，可以提取源生产作者并将其引入开发、暂存和生产CS中。

### 问：是否有办法估算使用CTT将内容从源AEM迁移到AEMaaCS所需的时间？

由于迁移过程取决于Internet带宽、分配给CTT进程的栈、可用的可用内存以及磁盘IO（对每个源系统都是主观的），因此建议尽早执行Proof Of迁移，并推断数据点得出估计值。

### 问：如果我启动CTT提取流程，源AEM性能会受到什么影响？

CTT工具在其自身的Java™进程中运行，该进程最多需要4gb栈，可通过OSGi配置进行配置。 此数字可能会发生变化，但您可以搜寻Java™进程并找出原因。

如果安装了AZCopy和/或启用了预复制选项/验证功能，则AZCopy进程会占用CPU周期。

除了jvm之外，该工具还使用磁盘IO将数据存储在过渡临时空间中，并在提取周期后清理这些数据。 除了RAM、CPU和磁盘IO之外，CTT工具还使用源系统的网络带宽将数据上载到Azure blob存储区。

CTT提取过程所用的资源量取决于节点数、Blob数及其聚合大小。 很难提供公式，因此建议执行小规模迁移证明以确定源服务器升级要求。

如果使用克隆环境进行迁移，则不会影响实时生产服务器资源利用率，但在实时生产与克隆之间同步内容方面有其自身的缺点

### 问：在我的源创作系统中，我们为用户配置了SSO，以便他们在创作实例中进行身份验证。 在这种情况下，我是否需要使用CTT的用户映射功能？

简短的答案是“**是**“。

CTT提取和摄取 **不含** 用户映射仅将内容、关联的原则（用户、组）从源AEM迁移到AEMaaCS。 但是，Adobe IMS中要求这些用户（身份）具有（设置的）AEMaaCS实例的访问权限，才能成功进行身份验证。 工作 [用户映射工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/legacy-user-mapping-tool/overview-user-mapping-tool-legacy.html) 是将本地AEM用户映射到IMS用户，以便身份验证和授权一起工作。

在这种情况下，SAML身份提供程序将针对Adobe IMS配置为使用联合/Enterprise ID，而不是使用身份验证处理程序直接配置给AEM。

### 问：在我的源创作系统中，我们为用户配置了基本身份验证，以便他们通过本地AEM用户进入创作实例进行身份验证。 在这种情况下，我是否需要使用CTT的用户映射功能？

简短的答案是“**是**“。

没有用户映射的CTT提取和摄取确实将内容、相关原则（用户、组）从源AEM迁移到AEMaaCS。 但是，Adobe IMS中要求这些用户（身份）具有（设置的）AEMaaCS实例的访问权限，才能成功进行身份验证。 工作 [用户映射工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/legacy-user-mapping-tool/overview-user-mapping-tool-legacy.html) 是将本地AEM用户映射到IMS用户，以便身份验证和授权一起工作。

在这种情况下，用户使用个人Adobe ID，IMS管理员使用Adobe ID提供对AEMaaCS的访问权限。

### 问：在CTT的上下文中，“划出”和“覆盖”这两个术语表示什么？

在上下文中 [萃取阶段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=en#extraction-setup-phase)，选项包括覆盖暂存容器中以前提取周期的数据，或将差异（添加/更新/删除）添加到其中。 暂存容器只是一个与迁移集关联的blob存储容器。 每个迁移集都有各自的暂存容器。

在上下文中 [摄取阶段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html)，选项为+以替换AEMaaCS的整个内容存储库，或从暂存迁移容器同步差异（添加/更新/删除）内容。

### 问：源系统中有多个网站、关联的资产、用户和组。 是否可以分阶段将它们迁移到AEMaaCS？

是的，这是可能的，但需要谨慎规划：

+ 创建迁移集时假定站点、资产位于其各自的层次结构中
   + 验证是否可以接受在一个迁移集中迁移所有资产，然后分阶段迁移正在使用这些资产的站点
+ 在当前状态，创作摄取过程使创作实例不可用于内容创作，即使发布层仍可以提供内容
   + 这意味着在摄取完成到创作中的之前，内容创作活动会被冻结

在规划迁移之前，请查看增补提取和摄取流程（如文档中所述）。

### 问：即使AEMaaCS创作或发布实例中发生了引入，我的网站是否仍可供最终用户使用？

是。内容迁移活动不会中断最终用户流量。 但是，创作引入会冻结内容创作，直到它完成。

### 问：BPA报表会显示与缺少原始演绎版相关的项目。 是否应在提取之前在源上清理它们？

是。缺少原始演绎版意味着资源二进制文件最初未正确上传。 认为数据不正确，请检查，使用包管理器进行备份（如果需要），并在运行提取之前从源AEM中删除这些数据。 坏数据将对资产处理步骤产生负面结果。

### 问：BPA报表包含与缺失相关的项目 `jcr:content` 文件夹节点。 我该拿他们怎么办？

时间 `jcr:content` 在文件夹级别缺失，缺少传播设置的任何操作，例如处理配置文件等。 来自父级的分手将在此级别中断。 请查看缺失的原因 `jcr:content`. 虽然这些文件夹可以迁移，但请注意，此类文件夹会降低用户体验，并会在以后导致不必要的故障排除周期。

### 问：我已经创建了一个迁移集。 是否可以检查其大小？

是的，有一个 [检查大小](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) 属于CTT一部分的特征。

### 问：我正在执行迁移（提取、摄取）。 是否可以验证我提取的所有内容是否已摄取到Target？

是的，有一个 [验证](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) 属于CTT的特征。

### 问：我的客户要求在AEMaaCS环境之间移动内容，例如从AEMaaCS Dev移动到AEMaaCS Stage或移动到AEMaaCS Prod。 我是否可以为这些用例使用内容传输工具？

很遗憾，不能。 CTT的使用案例是将内容从本地/AMS托管的AEM 6.3+源迁移到AEMaaCS云环境。 [请阅读CTT文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### 问：在提取期间预计会出现哪些问题？

提取阶段是一个涉及的过程，需要多个方面才能按预期工作。 了解可能会发生的各种问题以及如何缓解这些问题，可提高内容迁移的整体成功率。

公共文档根据学到的知识不断得到改进，但这里有一些高级别的问题类别和可能的潜在原因。

![AEMas a Cloud Service内容迁移提取问题](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### 问：在摄取期间可能会出现哪些问题？

摄取阶段完全发生在云平台中，需要有权访问AEMaaCS基础架构的资源提供帮助。 请创建支持工单以获取更多帮助。

以下是可能的问题类别（请勿将此视为排他性列表）

![AEMas a Cloud Service内容迁移引入问题](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### 问：我的源服务器是否需要通过出站Internet连接才能使CTT正常工作？

简短的答案是“**是**“。

CTT流程需要连接到以下资源：

+ 目标AEMas a Cloud Service环境： `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure Blob存储服务： `casstorageprod.blob.core.windows.net`
+ 用户映射IO端点： `usermanagement.adobe.io`

请参阅文档以了解有关 [源连接](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## 资产处理Dynamic Media相关问题

### 问：在AEMaaCS中摄取资产后，是否会自动重新处理资产？

否. 要处理资产，必须启动重新处理的请求。

### 问：资产在AEMaaCS中摄取后是否会自动重新索引？

是。系统会根据AEMaaCS上可用的索引定义，将资产重新编入索引。

### 问：源AEM与Dynamic Media集成。 在迁移内容之前是否必须考虑任何特定事项？

是，当源AEM具有Dynamic Media集成时，请考虑以下事项。

+ AEMaaCS仅支持Dynamic Media Scene7模式。 如果源系统处于混合模式，则需要将DM迁移到Scene7模式。
+ 如果方法是从源克隆实例进行迁移，则可以在用于CTT的克隆上禁用DM集成。 此步骤纯粹是为了避免向DM进行任何写入或避免DM流量加载。
+ 请注意，CTT会将节点、迁移集的元数据从源AEM迁移到AEMaaCS。 它不会直接对DM执行任何操作。

### 问：当源AEM上存在DM集成时，有哪些不同的迁移方法？

请先阅读上述问题和答案

（这是两种可能的选择，但不仅限于这两种）。 这取决于客户希望如何进行UAT 、性能测试、可用的环境以及是否使用克隆进行迁移。 请将这两者作为讨论的起点

**选项1**

如果源环境中的资产/节点数处于低端（约100K），假设这些资产或节点可以经过24 + 72小时的迁移（包括提取和摄取），则更好的方法为

+ 直接从生产环境执行迁移
+ 使用以下方式运行初始提取并摄取到AEMaaCS `wipe=true`
   + 此步骤会迁移所有节点和二进制文件
+ 继续使用内部部署/AMS Prod作者
+ 从现在开始，使用运行所有其他迁移周期证明 `wipe=true`
   + 请注意，此操作会迁移完整的节点存储，但只迁移修改的Blob而不是整个Blob。 上一组Blob位于目标AEMaaCS实例的Azure Blob存储中。
   + 使用此迁移证明来衡量迁移持续时间、用户映射、测试和所有其他功能的验证
+ 最后，在上线一周之前，执行划出=true迁移
   + 在AEMaaCS上连接Dynamic Media
   + 从AEM内部部署源断开DM配置

利用此选项，您可以运行一对一迁移，即内部部署→AEMaaCS开发等。 并从各自的环境中移动DM配置

（如果计划从克隆执行迁移）

**选项2**

+ 创建生产作者的克隆，从克隆中删除DM配置
+ 将内部部署克隆迁移→AEMaaCS开发/暂存
   + 将生产DM公司简要连接到AEMaaCS开发/暂存以进行验证
   + 在DM连接处于活动状态期间，避免将资产引入AEMaaCS
   + 这使他们能够验证CTT、DM特定的验证
+ 在AEMaaCS上完成测试后
   + 运行从内部部署阶段到AEMaaCS阶段的划出迁移

运行从内部部署开发到AEMaaCS开发的划出迁移。

上述方法可用于仅测量迁移持续时间，但需要稍后进行清理。

## 其他资源

+ [迁移到云中Experience Manager的提示和技巧( Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT专家系列视频](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [其他AEMaaCS主题的专家系列视频](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html)
