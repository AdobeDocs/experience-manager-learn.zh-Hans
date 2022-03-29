---
title: 用于调试AEM SDK的其他工具
description: 其他各种工具可以帮助调试AEM SDK的本地快速启动。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 1%

---

# 用于调试AEM SDK的其他工具

其他各种工具可帮助在AEM SDK的本地快速启动中调试应用程序。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite是基于Web的界面，用于与JCR AEM数据存储库进行交互。 CRXDE Lite提供对JCR的完全可见性，包括节点、属性、属性值和权限。

CRXDE Lite位于：

+ 工具>常规>CRXDE Lite
+ 或直接在 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### 调试内容

CRXDE Lite提供对JCR的直接访问。 通过CRXDE Lite显示的内容受授予用户的权限的限制，这意味着您可能无法查看或修改JCR中的所有内容，具体取决于您的访问权限。

+ 使用左侧导航窗格导航和操作JCR结构
+ 在左侧导航窗格中选择节点时，会在底部窗格中显示节点属性。
   + 可以从窗格中添加、删除或更改属性
+ 双击左侧导航中的文件节点，在右上方窗格中打开文件的内容
+ 点按左上角的全部保存按钮以保留已更改，或点按全部保存旁边的向下箭头以还原任何未保存的更改。

![CRXDE Lite — 调试内容](./assets/other-tools/crxde-lite__debugging-content.png)

通过CRXDE Lite直接对AEM SDK所做的任何更改都可能难以跟踪和管理。 根据需要，确保通过CRXDE Lite所做的更改可返回到AEM项目的可变内容包(`ui.content`)并承诺使用Git。 理想情况下，所有应用程序内容更改都源自代码库，并通过部署流入AEM SDK，而不是通过CRXDE Lite直接对AEM SDK进行更改。

### 调试访问控制

CRXDE Lite提供了一种方法来测试和评估特定用户或组（即主体）在特定节点上的访问控制。

要在CRXDE Lite中访问测试访问控制控制台，请导航到：

+ CRXDE Lite>工具>测试访问控制……

![CRXDE Lite — 测试访问控制](./assets/other-tools/crxde-lite__test-access-control.png)

1. 使用“路径”字段，选择要评估的JCR路径
1. 使用“主体”(Principal)字段，选择用户或组以根据
1. 点按测试按钮

结果显示如下：

+ __路径__ 重申评估的路径
+ __主体__ 重申对路径进行评估的用户或组
+ __承担者__ 列出所选主体所属的所有主体。
   + 这有助于了解可通过继承提供权限的传递组成员关系
+ __路径权限__ 列出所选主体在评估路径上拥有的所有JCR权限

## 说明查询

![说明查询](./assets/other-tools/explain-query.png)

解释AEM SDK本地快速入门中基于Web的查询工具，该工具提供了AEM如何解释和执行查询的关键分析，并且是一款非常宝贵的工具，可确保AEM以性能方式执行查询。

解释查询位于：

+ “工具”>“诊断”>“查询性能”>“说明查询”选项卡
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) >“说明查询”选项卡

## QueryBuilder Debugger

![QueryBuilder Debugger](./assets/other-tools/query-debugger.png)

QueryBuilder调试器是一款基于Web的工具，可帮助您使用AEM调试和了解搜索查询 [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 语法。

QueryBuilder Debugger位于：

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
