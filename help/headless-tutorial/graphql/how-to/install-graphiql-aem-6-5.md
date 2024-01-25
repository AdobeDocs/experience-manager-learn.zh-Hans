---
title: 在AEM 6.5上安装GraphiQL IDE
description: 了解如何在AEM 6.5上安装和配置GraphiQL IDE
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 55
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 14%

---

# 在AEM 6.5上安装GraphiQL IDE

在AEM 6.5中，必须手动安装GraphiQL IDE工具。

1. 导航到&#x200B;**[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**。
1. 搜索“GraphiQL”(确保包括 **i** 在 **GraphiQL**)。
1. 下载最新版本 **GraphiQL内容包v.x.x.x**.

   ![下载GraphiQL包](assets/graphiql/software-distribution.png)

   zip文件是可以直接安装的AEM包。

1. 从AEM的“开始”菜单中，导航到 **工具** > **部署** > **包**.
1. 单击&#x200B;**上传软件包**，然后选择在之前步骤中下载的软件包。单击&#x200B;**安装**&#x200B;可安装软件包。

   ![安装GraphiQL包](assets/graphiql/install-graphiql-package.png)

1. 导航到 **CRXDE Lite** > **存储库面板** >选择 `/content/graphiql` 节点(例如， <http://localhost:4502/crx/de/index.jsp#/content/graphiql>)。
1. 在 **属性** 选项卡更改值 `endpoint` 属性至 `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![终结点属性值更改](assets/graphiql/endpoint-prop-value-change.png)

1. 导航至 **Web控制台配置** UI >搜索 **CSRF筛选器** 配置(例如，<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. 在 `Excluded Paths` 属性名称字段更新，WKND GraphQL端点路径 `/content/cq:graphql/wknd-shared/endpoint`.

![Exclude Paths属性值更改](assets/graphiql/exclude-paths-value-change.png)

1. 使用以下方式访问GraphiQL编辑器 `//HOST:PORT/content/graphiql.html`，并验证您是否可以构造新查询或执行现有查询。 (例如 <http://localhost:4502/content/graphiql.html>)

![GraphiQL编辑器](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>要支持特定于项目的GraphQL架构和查询执行，您必须对进行相应的更改 `endpoint` 和 `Excluded Paths` 中的值。
