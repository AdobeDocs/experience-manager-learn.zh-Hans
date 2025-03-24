---
title: 创建AEM站点
description: 在AEM Sites中为Edge Delivery Services创建一个站点，该站点可使用通用编辑器进行编辑。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# 创建AEM站点

AEM站点是编辑、管理和发布网站内容的地方。 要创建通过Edge Delivery Services交付并使用Universal Editor创作的AEM站点，请将[Edge Delivery Services与AEM创作站点模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)一起使用，以在AEM Author上创建新站点。

AEM站点是存储和创作网站内容的地方。 最终体验是AEM网站内容与[网站代码](./1-new-code-project.md)的组合。

![适用于Edge Delivery Services和通用编辑器的新AEM站点](./assets/2-new-aem-site/new-site.png)

按照文档](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site)中概述的[详细步骤创建新的AEM站点。  以下是这些步骤的汇总列表，包括本教程中使用的值。
1. **在AEM创作中创建新站点**。 本教程使用以下站点命名：
   * 站点标题： `WKND (Universal Editor)`
   * 站点名称： `aem-wknd-eds-ue`

      * 网站名称值必须与添加到`paths.json`](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping)的网站路径名称[匹配。

2. 使用AEM创作站点模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)从[Edge Delivery Services中导入最新的模板&#x200B;**。**
3. **命名站点**&#x200B;以匹配GitHub存储库名称，并将GitHub URL设置为存储库的URL。

## 发布新站点以进行预览

在AEM Author中创建站点后，将其发布到Edge Delivery Services预览，以便内容可供[本地开发环境](./3-local-development-environment.md)使用。

1. 登录到&#x200B;**AEM作者**&#x200B;并导航到&#x200B;**站点**。
2. 选择&#x200B;**新站点** (`WKND (Universal Editor)`)，然后单击&#x200B;**管理发布**。
3. 在&#x200B;**目标**&#x200B;下选择&#x200B;**预览**，然后单击&#x200B;**下一步**。
4. 在&#x200B;**包括子项设置**&#x200B;下，选择&#x200B;**包括子项**，取消选择其他选项，然后单击&#x200B;**确定**。
5. 单击&#x200B;**发布**&#x200B;发布要预览的站点内容。
6. 发布到预览环境后，这些页面将在Edge Delivery Services预览环境中可用(这些页面将不会显示在AEM预览服务中)。
