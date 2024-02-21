---
user-guide-title: Adobe Experience Manager as a Cloud Service 教程
user-guide-description: Adobe Experience Manager as a Cloud Service 的教程集合。
breadcrumb-title: AEM as a Cloud Service 教程
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: da8d584e04b5ef375ef5731ea1d5f9e2c0438b2a
workflow-type: tm+mt
source-wordcount: '1128'
ht-degree: 17%

---


# Adobe Experience Manager as a Cloud Service 教程 {#cloud-service}

+ [概述](./overview.md)
+ AEM试用 {#aem-trials}
   + [图像](./aem-trials/images.md)
+ AEMas a Cloud Service简介{#introduction}
   + [什么是AEMas a Cloud Service？](./introduction/what-is-aem-as-a-cloud-service.md)
   + [架构](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 战略与思想领导力{#strategy}
      + [Experience Manager — 治理和人员配置模型和原型](./introduction/experience-manager-governance-and-staffing-models.md)
      + [如何使用Adobe Experience Manager提高内容速度](./introduction/drive-content-velocity-for-sites.md)
      + [使用AEM样式系统加快内容速度](./introduction/accelerate-content-velocity-aem.md)
+ Experience Cloud集成{#integrations}
   + [集成](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ 底层技术 {#underlying-technology}
   + [AEM架构](./underlying-technology/introduction-architecture.md)
   + [osgi](./underlying-technology/introduction-osgi.md)
   + [Java内容存储库](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [创作和发布服务](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [AEM AssetsSidekick插件](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [项目](./cloud-manager/programs.md)
   + [环境](./cloud-manager/environments.md)
   + [CI/CD 生产管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 非生产管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活动](./cloud-manager/activity.md)
   + 开发操作{#devops}
      + [部署代码](./cloud-manager/devops/deploy-code.md)
      + [合并项目](./cloud-manager/devops/merge-projects.md)
      + [配置管道](./cloud-manager/devops/configure-pipelines.md)
      + [持续集成](./cloud-manager/devops/continuous-integration.md)
      + [分析测试结果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 配置](./cloud-manager/devops/dispatcher-configurations.md)
+ 本地开发环境设置 {#local-development-environment-set-up}
   + [概述](./local-development-environment/overview.md)
   + [开发工具](./local-development-environment/development-tools.md)
   + [本地AEM SDK](./local-development-environment/aem-runtime.md)
   + [本地 Dispatcher 工具](./local-development-environment/dispatcher-tools.md)
+ 开发{#developing}
   + 可扩展性{#extensibility}
      + App Builder{#app-builder}
         + [生成访问令牌](./developing/extensibility/app-builder/jwt-auth.md)
      + UI可扩展性{#ui}
         + [概述](./developing/extensibility/ui/overview.md)
         + [Adobe Developer控制台项目](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [初始化应用程序](./developing/extensibility/ui/app-initialization.md)
         + [注册扩展](./developing/extensibility/ui/extension-registration.md)
         + [模态](./developing/extensibility/ui/modal.md)
         + [Adobe I/O Runtime操作](./developing/extensibility/ui/runtime-action.md)
         + [验证](./developing/extensibility/ui/verify.md)
         + [部署](./developing/extensibility/ui/deploy.md)
         + 内容片段{#content-fragments}
            + [概述](./developing/extensibility/ui/content-fragments/overview.md)
            + 示例{#examples}
               + [AI图像生成](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [批量属性更新](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [自定义网格列](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [导出为XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [RTE工具栏按钮](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [RTE小组件](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [RTE标记](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
   + 开发基础{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本地开发环境](./developing/basics/local-development-environment.md)
      + [AEM 项目原型](./developing/basics/aem-project-archetype.md)
      + [AEM 项目结构](./developing/basics/project-structure.md)
      + [可变内容与不可变内容](./developing/basics/mutable-immutable.md)
      + [存储库结构包](./developing/basics/repository-structure-package.md)
      + [内容发布](./developing/basics/content-publishing.md)
      + [OSGi 配置](./developing/basics/osgi-configurations.md)
      + [Dispatcher配置迁移](./developing/basics/dispatcher-configuration.md)
   + AEM项目{#aem-projects}
      + [AEM Maven项目](./developing/projects/maven-project-structure.md)
      + [清理AEM Maven项目](./developing/projects/remove-samples.md)
   + OSGi服务{#osgi-services}
      + [OSGi服务基础知识](./developing/osgi-services/basics.md)
      + [OSGi组件生命周期](./developing/osgi-services/lifecycle.md)
      + [OSGi配置基础](./developing/osgi-services/configurations.md)
      + [使用OCD的OSGi配置](./developing/osgi-services/configurations-ocd.md)
   + 高级{#advanced}
      + [缓存页面变量](./developing/advanced/variant-caching.md)
      + [CSRF保护](./developing/advanced/csrf-protection.md)
      + [自定义命名空间](./developing/advanced/custom-namespaces.md)
      + [服务用户](./developing/advanced/service-users.md)
      + [Web优化图像API](./developing/advanced/web-optimized-image-delivery-java-apis.md)
   + 快速开发环境{#rde}
      + [概述](./developing/rde/overview.md)
      + [如何设置](./developing/rde/how-to-setup.md)
      + [使用方法](./developing/rde/how-to-use.md)
      + [开发生命周期](./developing/rde/development-life-cycle.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ 调试AEM{#debugging}
   + 调试AEM SDK{#debugging-aem-sdk}
      + [概述](./debugging/aem-sdk-local-quickstart/overview.md)
      + [日志](./debugging/aem-sdk-local-quickstart/logs.md)
      + [远程调试](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 调试AEMas a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概述](./debugging/cloud-service/overview.md)
      + [日志](./debugging/cloud-service/logs.md)
      + [生成和部署](./debugging/cloud-service/build-and-deployment.md)
      + [开发人员控制台](./debugging/cloud-service/developer-console.md)
      + [存储库浏览器](./debugging/cloud-service/repository-browser.md)
      + 风险{#risks}
         + [遍历警告](./debugging/cloud-service/risks/traversals.md)
+ 内容交付{#content-delivery}
   + [URL重定向](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ 缓存{#caching}
   + [概述](./caching/overview.md)
   + [AEM Publish服务](./caching/publish.md)
   + [AEM Author服务](./caching/author.md)
   + [CDN缓存命中率分析](./caching/cdn-cache-hit-ratio-analysis.md)
   + 操作方法{#how-to}
      + [启用缓存](./caching/how-to/enable-caching.md)
      + [禁用缓存](./caching/how-to/disable-caching.md)
+ 访问AEM{#accessing}
   + [概述](./accessing/overview.md)
   + [Adobe IMS 用户](./accessing/adobe-ims-users.md)
   + [Adobe IMS 用户组](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS 产品配置文件](./accessing/adobe-ims-product-profiles.md)
   + [AEM用户、组和权限](./accessing/aem-users-groups-and-permissions.md)
   + [配置对 AEM 演练的访问权限](./accessing/walk-through.md)
+ 身份验证{#authentication}
   + [概述](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 高级联网{#networking}
   + [概述](./networking/advanced-networking.md)
   + [灵活的端口出口](./networking/flexible-port-egress.md)
   + [专用出口 IP 地址](./networking/dedicated-egress-ip-address.md)
   + [虚拟专用网络](./networking/vpn.md)
   + 代码示例{#examples}
      + [非标准端口上的HTTP/HTTPS，实现灵活端口出口](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [专用出口IP地址/VPN的HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [使用DataSourcePool的SQL连接](./networking/examples/sql-datasourcepool.md)
      + [SQL连接使用Java SQL API](./networking/examples/sql-java-apis.md)
      + [电子邮件服务](./networking/examples/email-service.md)
+ 安全性 {#security}
   + 包含WAF规则的流量过滤器规则{#traffic-filter-and-waf-rules}
      + [概述](./security/traffic-filter-rules/overview.md)
      + [如何设置](./security/traffic-filter-rules/how-to-setup.md)
      + [示例和结果分析](./security/traffic-filter-rules/examples-and-analysis.md)
      + [最佳实践](./security/traffic-filter-rules/best-practices.md)
+ AEM事件{#aem-eventing}
   + [概述](./eventing/overview.md)
   + 示例{#examples}
      + [Webhook — 接收AEM活动](./eventing/examples/webhook.md)
      + [日记 — 加载AEM事件](./eventing/examples/journaling.md)
      + [Adobe I/O Runtime操作 — 接收AEM事件](./eventing/examples/runtime-action.md)
      + [Adobe I/O Runtime操作 — 处理AEM事件](./eventing/examples/event-processing-using-runtime-action.md)
      + [AEM Assets事件 — PIM集成](./eventing/examples/assets-pim-integration.md)
+ 迁移 {#migration}
   + [内容传输工具](./migration/content-transfer-tool.md)
   + [批量导入资源](./migration/bulk-import.md)
   + 移动到 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [简介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [入门培训](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA和CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM现代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存储库现代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute微服务](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜索和编制索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 内容迁移 {#content-migration}
         + [批量导入服务](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [内容传输工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [常见问题解答](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [疑难解答](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Formsas a Cloud Service {#aem-forms}
         + [简介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [数字注册](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通信](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [简介](./migration/cloud-acceleration-manager/introduction.md)
      + [准备工作和Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [实施阶段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [内容传输工具](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [代码重构工具](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [代码存储库现代化器](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher转换器](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引转换器](./migration/cloud-acceleration-manager/index-converter.md)
      + [资源工作流迁移工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [浏览Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + 针对Formsas a Cloud Service进行开发{#developing-for-cloud-service}
      + [1 — 快速入门](./forms/developing-for-cloud-service/getting-started.md)
      + [2 — 安装IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 — 设置Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 — 将IntelliJ与AEM同步](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 — 构建表单](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 — 自定义提交处理程序](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 — 使用资源类型注册servlet](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 — 启用Forms Portal组件](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 — 包括Cloud Service和FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 — 上下文感知云配置](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 — 推送到Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 — 部署到开发环境](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 — 更新maven原型](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + 创建自适应表单{#create-first-af}
      + [简介](./forms/create-first-af/introduction.md)
      + [创建名称](./forms/create-first-af/create-theme.md)
      + [创建模板](./forms/create-first-af/create-template.md)
      + [创建片段](./forms/create-first-af/create-fragments.md)
      + [创建表单](./forms/create-first-af/create-af.md)
      + [配置根面板](./forms/create-first-af/configure-root-panel.md)
      + [配置人员面板](./forms/create-first-af/configure-people-panel.md)
      + [配置收入面板](./forms/create-first-af/configure-income-panel.md)
      + [“配置资源”面板](./forms/create-first-af/configure-assets-panel.md)
      + [配置开始面板](./forms/create-first-af/configure-start-panel.md)
      + [添加并配置工具栏](./forms/create-first-af/add-configure-toolbar.md)
   + 带Headless表单的自定义提交服务{#custom-submit-headless-forms}
      + [1 — 简介](./forms/custom-submit-headless-forms/introduction.md)
      + [2 — 创建自定义提交服务](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 — 显示响应](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + AEM Forms和Analytics{#forms-and-analytics}
      + [简介](./forms/form-data-analytics/introduction.md)
      + [创建数据元素](./forms/form-data-analytics/data-elements.md)
      + [创建规则](./forms/form-data-analytics/rules.md)
      + [测试解决方案](./forms/form-data-analytics/test.md)
   + AEM Forms CS中的Document Generation{#doc-gen-formscs}
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
      + [创建USC批次配置](./forms/formscs-batch-api/configure-usc-batch.md)
      + [创建批次配置](./forms/formscs-batch-api/create-batch-config.md)
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
   + 使用Blob索引标记存储表单提交{#store-submiited-data-with-metadata-tags}
      + [简介](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [扩展选择组组件](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [创建OSGi配置](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [创建索引标记](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [创建自定义提交](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + 预填充基于核心组件的表单{#prefill-core-component-based-form}
      + [简介](./forms/prefill-core-component-form/introduction.md)
      + [写入预填充服务](./forms/prefill-core-component-form/pre-fill-service.md)
      + [测试解决方案](./forms/prefill-core-component-form/test-solution.md)
   + Azure门户存储{#forms-cs-azure-portal}
      + [简介](./forms/forms-cs-azure-portal/introduction.md)
      + [创建表单数据模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [将表单数据存储在Azure存储中](./forms/forms-cs-azure-portal/create-af.md)
      + [预填表单](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查询提交](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 保存并继续填写表单{#prefill-azure-storage}
      + [1 — 简介](./forms/prefill-azure-storage/introduction.md)
      + [2 — 创建页面组件](./forms/prefill-azure-storage/page-component.md)
      + [3 — 创建自适应表单模板](./forms/prefill-azure-storage/associate-page-component.md)
      + [4 — 创建Azure存储集成](./forms/prefill-azure-storage/create-fdm.md)
      + [5 — 创建SendGrid集成](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 — 创建自适应表单](./forms/prefill-azure-storage/create-af.md)
      + [7 — 部署示例资源](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + 创建审阅工作流{#create-aem-workflow}
      + [将工作流存储外部化](./forms/create-aem-workflow/externalize-workflow.md)
      + [创建工作流模型](./forms/create-aem-workflow/create-workflow.md)
      + [触发工作流](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign与AEM Forms{#forms-and-sign}
      + [简介](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API应用程序](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign云配置](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [创建自适应表单](./forms/forms-and-sign/create-adaptive-form.md)
      + [用于填写和签名的配置](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 与Microsoft Power Automate集成{#forms-cs-and-power-automate}
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
   + 将表单提交存储在一个驱动器和SharePoint中{#one-drive}
      + [将表单数据存储在一个驱动器中](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [将表单数据存储在SharePoint中](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [使用SharePoint列表中的数据预填充表单](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
+ asset compute可扩展性{#asset-compute}
   + [概述](./asset-compute/overview.md)
   + 设置{#set-up}
      + [帐户和服务配置](./asset-compute/set-up/accounts-and-services.md)
      + [本地开发环境](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
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

+ 多步Tutorials{#multi-step-tutorials}
   + [AEM Sites开发](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html){target=_blank}
   + [SPA编辑器(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ 专家资源 {#expert-resources}
   + AEM Champions {#aem-champions}
      + [Cloud Manager入门行动手册](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Cloud Manager环境类型](./expert-resources/aem-champions/environment-types.md)
      + [Cloud Manager UI](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM Experts系列](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [简介](./expert-resources/cloud-5/cloud5-introduction.md)
      + [第1季](./expert-resources/cloud-5/cloud5-season-1.md)
      + [第2季](./expert-resources/cloud-5/cloud5-season-2.md)
      + [第3季](./expert-resources/cloud-5/cloud5-season-3.md)
      + [AEM CDN第1部分](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN第2部分](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM日志文件](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [登录令牌](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [云调度程序](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [迁移1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [迁移2](./expert-resources/cloud-5/cloud5-aem-content-migration-part-2.md)
      + [Dispatcher验证器](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [搜索和编制索引](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe应用程序生成器](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + 第2季{#season-2}
         + [片段](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Repo Modernizer](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling作业计划程序](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [修复缓存](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [修复您的重写](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager — 体验审核](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager — 单元测试](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager — 功能测试](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + 第3季{#season-3}
         + [第三方搜索](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
