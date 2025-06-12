---
title: 创建 AEM 网站
description: 在 AEM Sites 中为 Edge Delivery Services 创建一个网站，该网站可通过通用编辑器进行编辑。
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
workflow-type: ht
source-wordcount: '302'
ht-degree: 100%

---

# 创建 AEM 网站

AEM 网站是编辑、管理和发布网站内容的地方。要创建通过 Edge Delivery Services 投放且使用通用编辑器创作的 AEM 网站，请使用[具有 AEM Author 功能的 Edge Delivery Services 网站模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)在 AEM Author上 创建新的网站。

AEM 网站是存储和创作网站内容的地方。最终体验是 AEM 网站内容与[网站代码](./1-new-code-project.md)的结合。

![Edge Delivery Services 和通用编辑器的新 AEM 网站](./assets/2-new-aem-site/new-site.png)

按照[文档中列出的详细步骤](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site)来创建新的 AEM 网站。  以下是这些步骤的汇总列表，包括本教程中使用的值。
1. 在 AEM Author 中&#x200B;**创建新的网站**。本教程使用以下网站命名：
   * 网站标题：`WKND (Universal Editor)`
   * 网站名称：`aem-wknd-eds-ue`

      * 网站名称值必须与添加到 `paths.json`](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping) 中的网站路径名称[相匹配。

2. 从[具有 AEM Author 功能的 Edge Delivery Services 网站模板](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)中&#x200B;**导入最新模板**。
3. 将&#x200B;**网站命名**&#x200B;为与 GitHub 存储库名称一致，并将 GitHub URL 设置为该存储库的 URL。

## 发布新网站以进行预览

在 AEM Author 中创建网站后，将其发布到 Edge Delivery Services 的预览环境，使内容可用于[本地开发环境](./3-local-development-environment.md)。

1. 登录 **AEM Author** 并导航至&#x200B;**网站**。
2. 选择&#x200B;**新网站** (`WKND (Universal Editor)`) 并点击&#x200B;**管理发布**。
3. 在&#x200B;**目标环境**&#x200B;下选择&#x200B;**预览**，然后点击&#x200B;**下一步**。
4. 在&#x200B;**包含子项设置**&#x200B;下，选择&#x200B;**包含子项**，取消选择其他选项，然后点击&#x200B;**确定**。
5. 点击&#x200B;**发布**，将网站内容发布到预览环境。
6. 一旦发布到预览环境，页面将会在 Edge Delivery Services 的预览环境中可用（这些页面不会出现在 AEM Preview 服务中）。
