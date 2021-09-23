---
user-guide-title: Adobe Experience Manager as a Cloud Service 教程
user-guide-description: Adobe Experience Manager as a Cloud Service 的教程集合。
breadcrumb-title: AEM as a Cloud Service 教程
sub-product: cloud-service
team: TM
source-git-commit: 598d00578e5179f76b6f309c5c14dc7b1634f051
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 22%

---


# Adobe Experience Manager as a Cloud Service 教程 {#cloud-service}

+ [概述](./overview.md)
+ AEM as a Cloud Service 简介{#introduction}
   + [什么是AEM as aCloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [进化](./introduction/evolution.md)
   + [架构](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ 基础技术{#underlying-technology}
   + [AEM架构](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java内容存储库](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [创作和发布服务](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [程序](./cloud-manager/programs.md)
   + [环境](./cloud-manager/environments.md)
   + [CI/CD生产管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD非生产管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活动](./cloud-manager/activity.md)
   + Dev Ops{#devops}
      + [部署代码](./cloud-manager/devops/deploy-code.md)
      + [合并项目](./cloud-manager/devops/merge-projects.md)
      + [配置管道](./cloud-manager/devops/configure-pipelines.md)
      + [持续集成](./cloud-manager/devops/continuous-integration.md)
      + [分析测试结果](./cloud-manager/devops/analyze-test-results.md)
      + [调度程序配置](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 本地开发环境设置{#local-development-environment-set-up}
   + [概述](./local-development-environment/overview.md)
   + [开发工具](./local-development-environment/development-tools.md)
   + [本地AEM运行时](./local-development-environment/aem-runtime.md)
   + [本地Dispatcher工具](./local-development-environment/dispatcher-tools.md)
+ 开发{#developing}
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
   + OSGi服务{#osgi-services}
      + [OSGi服务基础知识](./developing/osgi-services/basics.md)
      + [OSGi组件生命周期](./developing/osgi-services/lifecycle.md)
      + [OSGi配置基础知识](./developing/osgi-services/configurations.md)
      + [使用OCD的OSGi配置](./developing/osgi-services/configurations-ocd.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ 调试AEM{#debugging}
   + 调试AEM SDK{#debugging-aem-sdk}
      + [概述](./debugging/aem-sdk-local-quickstart/overview.md)
      + [日志](./debugging/aem-sdk-local-quickstart/logs.md)
      + [远程调试](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 调试AEM as aCloud Service{#debugging-aem-as-a-cloud-service}
      + [概述](./debugging/cloud-service/overview.md)
      + [日志](./debugging/cloud-service/logs.md)
      + [构建和部署](./debugging/cloud-service/build-and-deployment.md)
      + [开发人员控制台](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ 访问AEM{#accessing}
   + [概述](./accessing/overview.md)
   + [AdobeIMS用户](./accessing/adobe-ims-users.md)
   + [AdobeIMS用户组](./accessing/adobe-ims-user-groups.md)
   + [AdobeIMS产品配置文件](./accessing/adobe-ims-product-profiles.md)
   + [AEM用户、组和权限](./accessing/aem-users-groups-and-permissions.md)
   + [配置对AEM演练的访问权限](./accessing/walk-through.md)
+ 迁移{#migration}
   + [内容传输工具](./migration/content-transfer-tool.md)
   + [批量导入资产](./migration/bulk-import.md)

   + 移动到 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [简介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [入门](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [双酚A和CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM现代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存储库现代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute微服务](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜索和索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 内容迁移{#content-migration}
         + [批量导入服务](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [内容传输工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
      + [疑难解答](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as aCloud Service{#aem-forms}
         + [简介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [数字注册](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通信](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [简介](./migration/cloud-acceleration-manager/introduction.md)
      + [就绪性和最佳实践分析器](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [实施阶段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [内容传输工具](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [代码重构工具](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [代码存储库Modernizer](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引转换器](./migration/cloud-acceleration-manager/index-converter.md)
      + [资产工作流迁移工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [导航Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ 表单{#forms}
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
   + Document CloudAPI和AEM Forms CS{#doc-cloud-sdk}
      + [简介](./forms/doc-cloud-sdk/introduction.md)
      + [创建Adobe I/O项目](./forms/doc-cloud-sdk/create-document-cloud-credentials.md)
      + [创建OSGi配置](./forms/doc-cloud-sdk/create-doc-cloud-configuration.md)
      + [定义界面](./forms/doc-cloud-sdk/create-interface.md)
      + [实施界面](./forms/doc-cloud-sdk/implement-interface.md)
      + [创建JSON部分](./forms/doc-cloud-sdk/get-content-analyzer.md)
      + [自定义流程步骤](./forms/doc-cloud-sdk/custom-process-step.md)
   + Azure门户存储{#forms-cs-azure-portal}
      + [简介](./forms/forms-cs-azure-portal/introduction.md)
      + [创建表单数据模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [在Azure存储中存储表单数据](./forms/forms-cs-azure-portal/create-af.md)
      + [预填表单](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查询提交](./forms/forms-cs-azure-portal/query-submitted-data.md)


      + 创建审核工作流{#create-aem-workflow}
         + [创建工作流模型](./forms/create-aem-workflow/create-workflow.md)
         + [触发工作流](./forms/create-aem-workflow/configure-af.md)
      + Adobe Sign与AEM Forms{#forms-and-sign}
         + [简介](./forms/forms-and-sign/introduction.md)
         + [Adobe Sign API应用程序](./forms/forms-and-sign/create-sign-api-application.md)
         + [Adobe Sign 云配置](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
         + [创建自适应表单](./forms/forms-and-sign/create-adaptive-form.md)
         + [配置以进行填写和签名](./forms/forms-and-sign/configure-form-fill-and-sign.md)
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
      + [Adobe项目Firefly](./asset-compute/set-up/firefly.md)
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
   + [AEM Sites开发](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA Editor(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA编辑器(Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
