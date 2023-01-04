---
user-guide-title: Adobe Experience Manager as a Cloud Service 教程
user-guide-description: Adobe Experience Manager as a Cloud Service 的教程集合。
breadcrumb-title: AEM as a Cloud Service 教程
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: 8b683fdcea05859151b929389f7673075c359141
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 20%

---


# Adobe Experience Manager as a Cloud Service 教程 {#cloud-service}

+ [概述](./overview.md)
+ AEM as a Cloud Service 简介{#introduction}
   + [什么是AEMas a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [进化](./introduction/evolution.md)
   + [架构](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 战略与思想领导{#strategy}
      + [Experience Manager — 治理和人员配置模式和原型](./introduction/experience-manager-governance-and-staffing-models.md)
      + [如何使用Adobe Experience Manager提高内容周转率](./introduction/drive-content-velocity-for-sites.md)
      + [使用AEM样式系统加快内容速度](./introduction/accelerate-content-velocity-aem.md)
+ [Experience Cloud 集成](./experience-cloud/integrations.md)
+ 底层技术 {#underlying-technology}
   + [AEM架构](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java内容存储库](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [创作和发布服务](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [项目](./cloud-manager/programs.md)
   + [环境](./cloud-manager/environments.md)
   + [CI/CD生产管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD非生产管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活动](./cloud-manager/activity.md)
   + 开发运营{#devops}
      + [部署代码](./cloud-manager/devops/deploy-code.md)
      + [合并项目](./cloud-manager/devops/merge-projects.md)
      + [配置管道](./cloud-manager/devops/configure-pipelines.md)
      + [持续集成](./cloud-manager/devops/continuous-integration.md)
      + [分析测试结果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 配置](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 本地开发环境设置 {#local-development-environment-set-up}
   + [概述](./local-development-environment/overview.md)
   + [开发工具](./local-development-environment/development-tools.md)
   + [本地AEM运行时](./local-development-environment/aem-runtime.md)
   + [本地Dispatcher工具](./local-development-environment/dispatcher-tools.md)
+ 开发{#developing}
   + 可扩展性{#extensibility}
      + 内容片段控制台{#content-fragments}
         + [概述](./developing/extensibility/content-fragments/overview.md)
         + [扩展注册](./developing/extensibility/content-fragments/extension-registration.md)
         + [标题菜单](./developing/extensibility/content-fragments/header-menu.md)
         + [操作栏](./developing/extensibility/content-fragments/action-bar.md)
         + [模态](./developing/extensibility/content-fragments/modal.md)
         + [Adobe I/O Runtime行动](./developing/extensibility/content-fragments/runtime-action.md)
         + [测试](./developing/extensibility/content-fragments/test.md)
         + [部署](./developing/extensibility/content-fragments/deploy.md)
         + 扩展示例{#example-extensions}
            + [批量属性更新扩展](./developing/extensibility/content-fragments/example-extensions/bulk-property-update.md)
            + [生成图像并上传到AEM](./developing/extensibility/content-fragments/example-extensions/image-generation-and-image-upload.md)
   + 开发基础知识{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本地开发环境](./developing/basics/local-development-environment.md)
      + [AEM 项目原型](./developing/basics/aem-project-archetype.md)
      + [AEM 项目结构](./developing/basics/project-structure.md)
      + [可变内容与不可变内容](./developing/basics/mutable-immutable.md)
      + [存储库结构包](./developing/basics/repository-structure-package.md)
      + [内容发布](./developing/basics/content-publishing.md)
      + [OSGi配置](./developing/basics/osgi-configurations.md)
      + [调度程序配置迁移](./developing/basics/dispatcher-configuration.md)
   + AEM 项目{#aem-projects}
      + [AEM Maven项目](./developing/projects/maven-project-structure.md)
      + [清除AEM Maven项目](./developing/projects/remove-samples.md)
   + OSGi服务{#osgi-services}
      + [OSGi服务基础知识](./developing/osgi-services/basics.md)
      + [OSGi组件生命周期](./developing/osgi-services/lifecycle.md)
      + [OSGi配置基础知识](./developing/osgi-services/configurations.md)
      + [使用OCD的OSGi配置](./developing/osgi-services/configurations-ocd.md)
   + 高级{#advanced}
      + [服务用户](./developing/advanced/service-users.md)
      + [自定义命名空间](./developing/advanced/custom-namespaces.md)
      + [缓存页面变体](./developing/advanced/variant-caching.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ 调试AEM{#debugging}
   + 调试AEM SDK{#debugging-aem-sdk}
      + [概述](./debugging/aem-sdk-local-quickstart/overview.md)
      + [日志](./debugging/aem-sdk-local-quickstart/logs.md)
      + [远程调试](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 调试AEMas a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概述](./debugging/cloud-service/overview.md)
      + [日志](./debugging/cloud-service/logs.md)
      + [构建和部署](./debugging/cloud-service/build-and-deployment.md)
      + [开发人员控制台](./debugging/cloud-service/developer-console.md)
      + [存储库浏览器](./debugging/cloud-service/repository-browser.md)
      + 风险{#risks}
         + [几个警告](./debugging/cloud-service/risks/traversals.md)
+ 内容交付{#content-delivery}
   + [URL重定向](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html)
+ 访问AEM{#accessing}
   + [概述](./accessing/overview.md)
   + [Adobe IMS用户](./accessing/adobe-ims-users.md)
   + [Adobe IMS用户组](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS产品配置文件](./accessing/adobe-ims-product-profiles.md)
   + [AEM用户、组和权限](./accessing/aem-users-groups-and-permissions.md)
   + [配置对AEM演练的访问权限](./accessing/walk-through.md)
+ 身份验证{#authentication}
   + [概述](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 高级网络{#networking}
   + [概述](./networking/advanced-networking.md)
   + [灵活的端口出口](./networking/flexible-port-egress.md)
   + [专用出口IP地址](./networking/dedicated-egress-ip-address.md)
   + [虚拟专用网](./networking/vpn.md)
   + 代码示例{#examples}
      + [在非标准端口上使用HTTP/HTTPS以实现灵活的端口出口](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [用于专用出口IP地址/VPN的HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [使用DataSourcePool的SQL连接](./networking/examples/sql-datasourcepool.md)
      + [使用Java SQL API的SQL连接](./networking/examples/sql-java-apis.md)
      + [电子邮件服务](./networking/examples/email-service.md)
+ 迁移 {#migration}
   + [内容转移工具](./migration/content-transfer-tool.md)
   + [批量导入资产](./migration/bulk-import.md)
   + 移动到 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [简介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [新用户引导](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [双酚A和CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM现代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存储库现代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute微服务](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜索和索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 内容迁移 {#content-migration}
         + [批量导入服务](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [内容转移工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [常见问题](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [疑难解答](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Formsas a Cloud Service {#aem-forms}
         + [简介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [数字注册](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通信](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [简介](./migration/cloud-acceleration-manager/introduction.md)
      + [就绪性和最佳实践分析器](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [实施阶段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [内容转移工具](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [代码重构工具](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [代码存储库Modernizer](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引转换器](./migration/cloud-acceleration-manager/index-converter.md)
      + [资源工作流迁移工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [导航Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}
   + 为Formsas a Cloud Service开发{#developing-for-cloud-service}
      + [入门](./forms/developing-for-cloud-service/getting-started.md)
      + [安装IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [设置Git](./forms/developing-for-cloud-service/setup-git.md)
      + [将IntelliJ与AEM同步](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [构建表单](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [启用Forms Portal组件](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [包括Cloud Services和FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [上下文感知云配置](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [推送到Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [部署到开发环境](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [更新maven原型](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + 创建自适应表单{#create-first-af}
      + [简介](./forms/create-first-af/introduction.md)
      + [创建名称](./forms/create-first-af/create-theme.md)
      + [创建模板](./forms/create-first-af/create-template.md)
      + [创建片段](./forms/create-first-af/create-fragments.md)
      + [创建表单](./forms/create-first-af/create-af.md)
      + [配置根面板](./forms/create-first-af/configure-root-panel.md)
      + [“配置人员”面板](./forms/create-first-af/configure-people-panel.md)
      + [配置收入面板](./forms/create-first-af/configure-income-panel.md)
      + [配置资产面板](./forms/create-first-af/configure-assets-panel.md)
      + [配置开始面板](./forms/create-first-af/configure-start-panel.md)
      + [添加和配置工具栏](./forms/create-first-af/add-configure-toolbar.md)
   + AEM Forms CS中的文档生成{#doc-gen-formscs}
      + [简介](./forms/doc-gen-forms-cs/introduction.md)
      + [创建服务凭据](./forms/doc-gen-forms-cs/service-credentials.md)
      + [创建JWT令牌](./forms/doc-gen-forms-cs/create-jwt.md)
      + [创建访问令牌](./forms/doc-gen-forms-cs/create-access-token.md)
      + [将数据与模板合并](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [测试解决方案](./forms/doc-gen-forms-cs/test.md)
      + [挑战](./forms/doc-gen-forms-cs/challenge.md)
   + 使用批处理API生成文档{#formscs-batch-api}
      + [简介](./forms/formscs-batch-api/introduction.md)
      + [配置Azure存储](./forms/formscs-batch-api/configure-azure-storage.md)
      + [创建USC批量配置](./forms/formscs-batch-api/configure-usc-batch.md)
      + [创建批量配置](./forms/formscs-batch-api/create-batch-config.md)
      + [执行批处理](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Forms CS中的PDF操作{#forms-cs-assembler}
      + [简介](./forms/forms-cs-assembler/introduction.md)
      + [创建服务凭据](./forms/forms-cs-assembler/service-credentials.md)
      + [创建JWT令牌](./forms/forms-cs-assembler/create-jwt.md)
      + [创建访问令牌](./forms/forms-cs-assembler/create-access-token.md)
      + [组合PDF文件](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A实用程序](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [测试解决方案](./forms/forms-cs-assembler/test.md)
      + [挑战](./forms/forms-cs-assembler/challenge.md)
   + Azure门户存储{#forms-cs-azure-portal}
      + [简介](./forms/forms-cs-azure-portal/introduction.md)
      + [创建表单数据模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [在Azure存储中存储表单数据](./forms/forms-cs-azure-portal/create-af.md)
      + [预填表单](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查询提交](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 创建审阅工作流{#create-aem-workflow}
      + [外部化工作流存储](./forms/create-aem-workflow/externalize-workflow.md)
      + [创建工作流模型](./forms/create-aem-workflow/create-workflow.md)
      + [触发工作流](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign与AEM Forms{#forms-and-sign}
      + [简介](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API应用程序](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign云配置](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [创建自适应表单](./forms/forms-and-sign/create-adaptive-form.md)
      + [配置以进行填写和签名](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 与Microsoft Power集成自动化{#forms-cs-and-power-automate}
      + [配置集成](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [解析提交的表单数据](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [将DoR作为电子邮件附件发送](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [从提交的数据中提取表单附件](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + 与Microsoft Dynamics集成{#formscs-dynamics-crm}
      + [创建Dynamics应用程序](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [配置数据源](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [创建表单数据模型](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [创建自适应表单](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + 与Salesforce集成{#integrate-with-salesforce}
      + [简介](./forms/integrate-with-salesforce/introduction.md)
      + [创建连接的应用程序](./forms/integrate-with-salesforce/create-connected-app.md)
      + [创建swagger文件](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [创建数据源](./forms/integrate-with-salesforce/create-data-source.md)
      + [创建表单数据模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [测试表单提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [测试点击事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute可扩展性{#asset-compute}
   + [概述](./asset-compute/overview.md)
   + 设置{#set-up}
      + [帐户和服务配置](./asset-compute/set-up/accounts-and-services.md)
      + [本地开发环境](./asset-compute/set-up/development-environment.md)
      + [应用程序生成器](./asset-compute/set-up/app-builder.md)
   + 开发{#develop}
      + [创建Asset compute项目](./asset-compute/develop/project.md)
      + [配置环境变量](./asset-compute/develop/environment-variables.md)
      + [配置manifest.yml](./asset-compute/develop/manifest.md)
      + [开发工作人员](./asset-compute/develop/worker.md)
      + [使用开发工具](./asset-compute/develop/development-tool.md)
   + 测试和调试{#test-debug}
      + [测试工作人员](./asset-compute/test-debug/test.md)
      + [调试工作程序](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署到Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [与AEM集成](./asset-compute/deploy/processing-profiles.md)
   + 高级{#advanced}
      + [元数据工作程序](./asset-compute/advanced/metadata.md)
   + [疑难解答](./asset-compute/troubleshooting.md)
+ 云5{#cloud-5}
   + [简介](./cloud-5/cloud5-introduction.md)
   + [第1季](./cloud-5/cloud5-season-1.md)
   + [第2季](./cloud-5/cloud5-season-2.md)
   + [AEM CDN第1部分](./cloud-5/cloud5-aem-cdn-part1.md)
   + [AEM CDN第2部分](./cloud-5/cloud5-aem-cdn-part2.md)
   + [AEM日志文件](./cloud-5/cloud5-aem-log-files.md)
   + [登录令牌](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [Cloud Dispatcher](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [迁移1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [迁移2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Dispatcher验证器](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [搜索和索引](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobe应用程序生成器](./cloud-5/cloud5-adobe-app-builder.md)
   + 第2季{#season-2}
      + [片段](./cloud-5/season-2/cloud5-experience-v-content-fragments.md)
      + [Repo Modernizer](./cloud-5/season-2/cloud5-repo-modernizer.md)
      + [Admin Console](./cloud-5/season-2/cloud5-admin-console.md)
      + [重新指点](./cloud-5/season-2/cloud5-repoinit.md)
      + [Sling作业计划程序](./cloud-5/season-2/cloud5-sling-job-scheduler.md)
      + [修复缓存](./cloud-5/season-2/cloud5-fix-your-cache.md)
      + [修复了重写](./cloud-5/season-2/cloud5-fix-your-rewrites.md)
      + [Cloud Manager — 体验审核](./cloud-5/season-2/cloud5-mocm-experience-audit.md)
      + [Cloud Manager — 单元测试](./cloud-5/season-2/cloud5-mocm-unit-tests.md)
      + [Cloud Manager — 功能测试](./cloud-5/season-2/cloud5-mocm-functional-tests.md)
+ [AEM专家系列](./aem-experts-series.md)
+ 多步Tutorials{#multi-step-tutorials}
   + [AEM Sites开发](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA Editor(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
