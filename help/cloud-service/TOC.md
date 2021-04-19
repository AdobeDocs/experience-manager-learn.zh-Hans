---
user-guide-title: Adobe Experience Manager as a Cloud Service 教程
user-guide-description: Adobe Experience Manager as a Cloud Service 的教程集合。
breadcrumb-title: AEM as a Cloud Service 教程
sub-product: 云服务
team: TM
translation-type: tm+mt
source-git-commit: 0c7759b59e6b6c99da3cd7e7c502445c14964e26
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 22%

---


# Adobe Experience Manager as a Cloud Service 教程 {#cloud-service}

+ [概述](./overview.md)
+ AEM as a Cloud Service 简介{#introduction}
   + [什么是AEM作为Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [进化](./introduction/evolution.md)
   + [架构](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ 底层技术{#underlying-technology}
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
   + 开发运营{#devops}
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
   + [本地AEM Runtime](./local-development-environment/aem-runtime.md)
   + [本地调度程序工具](./local-development-environment/dispatcher-tools.md)
+ 开发{#developing}
   + 开发基础{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本地开发环境](./developing/basics/local-development-environment.md)
      + [AEM 项目原型](./developing/basics/aem-project-archetype.md)
      + [AEM 项目结构](./developing/basics/project-structure.md)
      + [可变内容与不可变内容](./developing/basics/mutable-immutable.md)
      + [存储库结构包](./developing/basics/repository-structure-package.md)
      + [内容发布](./developing/basics/content-publishing.md)
      + [OSGi配置](./developing/basics/osgi-configurations.md)
      + [调度程序配置迁移](./developing/basics/dispatcher-configuration.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ 调试AEM{#debugging}
   + 调试AEM SDK{#debugging-aem-sdk}
      + [概述](./debugging/aem-sdk-local-quickstart/overview.md)
      + [日志](./debugging/aem-sdk-local-quickstart/logs.md)
      + [远程调试](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [调度程序工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 将AEM调试为Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概述](./debugging/cloud-service/overview.md)
      + [日志](./debugging/cloud-service/logs.md)
      + [构建和部署](./debugging/cloud-service/build-and-deployment.md)
      + [开发人员控制台](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ 访问AEM{#accessing}
   + [概述](./accessing/overview.md)
   + [AdobeIMS用户](./accessing/adobe-ims-users.md)
   + [AdobeIMS用户组](./accessing/adobe-ims-user-groups.md)
   + [AdobeIMS产品用户档案](./accessing/adobe-ims-product-profiles.md)
   + [AEM用户、用户组和权限](./accessing/aem-users-groups-and-permissions.md)
   + [配置对AEM直通服务的访问](./accessing/walk-through.md)
+ 迁移{#migration}
   + [内容传输工具](./migration/content-transfer-tool.md)
   + [批量导入资产](./migration/bulk-import.md)
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
      + [配置资源面板](./forms/create-first-af/configure-assets-panel.md)
      + [配置开始面板](./forms/create-first-af/configure-start-panel.md)
      + [添加和配置工具栏](./forms/create-first-af/add-configure-toolbar.md)
   + 创建审阅工作流{#create-aem-workflow}
      + [创建工作流模型](./forms/create-aem-workflow/create-workflow.md)
      + [触发工作流](./forms/create-aem-workflow/configure-af.md)
   + Adobe Sign wAEM Forms{#forms-and-sign}
      + [Adobe Sign API应用程序](./forms/forms-and-sign/create-sign-api-application.md)
      + [Adobe Sign 云配置](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [创建自适应表单](./forms/forms-and-sign/create-adaptive-form.md)
      + [配置以填写和签名](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 与Salesforce{#integrate-with-salesforce}集成
      + [简介](./forms/integrate-with-salesforce/introduction.md)
      + [创建连接的应用程序](./forms/integrate-with-salesforce/create-connected-app.md)
      + [创建Swagger文件](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [创建数据源](./forms/integrate-with-salesforce/create-data-source.md)
      + [创建表单数据模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [测试表单提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [测试单击事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute可扩展性{#asset-compute}
   + [概述](./asset-compute/overview.md)
   + 设置{#set-up}
      + [帐户和服务配置](./asset-compute/set-up/accounts-and-services.md)
      + [本地开发环境](./asset-compute/set-up/development-environment.md)
      + [AdobeProject Firefly](./asset-compute/set-up/firefly.md)
   + 开发{#develop}
      + [创建Asset compute项目](./asset-compute/develop/project.md)
      + [配置环境变量](./asset-compute/develop/environment-variables.md)
      + [配置manifest.yml](./asset-compute/develop/manifest.md)
      + [开发员工](./asset-compute/develop/worker.md)
      + [使用开发工具](./asset-compute/develop/development-tool.md)
   + 测试和调试{#test-debug}
      + [测试工作人员](./asset-compute/test-debug/test.md)
      + [调试工作人员](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署到Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [与AEM集成](./asset-compute/deploy/processing-profiles.md)
   + 高级{#advanced}
      + [元数据工作程序](./asset-compute/advanced/metadata.md)
   + [疑难解答](./asset-compute/troubleshooting.md)
+ 多步Tutorials{#multi-step-tutorials}
   + [AEM Sites开发](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/develop-wknd-tutorial.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA Editor(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA Editor(Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
