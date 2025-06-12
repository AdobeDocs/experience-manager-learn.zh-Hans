---
title: '创建代码项目 '
description: 为 Edge Delivery Services 创建一个代码项目，该项目可通过通用编辑器进行编辑。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# 创建一个 Edge Delivery Services 代码项目

要为 Edge Delivery Services 和通用编辑器构建 AEM 网站，请使用 Adobe的 [AEM Boilerplate XWalk 项目模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)。此模板可创建一个新的代码项目，其中包含用于创建网站体验的 CSS 和 JavaScript。此模板可创建一个新的 GitHub 存储库，并为其加载 Adobe 的样板代码和配置，为您的 AEM 网站项目奠定坚实基础。

请记住，由 Edge Delivery Services 提供的 [AEM 网站](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/sites/edge-delivery-services/overview)仅包含客户端（浏览器）代码。在 AEM Author 或 Publish 服务中，不会执行网站代码。

![新的 Edge Delivery Services 项目](./assets/1-new-project/new-project.png)

按照[文档中列出的详细步骤](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)，创建可在通用编辑器中编辑内容的 Edge Delivery Services 代码项目。  以下是这些步骤的汇总列表，包括本教程中使用的值。

1. **设置一个 GitHub 帐户。** 如果您正在为您的组织创建一个项目，请确保该组织拥有一个 GitHub 账户，并且您是该账户的成员。
2. 使用 [AEM Boilerplate XWalk 项目模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)**创建一个新的代码项目**。
3. **安装 AEM Code Sync GitHub 应用程序**，并授予对存储库的访问权限。您可以在[此处](https://github.com/apps/aem-code-sync)找到该应用程序。
4. **配置您的新项目的`fstab.yaml`**，使其指向正确的 AEM Author 服务。

   * 为了进行实验，您可以将较低版本的 AEM as a Cloud Service 环境（测试或开发环境），但实际的网站实施应配置为使用生产版 AEM 服务。

5. **编辑您新项目的`paths.json`**，将 AEM Author 服务路径映射到您网站的根目录。

这个 Git 存储库是在[本地开发环境](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment)章节中克隆的，代码就是在这里开发的。
