---
title: AEMas a Cloud Service内容迁移常见问题解答
description: 获取有关内容迁移到AEM as a Cloud Service的常见问题解答。
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
source-git-commit: b2656329270ac90458dbc25bb05f39bf76921f26
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 0%

---


# AEMas a Cloud Service内容迁移常见问题解答

获取有关内容迁移到AEM as a Cloud Service的常见问题解答。

## 术语

+ **AEMaCS**: [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [Best Practices Analyzer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [内容传输工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Identity Management系统](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

请在创建与CTT相关的Adobe支持票证时，使用以下模板提供更多详细信息。

![内容迁移Adobe支持票证模板](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## 一般内容迁移问题

### 问：将内容作为Cloud Services迁移到AEM的不同方法是什么？

有三种不同的方法可用

+ 使用内容传输工具(AEM 6.3+ → AEMaCS)
+ 通过包管理器(AEM → AEMaCS)
+ 资产的批量导入服务(S3/Azure → AEMaCS)

### 问：使用CTT可以传输的内容量是否存在限制？

否. CTT作为工具可以从AEM源提取并摄取到AEMaCS中。 但是，AEMaaCS平台存在一些特定的限制，在迁移之前应考虑这些限制。

有关更多信息，请参阅 [云迁移先决条件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### 问：我从源系统获得了最新的BPA报告，我该如何处理它？

将报表导出为CSV，然后将其上传到Cloud Acceleration Manager， [与您的IMS组织关联](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). 然后，按照 [准备阶段中概述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

请查看该工具提供的代码和内容复杂性评估，并记下导致代码重构积压或云迁移评估的相关操作项。

### 问：是否建议在源作者时提取并摄取到AEMaaCS作者和发布中？

始终建议在创作层和发布层之间执行1:1提取和摄取。 也就是说，可以提取源生产作者并将其摄取到开发、暂存和生产CS中。

### 问：是否可以使用CTT估算内容从源AEM迁移到AEMaCS所花费的时间？

由于迁移过程取决于Internet带宽、为CTT进程分配的堆、可用空闲内存和磁盘IO，这对每个源系统都是主观的，因此建议在上提前执行迁移证明并推断数据点以得出估计值。

### 问：如果启动CTT提取流程，我的源AEM性能会受到什么影响？

CTT工具在其自己的Java™进程中运行，该进程最多占用4gb堆，可通过OSGi配置进行配置。 此数字可能会更改，但您可以为Java™进程寻找答案。

如果安装了AZCopy和/或启用了预复制选项/验证功能，则AZCopy进程会消耗CPU周期。

除jvm之外，该工具还使用磁盘IO在过渡临时空间上存储数据，该数据将在提取周期后清理。 除了RAM、CPU和磁盘IO之外，CTT工具还使用源系统的网络带宽将数据上传到Azure Blob Store。

CTT提取过程所占用的资源量取决于节点数、Blob数量及其聚合大小。 很难提供公式，因此建议执行小的迁移校样以确定源服务器的升级要求。

如果将克隆环境用于迁移，则它不会影响实时生产服务器资源利用率，但在实时生产和克隆之间同步内容方面有其自身的缺点

### 问：在我的源创作系统中，我们为用户配置了SSO，以验证到创作实例中。 在这种情况下，我是否必须使用CTT的用户映射功能？

简单的答案是“**是**&quot;

CTT提取和摄取 **无** 用户映射仅将内容、相关原则（用户、组）从源AEM迁移到AEMaCS。 但是，Adobe IMS中存在这些用户（身份），并且需要拥有（配置）AEMaCS实例的访问权限才能成功进行身份验证。 的工作 [用户映射工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 是将本地AEM用户映射到IMS用户，以便身份验证和授权能够协同工作。

在这种情况下，针对Adobe IMS将SAML身份提供程序配置为使用联合/Enterprise ID，而不是使用身份验证处理程序直接发送到AEM。

### 问：在我的源创作系统中，我们为用户配置了基本身份验证，以通过本地AEM用户验证到创作实例中。 在这种情况下，我是否必须使用CTT的用户映射功能？

简单的答案是“**是**&quot;

无需用户映射的CTT提取和摄取会将内容、关联的原则（用户、组）从源AEM迁移到AEMaCS。 但是，Adobe IMS中存在这些用户（身份），并且需要拥有（配置）AEMaCS实例的访问权限才能成功进行身份验证。 的工作 [用户映射工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 是将本地AEM用户映射到IMS用户，以便身份验证和授权能够协同工作。

在这种情况下，用户使用个人Adobe ID，而IMS管理员使用Adobe ID来提供对AEMaCS的访问权限。

### 问：术语“划出”和“覆盖”在CTT上下文中表示什么？

在 [提取相](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html)，则选项可用于从以前的提取周期覆盖暂存容器中的数据，或向其中添加差异（已添加/已更新/已删除）。 暂存容器不是任何内容，而是与迁移集关联的blob存储容器。 每个迁移集都有其自己的暂存容器。

在 [摄取阶段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html)，则选项为+ ，用于替换AEMaaCS的整个内容存储库，或从暂存迁移容器同步差异（已添加/已更新/已删除）内容。

### 问：源系统中有多个网站、关联的资产、用户和组。 是否可以分阶段将它们迁移到AEMaCS?

是的，这是可能的，但需要谨慎规划：

+ 创建迁移集（假设站点）时，资产将进入其各自的层级
   + 验证是否可以将所有资产作为一个迁移集的一部分进行迁移，然后分阶段引入正在使用这些资产的站点
+ 在当前状态下，创作摄取过程会使创作实例不可用于内容创作，即使发布层仍可以提供内容
   + 这意味着在摄取完成并进入创作阶段之前，内容创作活动将冻结

在规划迁移之前，请查看所述的增补提取和摄取流程。

### 问：即使在AEMaaCS创作实例或发布实例中发生摄取，我的网站是否仍可供最终用户使用？

是。内容迁移活动不会中断最终用户流量。 但是，创作摄取会冻结内容创作，直到完成为止。

### 问：BPA报表显示与缺少原始演绎版相关的项目。 在提取前，我应该先在源上清理吗？

是。缺少的原始演绎版意味着资产二进制文件最初无法正确上传。 将其视为坏数据，请查看使用包管理器进行备份（视需要），并在运行提取之前从源AEM中删除它们。 错误数据会在资产处理步骤中产生负结果。

### 问：BPA报表包含与缺失相关的项目 `jcr:content` 文件夹的节点。 我该怎么处理它们？

When `jcr:content` 文件夹级别中缺少、用于传播设置（如处理配置文件等）的任何操作。 父母会在这个级别上分手。 请查看丢失的原因 `jcr:content`. 即使这些文件夹可以迁移，请注意，此类文件夹会降低用户体验，并导致以后出现不必要的故障排除周期。

### 问：我已创建迁移集。 是否可以检查其大小？

是的，有 [检查大小](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) 属于CTT的功能。

### 问：我正在执行迁移（提取、摄取）。 是否可以验证我提取的所有内容是否都已摄取到目标中？

是的，有 [验证](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) 属于CTT的功能。

### 问：我的客户需要在AEMaaCS环境（如从AEMaaCS Dev到AEMaaCS Stage或到AEMaCS Prod）之间移动内容。 我能否将这些用例使用内容传输工具？

很遗憾，不。 CTT的用例是将内容从本地/AMS托管的AEM 6.3+源迁移到AEMaCS云环境。 [请阅读CTT文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### 问：在采掘过程中会出现哪些问题？

提取阶段是一个涉及的流程，需要从多个方面按预期工作。 了解可能发生的不同类型的问题以及如何缓解这些问题将提高内容迁移的整体成功率。

公共文档不断根据学习情况得到改进，但以下是一些高级别问题类别和可能的根本原因。

![AEMas a Cloud Service内容迁移提取问题](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### 问：在摄取过程中会出现哪些问题？

摄取阶段完全在云平台中进行，需要有权访问AEMaaCS基础架构的资源的帮助。 请创建支持票证以获取更多帮助。

以下是可能的问题类别（请不要将此视为排他列表）

![AEMas a Cloud Service内容迁移摄取问题](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### 问：我的源服务器是否需要具有出站Internet连接才能使CTT正常工作？

简单的答案是“**是**&quot;

CTT流程需要连接到以下资源：

+ 目标AEMas a Cloud Service环境： `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure Blob存储服务： `casstorageprod.blob.core.windows.net`
+ 用户映射IO端点： `usermanagement.adobe.io`

有关 [源连接](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## 资产处理Dynamic Media相关问题

### 问：在AEMaCS中摄取后，是否会自动重新处理资产？

否. 要处理资产，必须启动重新处理请求。

### 问：在AEMaCS中摄取后，是否会自动将资产重新编入索引？

是。资产会根据AEMaaCS中提供的索引定义重新编入索引。

### 问：源AEM与Dynamic Media集成。 内容迁移之前是否必须考虑任何特定事项？

是，当源AEM具有Dynamic Media集成时，请考虑以下事项。

+ AEMaCS仅支持Dynamic Media Scene7模式。 如果源系统处于混合模式，则需要将DM迁移到Scene7模式。
+ 如果方法是从源克隆实例迁移，则可以在克隆上禁用将用于CTT的DM集成。 此步骤纯粹是为了避免向DM写入任何内容，或避免DM流量上的负载。
+ 请注意，CTT将节点、从源AEM到AEMaCS的迁移集的元数据迁移到CTT。 它不会直接对DM执行任何操作。

### 问：当源AEM上存在DM集成时，有哪些不同的迁移方法？

请先阅读上述问题并回答

（这是两个可能的选项，但不限于这两个选项）。 它取决于客户希望如何进入UAT、性能测试、可用环境，以及是否正在使用克隆进行迁移。 请把这两个作为讨论的起点

**选项1**

如果源环境中的资产/节点数位于较低端(~100K)，假设这些资产/节点可在包括提取和摄取在内的24 + 72小时内迁移，则更好的方法是

+ 直接从生产环境进行迁移
+ 通过 `wipe=true`
   + 此步骤可迁移所有节点和二进制文件
+ 继续在内部部署/AMS Prod作者中工作
+ 从现在起，使用 `wipe=true`
   + 请注意，此操作会迁移整个节点存储，但只迁移已修改的Blob，而不是整个Blob。 上一组Blob位于目标AEMaCS实例的Azure Blob存储区中。
   + 使用此迁移证明来测量迁移持续时间、用户映射、测试和验证所有其他功能
+ 最后，在上线一周之前，执行划出=true迁移
   + 在AEMaCS上连接Dynamic Media
   + 从AEM本地源断开DM配置

使用此选项，您可以运行一到一的迁移，这意味着On-prem Dev → AEMaCS Dev，等等。 并从相应的环境中移动DM配置

（如果计划从克隆执行迁移）

**选项2**

+ 创建生产作者的克隆，从克隆中删除DM配置
+ 迁移本地克隆→ AEMaCS开发/暂存
   + 出于验证目的，将生产DM公司短暂连接到AEMaCS开发/暂存环境
   + 在DM连接处于活动状态期间，避免将资产摄取到AEMaaCS中
   + 这样，他们就可以验证CTT、DM特定的验证
+ 在AEMaCS上完成测试后
   + 运行从内部部署阶段到AEMaaCS阶段的划出迁移

运行从内部部署Dev到AEMaaCS Dev的划出迁移。

上述方法可用于仅测量迁移持续时间，但需要稍后进行清理。

## 其他资源

+ [在云中迁移到Experience Manager的提示和技巧（峰会，2022年）](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT专家系列视频](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [其他AEMaCS主题的专家系列视频](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
