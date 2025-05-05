---
title: 创建代码项目
description: 为Edge Delivery Services创建一个代码项目，该代码项目可以使用通用编辑器进行编辑。
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
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# 创建Edge Delivery Services代码项目

要为Edge Delivery Services和通用编辑器构建AEM网站，请使用Adobe的[AEM Boilerplate XWalk项目模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)。 此模板将创建一个新代码项目，该项目包含用于创建网站体验的CSS和JavaScript。 此模板将创建一个新的GitHub存储库，并将其与Adobe的样板代码和配置一起加载，为您的AEM网站项目奠定坚实的基础。

请记住，由Edge Delivery Services交付的[AEM网站](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview)仅具有客户端（浏览器）代码。 网站代码不会在AEM创作或发布服务中执行。

![新Edge Delivery Services项目](./assets/1-new-project/new-project.png)

按照文档[&#128279;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)中概述的详细步骤创建内容可在通用编辑器中编辑的Edge Delivery Services代码项目。  以下是这些步骤的汇总列表，包括本教程中使用的值。

1. **设置GitHub帐户。**&#x200B;如果您在为组织创建项目，请确保组织具有GitHub帐户，并且您是成员。
2. **使用[AEM样板XWalk项目模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk)创建新的代码项目**。
3. **安装AEM代码同步GitHub应用程序**&#x200B;并授予对存储库的访问权限。 您可以在此处找到[应用](https://github.com/apps/aem-code-sync)。
4. **将新项目的`fstab.yaml`**&#x200B;配置为指向正确的AEM创作服务。

   * 要进行测试，您可以使用较低的AEM as a Cloud Service环境（暂存或开发），但应将真正的网站实施配置为使用生产AEM服务。

5. **编辑新项目的`paths.json`**&#x200B;以将AEM Author服务路径映射到您网站的根目录。

此Git存储库在[本地开发环境](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment)章节中克隆，并在其中开发代码。
