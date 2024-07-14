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
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 14%

---

# 在AEM 6.5上安装GraphiQL IDE

在AEM 6.5中，必须手动安装GraphiQL IDE工具。

1. 导航到&#x200B;**[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**。
1. 搜索“GraphiQL”（请确保在&#x200B;**GraphiQL**&#x200B;中包含&#x200B;**i**）。
1. 下载最新的&#x200B;**GraphiQL Content Package v.x.x.x**。

   ![下载GraphiQL包](assets/graphiql/software-distribution.png)

   zip文件是可以直接安装的AEM包。

1. 从AEM“开始”菜单中，导航到&#x200B;**工具** > **部署** > **包**。
1. 单击&#x200B;**上传软件包**，然后选择在之前步骤中下载的软件包。单击&#x200B;**安装**&#x200B;可安装软件包。

   ![安装GraphiQL包](assets/graphiql/install-graphiql-package.png)

1. 导航到&#x200B;**CRXDE Lite** > **存储库面板** >选择`/content/graphiql`节点（例如，<http://localhost:4502/crx/de/index.jsp#/content/graphiql>）。
1. 在&#x200B;**属性**&#x200B;选项卡中，将`endpoint`属性的值更改为`/content/_cq_graphql/wknd-shared/endpoint.json`。
   ![终结点属性值更改](assets/graphiql/endpoint-prop-value-change.png)

1. 导航到&#x200B;**Web控制台配置**&#x200B;用户界面>搜索&#x200B;**CSRF筛选器**&#x200B;配置（例如，<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>）
1. 在`Excluded Paths`属性名称字段更新中，WKND GraphQL终结点路径为`/content/cq:graphql/wknd-shared/endpoint`。

![排除路径属性值更改](assets/graphiql/exclude-paths-value-change.png)

1. 使用`//HOST:PORT/content/graphiql.html`访问GraphiQL编辑器，并确认您可以构造新查询或执行现有查询。 （如<http://localhost:4502/content/graphiql.html>）

![GraphiQL编辑器](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>要支持特定于项目的GraphQL架构和查询执行，您必须对上述步骤中的`endpoint`和`Excluded Paths`值进行相应的更改。
