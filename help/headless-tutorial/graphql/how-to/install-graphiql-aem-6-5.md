---
title: 在AEM 6.5上安装GraphiQL IDE
description: 了解如何在AEM 6.5上安装和配置GraphiQL IDE
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 17%

---


# 在AEM 6.5上安装GraphiQL IDE

在AEM 6.5中，必须手动安装GraphiQL IDE工具。

1. 导航到&#x200B;**[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**。
1. 搜索“GraphiQL”(请务必包含 **i** in **GraphiQL**)。
1. 下载最新版本 **GraphiQL内容包v.x.x.x**.

   ![下载GraphiQL包](assets/graphiql/software-distribution.png)

   zip文件是可直接安装的AEM包。

1. 从AEM开始菜单中，导航到 **工具** > **部署** > **包**.
1. 单击&#x200B;**上传软件包**，然后选择在之前步骤中下载的软件包。单击&#x200B;**安装**&#x200B;可安装软件包。

   ![安装GraphiQL包](assets/graphiql/install-graphiql-package.png)

1. 导航到 **CRXDE Lite** > **存储库面板** >选择 `/content/graphiql` 节点(例如， <http://localhost:4502/crx/de/index.jsp#/content/graphiql>)。
1. 在 **属性** 选项卡更改值 `endpoint` 属性 `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![端点属性值更改](assets/graphiql/endpoint-prop-value-change.png)

1. 导航到 **Web控制台配置** UI >搜索 **CSRF滤波器** 配置(例如，<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. 在 `Excluded Paths` 属性名称字段更新， WKND GraphQL端点路径 `/content/cq:graphql/wknd-shared/endpoint`.

![Exclude Paths属性值更改](assets/graphiql/exclude-paths-value-change.png)

1. 使用访问GraphiQL编辑器 `//HOST:PORT/content/graphiql.html`，并验证您是否可以构建新查询或执行现有查询。 (例如 <http://localhost:4502/content/graphiql.html>)

![GraphiQL编辑器](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>要支持特定于项目的GraphQL架构和查询执行，您必须对 `endpoint` 和 `Excluded Paths` 值。
