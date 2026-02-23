---
user-guide-title: Adobe Experience Manager as a Cloud Service 教程
user-guide-description: Adobe Experience Manager as a Cloud Service 的教程集合。
breadcrumb-title: AEM as a Cloud Service 教程
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Experience Manager as a Cloud Service
team: TM
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '1455'
ht-degree: 97%

---


# Adobe Experience Manager as a Cloud Service 教程 {#cloud-service}

+ [概述](./overview.md)
+ AEM 试用版 {#aem-trials}
   + [图像](./aem-trials/images.md)
+ 播放列表{#playlists}
   + [AEM 开发](./playlists/development.md)
+ AEM as a Cloud Service 简介{#introduction}
   + [AEM as a Cloud Service 是什么？](./introduction/what-is-aem-as-a-cloud-service.md)
   + [架构](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 战略与思维领导力{#strategy}
      + [Experience Manager — 治理与人员配置模型及典型范例](./introduction/experience-manager-governance-and-staffing-models.md)
+ [Experience Hub](./experience-hub.md)
+ 人工智能 {#ai}
   + [概述](./ai/overview.md)
   + [设置和配置](./ai/setup.md)
   + [AI 助手](./ai/ai-assistant.md)
   + [代理](./ai/agents-in-aem.md)
   + [使用AEM Development Agent对CI/CD管道进行故障诊断](./ai/development-agent-troubleshoot-ci-cd-pipeline.md)
+ Experience Cloud 集成{#integrations}
   + [集成](./integrations/experience-cloud.md)
   + [AEM Headless 和 Target](./integrations/target.md)
+ 底层技术 {#underlying-technology}
   + [AEM 体系结构](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java 内容存储库](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Author 和 Publish 服务](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [AEM Assets Sidekick 插件](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [程序](./cloud-manager/programs.md)
   + [环境](./cloud-manager/environments.md)
   + [使用 GitHub 存储库](./cloud-manager/byogithub.md)
   + [CI/CD 生产管道](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 非生产管道](./cloud-manager/cicd-non-production-pipeline.md)
   + [活动](./cloud-manager/activity.md)
   + [自定义域名](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + [内容还原](./cloud-manager/content-restore.md)
   + 开发运营{#devops}
      + [部署代码](./cloud-manager/devops/deploy-code.md)
      + [合并项目](./cloud-manager/devops/merge-projects.md)
      + [配置管道](./cloud-manager/devops/configure-pipelines.md)
      + [持续集成](./cloud-manager/devops/continuous-integration.md)
      + [分析测试结果](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher 配置](./cloud-manager/devops/dispatcher-configurations.md)
      + [CDN 日志分析](./cloud-manager/devops/cdn-log-analysis.md)
+ 本地开发环境设置 {#local-development-environment-set-up}
   + [概述](./local-development-environment/overview.md)
   + [开发工具](./local-development-environment/development-tools.md)
   + [本地 AEM SDK](./local-development-environment/aem-runtime.md)
   + [本地 Dispatcher 工具](./local-development-environment/dispatcher-tools.md)
+ 开发{#developing}
   + 可扩展性{#extensibility}
      + App Builder{#app-builder}
         + [生成 JWT 访问令牌](./developing/extensibility/app-builder/jwt-auth.md)
         + [生成服务器到服务器访问令牌](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Github webhook 验证](./developing/extensibility/app-builder/github-webhook-verification.md)
      + UI 可扩展性{#ui}
         + [概述](./developing/extensibility/ui/overview.md)
         + [Adobe Developer Console 项目](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [初始化应用程序](./developing/extensibility/ui/app-initialization.md)
         + [注册扩展](./developing/extensibility/ui/extension-registration.md)
         + [模态](./developing/extensibility/ui/modal.md)
         + [Adobe I/O Runtime 操作](./developing/extensibility/ui/runtime-action.md)
         + [验证](./developing/extensibility/ui/verify.md)
         + [部署](./developing/extensibility/ui/deploy.md)
         + 内容片段{#content-fragments}
            + [概述](./developing/extensibility/ui/content-fragments/overview.md)
            + 示例{#examples}
               + [AI 图像生成](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [批量属性更新](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [自定义网格列](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [导出为 XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [RTE 工具栏按钮](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [RTE 构件](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [RTE 徽章](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [自定义字段](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + 开发基础{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [本地开发环境](./developing/basics/local-development-environment.md)
      + [AEM 项目原型](./developing/basics/aem-project-archetype.md)
      + [AEM 项目结构](./developing/basics/project-structure.md)
      + [可变内容与不可变内容](./developing/basics/mutable-immutable.md)
      + [存储库结构包](./developing/basics/repository-structure-package.md)
      + [内容发布](./developing/basics/content-publishing.md)
      + [OSGi 配置](./developing/basics/osgi-configurations.md)
      + [Dispatcher 配置迁移](./developing/basics/dispatcher-configuration.md)
   + AEM 项目{#aem-projects}
      + [AEM Maven 项目](./developing/projects/maven-project-structure.md)
      + [清理 AEM Maven 项目](./developing/projects/remove-samples.md)
   + OSGi 服务{#osgi-services}
      + [OSGi 服务基础知识](./developing/osgi-services/basics.md)
      + [OSGi 组件生命周期](./developing/osgi-services/lifecycle.md)
      + [OSGi 配置基础知识](./developing/osgi-services/configurations.md)
      + [使用 OCD 配置 OSGi](./developing/osgi-services/configurations-ocd.md)
   + 高级{#advanced}
      + [缓存页面变体](./developing/advanced/variant-caching.md)
      + [CSRF 保护](./developing/advanced/csrf-protection.md)
      + [自定义命名空间](./developing/advanced/custom-namespaces.md)
      + [从 HTL 参数化 Sling 模型](./developing/advanced/sling-model-parameters.md)
      + [密码](./developing/advanced/secrets.md)
      + [服务用户](./developing/advanced/service-users.md)
      + [Web 优化图像 API](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [在 AEM Author 中于主实例上运行作业](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
      + [查找和删除已弃用的API](./developing/advanced/deprecated-apis-find-removal.md)
   + 快速开发环境{#rde}
      + [概述](./developing/rde/overview.md)
      + [如何设置](./developing/rde/how-to-setup.md)
      + [使用方法](./developing/rde/how-to-use.md)
      + [开发生命周期](./developing/rde/development-life-cycle.md)
   + 通用编辑器{#universal-editor}
      + React 应用程序编辑{#react-app-editing}
         + [概述](./developing/universal-editor/react-app/overview.md)
         + [本地开发设置](./developing/universal-editor/react-app/local-development-setup.md)
         + [为 React 应用程序添加监测与分析工具](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ 调试 AEM{#debugging}
   + 调试 AEM SDK{#debugging-aem-sdk}
      + [概述](./debugging/aem-sdk-local-quickstart/overview.md)
      + [日志](./debugging/aem-sdk-local-quickstart/logs.md)
      + [远程调试](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi 网页控制台](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher 工具](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [其他工具](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + 调试 AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概述](./debugging/cloud-service/overview.md)
      + [日志](./debugging/cloud-service/logs.md)
      + [生成和部署](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [存储库浏览器](./debugging/cloud-service/repository-browser.md)
      + 风险{#risks}
         + [遍历警告](./debugging/cloud-service/risks/traversals.md)
+ 个性化 {#personalization}
   + [概述](./personalization/overview.md)
   + [实时演示](./personalization/live-demo.md)
   + 设置{#setup}
      + [集成 Adobe Target](./personalization/setup/integrate-adobe-target.md)
      + [集成标记](./personalization/setup/integrate-adobe-tags.md)
   + 用例 {#use-cases}
      + [试验（A/B 测试）](./personalization/use-cases/experimentation.md)
      + [行为定位](./personalization/use-cases/behavioral-targeting.md)
      + [已知用户Personalization](./personalization/use-cases/known-user-personalization.md)
+ AEM API{#aem-apis}
   + [概述](./apis/overview.md)
   + OpenAPI{#openapis}
      + [概述](./apis/openapis/overview.md)
      + [如何设置](./apis/openapis/setup.md)
      + [服务器到服务器身份验证](./apis/openapis/use-cases/invoke-api-using-oauth-s2s.md)
      + [用户身份验证（Web 应用程序）](./apis/openapis/use-cases/invoke-api-using-oauth-web-app.md)
      + [用户身份验证（SPA）](./apis/openapis/use-cases/invoke-api-using-oauth-single-page-app.md)
      + 操作方式{#how-to}
         + [凭据和产品轮廓管理](./apis/openapis/how-to/credentials-and-product-profile-management.md)
         + [权限管理](./apis/openapis/how-to/services-user-group-permission-management.md)
+ 内容投放{#content-delivery}
   + [自定义域名](./content-delivery/custom-domain-names.md)
   + [具有 Adobe 托管的 CDN 的自定义域名](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [具有客户 CDN 的自定义域名](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [缓存](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [Adobe CDN - 超越缓存](./content-delivery/adobe-cdn-beyond-caching.md)
   + [自定义错误页面](./content-delivery/custom-error-pages.md)
   + [URL 重定向](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ 缓存{#caching}
   + [概述](./caching/overview.md)
   + [AEM Publish 服务](./caching/publish.md)
   + [AEM Author 服务](./caching/author.md)
   + [CDN 缓存命中率分析](./caching/cdn-cache-hit-ratio-analysis.md)
   + 操作方式{#how-to}
      + [启用缓存](./caching/how-to/enable-caching.md)
      + [禁用缓存](./caching/how-to/disable-caching.md)
      + [清除缓存](./caching/how-to/purge-cache.md)
+ 访问 AEM {#accessing}
   + [概述](./accessing/overview.md)
   + [Adobe IMS 用户](./accessing/adobe-ims-users.md)
   + [Adobe IMS 用户组](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS 产品轮廓](./accessing/adobe-ims-product-profiles.md)
   + [AEM 用户、群组和权限](./accessing/aem-users-groups-and-permissions.md)
   + [配置对 AEM 演练的访问权限](./accessing/walk-through.md)
+ 身份验证{#authentication}
   + [概述](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 高级网络{#networking}
   + [概述](./networking/advanced-networking.md)
   + [灵活的端口出口](./networking/flexible-port-egress.md)
   + [专用出口 IP 地址](./networking/dedicated-egress-ip-address.md)
   + [虚拟专用网络](./networking/vpn.md)
   + 代码示例{#examples}
      + [非标准端口上的 HTTP/HTTPS 以实现灵活的端口出站连接](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [用于专用出站 IP 地址/VPN 的 HTTP/HTTPS 连接](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [使用 DataSourcePool 的 SQL 连接](./networking/examples/sql-datasourcepool.md)
      + [使用 Java SQL API 的 SQL 连接](./networking/examples/sql-java-apis.md)
      + [电子邮件服务](./networking/examples/email-service.md)
+ 安全性 {#security}
   + [使用流量过滤规则阻止 DoS/DDoS 攻击](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + 流量过滤规则（包括 WAF 规则） {#traffic-filter-and-waf-rules}
      + [保护 AEM 网站](./security/traffic-filter-and-waf-rules/overview.md)
      + [如何设置](./security/traffic-filter-and-waf-rules/setup.md)
      + [使用流量过滤规则](./security/traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md)
      + [使用 WAF 规则](./security/traffic-filter-and-waf-rules/use-cases/using-waf-rules.md)
      + [最佳实践](./security/traffic-filter-and-waf-rules/best-practices.md)
      + 操作方式{#how-to}
         + [监控敏感请求](./security/traffic-filter-and-waf-rules/how-to/request-logging.md)
         + [限制访问](./security/traffic-filter-and-waf-rules/how-to/request-blocking.md)
         + [将请求标准化](./security/traffic-filter-and-waf-rules/how-to/request-transformation.md)
+ AEM 事件{#aem-eventing}
   + [概述](./eventing/overview.md)
   + 示例{#examples}
      + [Webhook - 接收 AEM 事件](./eventing/examples/webhook.md)
      + [日志 - 加载 AEM 事件](./eventing/examples/journaling.md)
      + [Adobe I/O Runtime 操作 - 接收 AEM 事件](./eventing/examples/runtime-action.md)
      + [Adobe I/O Runtime 操作 - 处理 AEM 事件](./eventing/examples/event-processing-using-runtime-action.md)
      + [AEM Assets 事件 - PIM 集成](./eventing/examples/assets-pim-integration.md)
+ 迁移 {#migration}
   + [内容传输工具](./migration/content-transfer-tool.md)
   + [批量导入资产](./migration/bulk-import.md)
   + 迁移到 AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [简介](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [加入](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA 和 CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM 现代化工具](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [存储库现代化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Asset Compute 微服务](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [搜索和索引](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + 内容迁移 {#content-migration}
         + [批量导入服务](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [内容传输工具](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [常见问题解答](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [疑难解答](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [简介](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [数字登记](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通信](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [简介](./migration/cloud-acceleration-manager/introduction.md)
      + [准备工作和 Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [实施阶段](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [代码重构工具 &#x200B;](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [代码存储库现代化器](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher 转换器](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [索引转换器](./migration/cloud-acceleration-manager/index-converter.md)
      + [资源工作流迁移工具](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [浏览 Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [使用 Cloud Acceleration Manager &#x200B;](./migration/cloud-acceleration-manager/using.md)
+ [内容片段](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + 为 Forms as a Cloud Service 进行开发 {#developing-for-cloud-service}
      + [1 - 快速入门 &#x200B;](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - 安装 IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - 设置 Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - 将 IntelliJ 与 AEM 同步](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - 生成表单](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - 自定义提交处理程序](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - 使用资源类型注册 servlet](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - 启用表单门户组件](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - 包括云服务和 FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 - 上下文感知云配置](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 - 推送到 Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 - 部署到开发环境](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 - 更新 Maven 原型](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + 创建自适应表单{#create-first-af}
      + [简介](./forms/create-first-af/introduction.md)
      + [创建主题](./forms/create-first-af/create-theme.md)
      + [创建模板](./forms/create-first-af/create-template.md)
      + [创建片段](./forms/create-first-af/create-fragments.md)
      + [创建表单](./forms/create-first-af/create-af.md)
      + [配置根面板](./forms/create-first-af/configure-root-panel.md)
      + [配置人员面板](./forms/create-first-af/configure-people-panel.md)
      + [配置收入面板](./forms/create-first-af/configure-income-panel.md)
      + [配置资产面板](./forms/create-first-af/configure-assets-panel.md)
      + [配置启动面板](./forms/create-first-af/configure-start-panel.md)
      + [添加和配置工具栏](./forms/create-first-af/add-configure-toolbar.md)
   + 使用 Headless 表单的自定义提交服务{#custom-submit-headless-forms}
      + [1 - 简介](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - 创建自定义提交服务](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - 显示响应](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + 创建地址区块组件{#create-address-block}
      + [1 - 简介](./forms/create-address-block-component/introduction.md)
      + [2 - 设置](./forms/create-address-block-component/set-up.md)
      + [3 - 创建组件](./forms/create-address-block-component/creating-address-component.md)
      + [4 - 部署组件](./forms/create-address-block-component/deploy-your-project.md)
   + 创建可点击的图像组件{#clickable-image-component}
      + [1 - 简介](./forms/clickable-image-component/introduction.md)
      + [2 - 创建组件](./forms/clickable-image-component/create-component.md)
      + [3 - 处理点击事件](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms 和 Analytics{#forms-and-analytics}
      + [简介](./forms/form-data-analytics/introduction.md)
      + [创建数据元素](./forms/form-data-analytics/data-elements.md)
      + [创建规则](./forms/form-data-analytics/rules.md)
      + [测试解决方案](./forms/form-data-analytics/test.md)
   + 创建国家/地区下拉组件{#countries-drop-down}
      + [简介](./forms/countries-drop-down/introduction.md)
      + [创建组件](./forms/countries-drop-down/component.md)
      + [创建对话框](./forms/countries-drop-down/dialog.md)
      + [创建 Sling 模型](./forms/countries-drop-down/slingmodel.md)
      + [构建和测试](./forms/countries-drop-down/build.md)
   + 创建按钮变体{#style-system}
      + [简介](./forms/style-system/introduction.md)
      + [定义策略](./forms/style-system/style-policy.md)
      + [定义变体](./forms/style-system/create-variations.md)
      + [测试变体](./forms/style-system/build.md)
   + 使用垂直选项卡{#using-vertical-tabs}
      + [&#x200B;1. 简介](./forms/using-vertical-tabs/introduction.md)
      + [&#x200B;2. 创建表单](./forms/using-vertical-tabs/create-af.md)
      + [&#x200B;3. 导航](./forms/using-vertical-tabs/navigation.md)
      + [&#x200B;4. 添加图标](./forms/using-vertical-tabs/icons.md)
   + 使用输出和表单服务{#forms-cs-output-and-forms-service}
      + [生成 PDF](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + 交互式通信教程{#interactive-communication-tutorial}
      + [&#x200B;1. 简介](./forms/interactive-communication-tutorial/introduction.md)
      + [2.创建FDM](./forms/interactive-communication-tutorial/create-form-data-model.md)
      + [3.创建模板](./forms/interactive-communication-tutorial/create-template.md)
      + [4.创建片段](./forms/interactive-communication-tutorial/create-fragments.md)
      + [5.创建IC文档](./forms/interactive-communication-tutorial/create-ic-document.md)
      + [6.生成IC文档](./forms/interactive-communication-tutorial/test-document-generation.md)
   + 在 AEM Forms CS 中生成文档{#doc-gen-formscs}
      + [简介](./forms/doc-gen-forms-cs/introduction.md)
      + [创建服务凭据](./forms/doc-gen-forms-cs/service-credentials.md)
      + [创建 JWT 令牌](./forms/doc-gen-forms-cs/create-jwt.md)
      + [创建访问令牌](./forms/doc-gen-forms-cs/create-access-token.md)
      + [将数据与模板合并](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [测试解决方案](./forms/doc-gen-forms-cs/test.md)
      + [挑战](./forms/doc-gen-forms-cs/challenge.md)
   + 使用表单文档服务 API{#forms-document-services-api}
      + [简介](./forms/forms-document-services/introduction.md)
      + [配置 OpenAPI](./forms/forms-document-services/using-open-api.md)
      + [生成访问令牌](./forms/forms-document-services/generate-access-token.md)
      + [应用使用权限](./forms/forms-document-services/make-api-calls.md)
      + [示例代码](./forms/forms-document-services/sample-project.md)
   + 使用批处理 API 生成文档{#formscs-batch-api}
      + [简介](./forms/formscs-batch-api/introduction.md)
      + [配置 Azure 存储](./forms/formscs-batch-api/configure-azure-storage.md)
      + [创建 USC 批处理配置](./forms/formscs-batch-api/configure-usc-batch.md)
      + [创建批处理配置](./forms/formscs-batch-api/create-batch-config.md)
      + [执行批处理](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Forms CS 中的 PDF 操作{#forms-cs-assembler}
      + [简介](./forms/forms-cs-assembler/introduction.md)
      + [创建服务凭据](./forms/forms-cs-assembler/service-credentials.md)
      + [创建 JWT 令牌](./forms/forms-cs-assembler/create-jwt.md)
      + [创建访问令牌](./forms/forms-cs-assembler/create-access-token.md)
      + [汇编 PDF 文件](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A 工具集](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [测试解决方案](./forms/forms-cs-assembler/test.md)
      + [挑战](./forms/forms-cs-assembler/challenge.md)
   + 使用 Blob 索引标记存储表单提交数据{#store-submiited-data-with-metadata-tags}
      + [简介](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [扩展选项组组件](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [创建 OSGi 配置](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [创建索引标记](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [创建自定义提交](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + 预填基于核心组件的表单{#prefill-core-component-based-form}
      + [简介](./forms/prefill-core-component-form/introduction.md)
      + [创作预填服务](./forms/prefill-core-component-form/pre-fill-service.md)
      + [测试解决方案](./forms/prefill-core-component-form/test-solution.md)
   + Azure 门户存储{#forms-cs-azure-portal}
      + [简介](./forms/forms-cs-azure-portal/introduction.md)
      + [创建表单数据模型](./forms/forms-cs-azure-portal/create-fdm.md)
      + [将表单数据存储在 Azure Storage 中](./forms/forms-cs-azure-portal/create-af.md)
      + [预填充表单](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [查询提交](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + 保存并继续填写表单{#prefill-azure-storage}
      + [1- 简介](./forms/prefill-azure-storage/introduction.md)
      + [2- 创建页面组件](./forms/prefill-azure-storage/page-component.md)
      + [3 - 创建自适应表单模板](./forms/prefill-azure-storage/associate-page-component.md)
      + [4- 创建 Azure 存储集成](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - 创建 SendGrid 集成](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - 创建自适应表单 &#x200B;](./forms/prefill-azure-storage/create-af.md)
      + [7 - 部署示例资产](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + 创建审核工作流程{#create-aem-workflow}
      + [外部化工作流存储](./forms/create-aem-workflow/externalize-workflow.md)
      + [创建工作流模型](./forms/create-aem-workflow/create-workflow.md)
      + [触发工作流](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign 与 AEM Forms{#forms-and-sign}
      + [简介](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API 应用程序](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign 云配置](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [创建自适应表单](./forms/forms-and-sign/create-adaptive-form.md)
      + [配置填写与签名功能](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + 与 Microsoft Power Automate 集成{#forms-cs-and-power-automate}
      + [配置集成](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [解析提交的表单数据](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [将 DoR 作为电子邮件附件发送](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [从提交的数据中提取表单附件](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + 与 Microsoft Dynamics 集成{#formscs-dynamics-crm}
      + [创建 Dynamics 应用程序](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [配置数据源](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [创建表单数据模型](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [创建自适应表单](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + 与 Salesforce 集成{#integrate-with-salesforce}
      + [简介](./forms/integrate-with-salesforce/introduction.md)
      + [创建连接的应用程序](./forms/integrate-with-salesforce/create-connected-app.md)
      + [创建 Swagger 文件](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [创建数据源](./forms/integrate-with-salesforce/create-data-source.md)
      + [创建表单数据模型](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [测试表单提交](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [测试点击事件](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + 将表单提交数据存储到 OneDrive 和 SharePoint{#one-drive}
      + [将表单数据存储到 OneDrive](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [将表单数据存储到 SharePoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [使用 SharePoint 列表中的数据预填充表单](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [通过工作流将数据插入 SharePoint 列表](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Asset Compute 可扩展性{#asset-compute}
   + [概述](./asset-compute/overview.md)
   + 设置{#set-up}
      + [帐户和服务配置](./asset-compute/set-up/accounts-and-services.md)
      + [本地开发环境](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + 开发{#develop}
      + [创建 Asset Compute 项目](./asset-compute/develop/project.md)
      + [配置环境变量](./asset-compute/develop/environment-variables.md)
      + [配置 manifest.yml](./asset-compute/develop/manifest.md)
      + [开发一个工作程序](./asset-compute/develop/worker.md)
      + [使用开发工具](./asset-compute/develop/development-tool.md)
   + 测试和调试{#test-debug}
      + [测试工作程序](./asset-compute/test-debug/test.md)
      + [调试工作程序](./asset-compute/test-debug/debug.md)
   + 部署{#deploy}
      + [部署到 Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [与 AEM 集成](./asset-compute/deploy/processing-profiles.md)
   + 高级{#advanced}
      + [元数据工作程序](./asset-compute/advanced/metadata.md)
   + [疑难解答](./asset-compute/troubleshooting.md)

+ 多步教程{#multi-step-tutorials}
   + [AEM Sites 开发](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html){target=_blank}
   + [SPA 编辑器 (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites 和 Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ 专家资源 {#expert-resources}
   + AEM 支持人员 {#aem-champions}
      + [Cloud Manager 入门手册](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Cloud Manager 环境类型](./expert-resources/aem-champions/environment-types.md)
      + [Cloud Manager UI](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM 专家系列](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [简介](./expert-resources/cloud-5/cloud5-introduction.md)
      + [第 4 季](./expert-resources/cloud-5/cloud5-season-4.md)
      + [第 3 季](./expert-resources/cloud-5/cloud5-season-3.md)
      + [第 2 季](./expert-resources/cloud-5/cloud5-season-2.md)
      + [第 1 季](./expert-resources/cloud-5/cloud5-season-1.md)
      + [AEM CDN 第 1 部分](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN 第 2 部分](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM 日志文件](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [登录令牌](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [迁移 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Dispatcher 验证器](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [搜索和索引](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + 第 2 季{#season-2}
         + [片段](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [存储库现代化工具](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Sling 作业调度程序](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [修复缓存](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [修复重写规则问题](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager - 体验审核](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager - 单元测试](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager - 功能测试](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + 第 3 季{#season-3}
         + [第三方搜索](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [边缘处理程序](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [在 Edge Delivery Services 中发布、取消发布事件](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [查询索引和 Excel 公式](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [自带 Cloudflare CDN](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [集成 AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [适用于 AEM Sites 的生成式 AI](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [探索通用编辑器](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [导入网站](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [使用管理员 API](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [Lighthouse 评分优化 - 第 1 部分](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [Lighthouse 评分优化 - 第 2 部分](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [Lighthouse 评分优化 - 第 3 部分](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)
      + 第 4 季{#season-4}
         + [最佳实践](./expert-resources/cloud-5/season-4/cloud5-edge-delivery-services-best-practices.md)
         + [搜索优化](./expert-resources/cloud-5/season-4/cloud5-search-optimization.md)
         + [Google 地图](./expert-resources/cloud-5/season-4/cloud5-google-maps.md)
