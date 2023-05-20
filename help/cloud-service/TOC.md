---
user-guide-title: Adobe Experience Manager as a Cloud Service 教程
user-guide-description: Adobe Experience Manager as a Cloud Service 的教程集合。
breadcrumb-title: AEM as a Cloud Service 教程
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: a439c72a7b080633d3777eefad3b47f01c92b970
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 21%

---


# Adobe Experience Manager as a Cloud Service 教程 {#cloud-service}

+ [概述](./overview.md)
+ AEM試用版 {#aem-trials}
   + [图像](./aem-trials/images.md)
+ AEM as a Cloud Service 简介{#introduction}
   + [什麼是AEMas a Cloud Service？](./introduction/what-is-aem-as-a-cloud-service.md)
   + [演化](./introduction/evolution.md)
   + [架构](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 策略與思維領導力{#strategy}
      + [Experience Manager — 治理與人員配置模型與原型](./introduction/experience-manager-governance-and-staffing-models.md)
      + [如何使用Adobe Experience Manager加快內容速度](./introduction/drive-content-velocity-for-sites.md)
      + [使用AEM樣式系統加快內容速度](./introduction/accelerate-content-velocity-aem.md)
+ Experience Cloud 集成{#integrations}
   + [集成](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ 基礎技術 {#underlying-technology}
   + [AEM架構](./underlying-technology/introduction-architecture.md)
   + [osgi](./underlying-technology/introduction-osgi.md)
   + [Java內容存放庫](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [製作和發佈服務](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [项目](./cloud-manager/programs.md)
   + [环境](./cloud-manager/environments.md)
   + [CI/CD 生产管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 非生产管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活动](./cloud-manager/activity.md)
   + Dev Ops{#devops}
      + [部署程式碼](./cloud-manager/devops/deploy-code.md)
      + [合并项目](./cloud-manager/devops/merge-projects.md)
      + [設定管道](./cloud-manager/devops/configure-pipelines.md)
      + [持續整合](./cloud-manager/devops/continuous-integration.md)
      + [分析測試結果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 配置](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ 本機開發環境設定 {#local-development-environment-set-up}
   + [概述](./local-development-environment/overview.md)
   + [開發工具](./local-development-environment/development-tools.md)
   + [本地 AEM 运行时](./local-development-environment/aem-runtime.md)
   + [本地 Dispatcher 工具](./local-development-environment/dispatcher-tools.md)
+ 开发{#developing}
   + 擴充性{#extensibility}
      + App Builder{#app-builder}
         + [產生存取權杖](./developing/extensibility/app-builder/jwt-auth.md)
      + 內容片段主控台{#content-fragments}
         + [概述](./developing/extensibility/content-fragments/overview.md)
         + [Adobe Developer Console專案](./developing/extensibility/content-fragments/adobe-developer-console-project.md)
         + [應用程式初始化](./developing/extensibility/content-fragments/app-initialization.md)
         + [擴充功能註冊](./developing/extensibility/content-fragments/extension-registration.md)
         + [頁首功能表](./developing/extensibility/content-fragments/header-menu.md)
         + [動作列](./developing/extensibility/content-fragments/action-bar.md)
         + [模态](./developing/extensibility/content-fragments/modal.md)
         + [Adobe I/O Runtime動作](./developing/extensibility/content-fragments/runtime-action.md)
         + [测试](./developing/extensibility/content-fragments/test.md)
         + [部署](./developing/extensibility/content-fragments/deploy.md)
         + 範例擴充功能{#example-extensions}
            + [大量屬性更新](./developing/extensibility/content-fragments/example-extensions/bulk-property-update.md)
            + [使用OpenAI產生AEM影像資產](./developing/extensibility/content-fragments/example-extensions/image-generation-and-image-upload.md)
   + 开发基础{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本機開發環境](./developing/basics/local-development-environment.md)
      + [AEM 项目原型](./developing/basics/aem-project-archetype.md)
      + [AEM 项目结构](./developing/basics/project-structure.md)
      + [可變與不可變內容](./developing/basics/mutable-immutable.md)
      + [存放庫結構套件](./developing/basics/repository-structure-package.md)
      + [內容發佈](./developing/basics/content-publishing.md)
      + [OSGi 配置](./developing/basics/osgi-configurations.md)
      + [Dispatcher設定移轉](./developing/basics/dispatcher-configuration.md)
   + AEM 项目{#aem-projects}
      + [AEM Maven專案](./developing/projects/maven-project-structure.md)
      + [清除AEM Maven專案](./developing/projects/remove-samples.md)
   + OSGi服務{#osgi-services}
      + [OSGi服務基本需知](./developing/osgi-services/basics.md)
      + [OSGi元件生命週期](./developing/osgi-services/lifecycle.md)
      + [OSGi設定基本需知](./developing/osgi-services/configurations.md)
      + [使用OCD的OSGi設定](./developing/osgi-services/configurations-ocd.md)
   + 高级{#advanced}
      + [網頁最佳化的影像API](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [服務使用者](./developing/advanced/service-users.md)
      + [自訂名稱空間](./developing/advanced/custom-namespaces.md)
      + [快取頁面變體](./developing/advanced/variant-caching.md)
   + 快速開發環境{#rde}
      + [概述](./developing/rde/overview.md)
      + [如何設定](./developing/rde/how-to-setup.md)
      + [使用方式](./developing/rde/how-to-use.md)
      + [開發生命週期](./developing/rde/development-life-cycle.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ 偵錯AEM{#debugging}
   + 偵錯AEM SDK{#debugging-aem-sdk}
      + [概述](./debugging/aem-sdk-local-quickstart/overview.md)
      + [日志](./debugging/aem-sdk-local-quickstart/logs.md)
      + [远程调试](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web主控台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 偵錯AEMas a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概述](./debugging/cloud-service/overview.md)
      + [日志](./debugging/cloud-service/logs.md)
      + [建置和部署](./debugging/cloud-service/build-and-deployment.md)
      + [开发人员控制台](./debugging/cloud-service/developer-console.md)
      + [存储库浏览器](./debugging/cloud-service/repository-browser.md)
      + 風險{#risks}
         + [周遊警告](./debugging/cloud-service/risks/traversals.md)
+ 內容傳送{#content-delivery}
   + [URL重新導向](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html)
+ 存取AEM{#accessing}
   + [概述](./accessing/overview.md)
   + [Adobe IMS使用者](./accessing/adobe-ims-users.md)
   + [Adobe IMS使用者群組](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS產品設定檔](./accessing/adobe-ims-product-profiles.md)
   + [AEM使用者、群組和許可權](./accessing/aem-users-groups-and-permissions.md)
   + [配置对 AEM 演练的访问权限](./accessing/walk-through.md)
+ 身份验证{#authentication}
   + [概述](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 進階網路{#networking}
   + [概述](./networking/advanced-networking.md)
   + [彈性的連線埠輸出](./networking/flexible-port-egress.md)
   + [專用輸出IP位址](./networking/dedicated-egress-ip-address.md)
   + [虛擬私人網路](./networking/vpn.md)
   + 程式碼範例{#examples}
      + [非標準連線埠上的HTTP/HTTPS，用於彈性連線埠輸出](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [專用輸出IP位址/VPN的HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [使用資料來源集區的SQL連線](./networking/examples/sql-datasourcepool.md)
      + [使用Java SQL API的SQL連線](./networking/examples/sql-java-apis.md)
      + [電子郵件服務](./networking/examples/email-service.md)
+ 迁移 {#migration}
   + [内容传输工具](./migration/content-transfer-tool.md)
   + [大量匯入資產](./migration/bulk-import.md)
   + 移动到 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [简介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [入门培训](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA與CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM現代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存放庫現代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset compute微服務](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜尋和建立索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 内容迁移 {#content-migration}
         + [大量匯入服務](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [内容传输工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [常见问题](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [疑难解答](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Formsas a Cloud Service {#aem-forms}
         + [简介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [數位註冊](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通信](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [简介](./migration/cloud-acceleration-manager/introduction.md)
      + [整備與Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [實作階段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [内容传输工具](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [程式碼重構工具](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [程式碼存放庫現代化工具](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引转换器](./migration/cloud-acceleration-manager/index-converter.md)
      + [资源工作流迁移工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [瀏覽Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}
   + 針對Formsas a Cloud Service開發{#developing-for-cloud-service}
      + [快速入門](./forms/developing-for-cloud-service/getting-started.md)
      + [安裝IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [設定Git](./forms/developing-for-cloud-service/setup-git.md)
      + [將IntelliJ與AEM同步](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [建立表單](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [啟用Forms Portal元件](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [包含Cloud Services和FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [內容感知雲端設定](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [推送到Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [部署至開發環境](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [更新maven原型](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + 建立最適化表單{#create-first-af}
      + [简介](./forms/create-first-af/introduction.md)
      + [创建名称](./forms/create-first-af/create-theme.md)
      + [创建模板](./forms/create-first-af/create-template.md)
      + [建立片段](./forms/create-first-af/create-fragments.md)
      + [创建表单](./forms/create-first-af/create-af.md)
      + [設定根面板](./forms/create-first-af/configure-root-panel.md)
      + [設定人員面板](./forms/create-first-af/configure-people-panel.md)
      + [設定收入面板](./forms/create-first-af/configure-income-panel.md)
      + [設定資產面板](./forms/create-first-af/configure-assets-panel.md)
      + [設定開始面板](./forms/create-first-af/configure-start-panel.md)
      + [新增和設定工具列](./forms/create-first-af/add-configure-toolbar.md)
   + AEM Forms與Analytics{#forms-and-analytics}
      + [简介](./forms/form-data-analytics/introduction.md)
      + [建立資料元素](./forms/form-data-analytics/data-elements.md)
      + [建立規則](./forms/form-data-analytics/rules.md)
      + [測試解決方案](./forms/form-data-analytics/test.md)
   + AEM Forms CS中的檔案產生{#doc-gen-formscs}
      + [简介](./forms/doc-gen-forms-cs/introduction.md)
      + [建立服務認證](./forms/doc-gen-forms-cs/service-credentials.md)
      + [建立JWT權杖](./forms/doc-gen-forms-cs/create-jwt.md)
      + [建立存取Token](./forms/doc-gen-forms-cs/create-access-token.md)
      + [合併資料與範本](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [測試解決方案](./forms/doc-gen-forms-cs/test.md)
      + [挑戰](./forms/doc-gen-forms-cs/challenge.md)
   + 使用批次API產生檔案{#formscs-batch-api}
      + [简介](./forms/formscs-batch-api/introduction.md)
      + [設定Azure儲存體](./forms/formscs-batch-api/configure-azure-storage.md)
      + [建立USC批次設定](./forms/formscs-batch-api/configure-usc-batch.md)
      + [建立批次設定](./forms/formscs-batch-api/create-batch-config.md)
      + [執行批次](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Forms CS中的PDF操作{#forms-cs-assembler}
      + [简介](./forms/forms-cs-assembler/introduction.md)
      + [建立服務認證](./forms/forms-cs-assembler/service-credentials.md)
      + [建立JWT權杖](./forms/forms-cs-assembler/create-jwt.md)
      + [建立存取Token](./forms/forms-cs-assembler/create-access-token.md)
      + [組合PDF檔案](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A公用程式](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [測試解決方案](./forms/forms-cs-assembler/test.md)
      + [挑戰](./forms/forms-cs-assembler/challenge.md)
   + Azure入口網站儲存體{#forms-cs-azure-portal}
      + [简介](./forms/forms-cs-azure-portal/introduction.md)
      + [创建表单数据模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [將表單資料儲存在Azure儲存體中](./forms/forms-cs-azure-portal/create-af.md)
      + [預填表單](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查詢提交](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 建立稽核工作流程{#create-aem-workflow}
      + [將工作流程儲存區外部化](./forms/create-aem-workflow/externalize-workflow.md)
      + [建立工作流程模型](./forms/create-aem-workflow/create-workflow.md)
      + [觸發工作流程](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign與AEM Forms{#forms-and-sign}
      + [简介](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API應用計畫](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign雲端設定](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [建立最適化表單](./forms/forms-and-sign/create-adaptive-form.md)
      + [設定以填寫和簽署](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 與Microsoft Power Automate整合{#forms-cs-and-power-automate}
      + [設定整合](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [剖析提交的表單資料](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [將DoR作為電子郵件附件傳送](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [從提交的資料擷取表單附件](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + 與Microsoft Dynamics整合{#formscs-dynamics-crm}
      + [建立Dynamics應用程式](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [設定資料來源](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [创建表单数据模型](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [建立最適化表單](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + 與Salesforce整合{#integrate-with-salesforce}
      + [简介](./forms/integrate-with-salesforce/introduction.md)
      + [建立連線應用程式](./forms/integrate-with-salesforce/create-connected-app.md)
      + [建立Swagger檔案](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [建立資料來源](./forms/integrate-with-salesforce/create-data-source.md)
      + [建立表單資料模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [測試表單提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [測試點選事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + 將表單提交內容儲存在單一磁碟機和Sharepoint中{#one-drive}
      + [將表單資料儲存在單一磁碟機中](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [將表單資料儲存在SharePoint中](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
+ asset compute擴充性{#asset-compute}
   + [概述](./asset-compute/overview.md)
   + 設定{#set-up}
      + [帳戶和服務布建](./asset-compute/set-up/accounts-and-services.md)
      + [本機開發環境](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + 开发{#develop}
      + [建立Asset compute專案](./asset-compute/develop/project.md)
      + [設定環境變數](./asset-compute/develop/environment-variables.md)
      + [設定manifest.yml](./asset-compute/develop/manifest.md)
      + [開發背景工作](./asset-compute/develop/worker.md)
      + [使用開發工具](./asset-compute/develop/development-tool.md)
   + 測試和偵錯{#test-debug}
      + [測試背景工作](./asset-compute/test-debug/test.md)
      + [對背景工作進行偵錯](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署至Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [與AEM整合](./asset-compute/deploy/processing-profiles.md)
   + 高级{#advanced}
      + [中繼資料背景工作](./asset-compute/advanced/metadata.md)
   + [疑难解答](./asset-compute/troubleshooting.md)

+ 多步驟Tutorials{#multi-step-tutorials}
   + [AEM Sites開發](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA編輯器(React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [AEM Sites和Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [權杖型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
+ 專家資源 {#expert-resources}
   + AEM Champions {#aem-champions}
      + [Cloud Manager入門行動手冊](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Cloud Manager環境型別](./expert-resources/aem-champions/environment-types.md)
      + [Cloud Manager UI](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM Experts系列](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [简介](./expert-resources/cloud-5/cloud5-introduction.md)
      + [第1季](./expert-resources/cloud-5/cloud5-season-1.md)
      + [第2季](./expert-resources/cloud-5/cloud5-season-2.md)
      + [AEM CDN第1部分](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN第2部分](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM記錄檔](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [登入Token](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [雲端Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [移轉1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [移轉2](./expert-resources/cloud-5/cloud5-aem-content-migration-part-2.md)
      + [Dispatcher驗證器](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [搜尋和建立索引](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [AdobeApp Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + 第2季{#season-2}
         + [片段](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Repo Modernizer](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling工作排程器](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [修正您的快取](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [修正您的重新寫入](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager — 體驗稽核](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager — 單元測試](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager — 功能測試](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)