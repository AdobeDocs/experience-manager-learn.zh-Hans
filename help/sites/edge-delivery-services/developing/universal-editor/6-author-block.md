---
title: 创作区块
description: 使用通用编辑器创作 Edge Delivery Services 区块。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '376'
ht-degree: 100%

---

# 创作区块

将 [Teaser 区块的 JSON](./5-new-block.md) 推送到 `teaser` 分支后，该区块即可在 AEM 通用编辑器中进行编辑。

在开发中创作区块非常重要，原因如下：

1. 它验证了区块的定义和模型是否准确。
1. 它允许开发人员审查区块的语义 HTML，作为开发的基础。
1. 它可以将内容和语义 HTML 部署到预览环境中，以加快区块开发的速度。

## 使用来自 `teaser` 分支的代码打开通用编辑器

1. 登录到 AEM Author。
2. 导航到&#x200B;**网站**，并选择在[上一章](./2-new-aem-site.md)中创建的网站（WKND（通用编辑器））。

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. 创建或编辑页面，以添加新的区块，并确保上下文可用于支持本地开发。虽然可以在网站内的任何位置创建页面，但通常最好为每个新作品创建独立的页面。创建一个名为 **Branches** 的新“文件夹”页面。每个子页面用于支持同名 Git 分支的开发。

   ![AEM Sites - 创建分支页面](./assets/6-author-block/branches-page-3.png)

4. 在&#x200B;**分支**&#x200B;页面下，创建一个名为 **Teaser** 的新页面，并与开发分支的名称相匹配，然后点击&#x200B;**打开**&#x200B;以编辑该页面。

   ![AEM Sites - 创建 Teaser 页面](./assets/6-author-block/teaser-page-3.png)

5. 通过在 URL 中添加 `?ref=teaser` 来更新通用编辑器，以便从 `teaser` 分支中加载代码。确保在 `#` 符号&#x200B;**之前**&#x200B;添加查询参数。

   ![通用编辑器 - 选择 Teaser 分支](./assets/6-author-block/select-branch.png)

6. 选择&#x200B;**主要**&#x200B;下的第一个部分，点击&#x200B;**添加**&#x200B;按钮，然后选择 **Teaser** 区块。

   ![通用编辑器 - 添加区块](./assets/6-author-block/add-teaser-2.png)

7. 在画布上，选择新添加的 Teaser 并在右侧创作字段，或通过内联编辑功能进行创作。

   ![通用编辑器 - 创作区块](./assets/6-author-block/author-block.png)

8. 完成创作后，选择通用编辑器右上角的&#x200B;**发布**&#x200B;按钮，选择发布到&#x200B;**预览**&#x200B;环境，从而将更改发布到预览环境。然后将这些更改发布到该网站的 `aem.page` 域。
   ![AEM 网站 - 发布或预览](./assets/6-author-block/publish-to-preview.png)

9. 等待更改发布到预览环境，然后通过 [AEM CLI](./3-local-development-environment.md#install-the-aem-cli) 在 [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser) 打开网页。

   ![本地网站 - 刷新](./assets/6-author-block/preview.png)

现在，创作的 Teaser 区块的内容和语义 HTML 可在预览网站上获取，并可在本地开发环境中使用 AEM CLI 进行开发。
