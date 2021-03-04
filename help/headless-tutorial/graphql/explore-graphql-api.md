---
title: 浏览GraphQL API - AEM无外设入门 — GraphQL
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 使用内置的GrapiQL IDE浏览AEM GraphQL API。 了解AEM如何根据内容片段模型自动生成GraphQL模式。 尝试使用GraphQL语法构建基本查询。
sub-product: 资产
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: '"内容片段， GraphQL API"'
topic: “无头、内容管理”
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 0%

---


# 浏览GraphQL API {#explore-graphql-apis}

AEM的GraphQL API提供了一种功能强大的查询语言，可向下游应用程序公开内容片段的数据。 内容片段模型定义内容片段使用的数据模式。 每当创建或更新内容片段模型时，模式都会被转换并添加到构成GraphQL API的“图形”中。

在本章中，我们将探索一些常见的GraphQL查询，以使用称为[GraphiQL](https://github.com/graphql/graphiql)的IDE收集内容。 GraphiQL IDE允许您快速测试和优化返回的查询和数据。 GraphiQL还提供了对文档的轻松访问，使您能轻松了解和了解可用的方法。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定[创作内容片段](./author-content-fragments.md)中概述的步骤已完成。

## 目标{#objectives}

* 了解如何使用GraphQL工具使用GraphQL语法构建查询。
* 了解如何查询内容片段和单个内容片段的列表。
* 了解如何过滤和请求特定数据属性。
* 了解如何查询内容片段的变体。
* 了解如何加入多个内容片段模型的查询

## 安装GraphiQL工具{#install-graphiql}

GraphiQL IDE是一个开发工具，只需在较低级别环境（如开发或本地实例）上使用。 因此，它不包含在AEM项目中，而是作为可以临时安装的单独包提供。

1. 导航到&#x200B;**[软件分发门户](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM作为Cloud Service**。
1. 搜索“GraphiQL”(请确保在&#x200B;**GraphiQL**&#x200B;中包含&#x200B;**i**。
1. 下载最新的&#x200B;**GraphiQL内容包v.x.x.x**

   ![下载GraphiQL包](assets/explore-graphql-api/software-distribution.png)

   zip文件是可直接安装的AEM包。

1. 从&#x200B;**AEM 开始**&#x200B;菜单导航到&#x200B;**工具** > **部署** > **软件包**。
1. 单击&#x200B;**上载包**，然后选择在上一步中下载的包。 单击&#x200B;**安装**&#x200B;以安装软件包。

   ![安装GraphiQL包](assets/explore-graphql-api/install-graphiql-package.png)

## 查询内容片段的列表{#query-list-cf}

一个常见要求是查询多个内容片段。

1. 导航到位于[http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)的GraphiQL IDE。
1. 将以下查询粘贴到左面板(注释列表下):

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. 按顶部菜单中的&#x200B;**播放**&#x200B;按钮以执行查询。 您应当看到上一章中参与者内容片段的结果：

   ![参与者列表结果](assets/explore-graphql-api/contributorlist-results.png)

1. 将光标置于`_path`文本下方，然后输入&#x200B;**CTRL+Space**&#x200B;以触发代码提示。 将`fullName`和`occupation`添加到查询。

   ![使用代码隐藏更新查询](assets/explore-graphql-api/update-query-codehinting.png)

1. 通过按&#x200B;**播放**&#x200B;按钮再次执行查询，结果应包括`fullName`和`occupation`的其他属性。

   ![全名和职业成果](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` 属 `occupation` 性简单。从[定义内容片段模型](./content-fragment-models.md)一章中回想，`fullName`和`occupation`是定义相应字段的&#x200B;**属性名称**&#x200B;时使用的值。

1. `pictureReference` 和表 `biographyText` 示更复杂的字段。使用以下更新查询以返回有关`pictureReference`和`biographyText`字段的数据。

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biographyText {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biographyText` 是多行文本字段，GraphQL API允许我们为结果选择各种格式，如 `html`、 `markdown` `json` 或 `plaintext`。

   `pictureReference` 是内容引用，它应为图像，因此使用 `ImageRef` 内置对象。这允许我们请求有关正在引用的图像的其他数据，如`width`和`height`。

1. 接下来，尝试查询&#x200B;**Adventures**&#x200B;的列表。 执行以下查询:

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   您应当看到返回的列表&#x200B;**Adventures**。 通过向查询添加其他字段，尽情体验。

## 筛选内容片段列表{#filter-list-cf}

接下来，让我们看看如何根据属性值将结果筛选为内容片段的子集。

1. 在GraphiQL UI中输入以下查询:

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   以上查询会对系统中的所有参与者执行搜索。 添加到查询开头的过滤器将对`occupation`字段和字符串&quot;**Photographer**&quot;执行比较。

1. 执行查询，应该只返回一个&#x200B;**Contributor**。
1. 输入以下查询,查询&#x200B;**Adventures**&#x200B;的列表，其中`adventureActivity`为&#x200B;**not**&#x200B;等于&#x200B;**&quot;Surfing&quot;**:

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. 执行查询并检查结果。 请注意，所有结果中均不包含等于&#x200B;**&quot;Surfing&quot;**&#x200B;的`adventureType`。

过滤和创建复杂查询有许多其他选项，以上只是几个示例。

## 查询单个内容片段{#query-single-cf}

也可以直接查询单个内容片段。 AEM中的内容以分层方式存储，片段的唯一标识符基于片段的路径。 如果目标是返回有关单个片段的数据，则最好使用路径并直接查询模型。 使用此语法意味着查询复杂性将非常低，并且会生成更快的结果。

1. 在GraphiQL编辑器中输入以下查询:

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

1. 执行查询并观察&#x200B;**Stacey Roswells**&#x200B;片段的单个结果已返回。

   在上一练习中，您使用过滤器缩小结果列表。 您可以使用类似的语法按路径过滤，但出于性能原因，上述语法是首选的。

1. 在[创作内容片段](./author-content-fragments.md)章中，请回想一下，为&#x200B;**Stacey Roswells**&#x200B;创建了&#x200B;**Summary**&#x200B;变量。 更新查询以返回&#x200B;**摘要**&#x200B;变量：

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

   即使变量名为&#x200B;**Summary**，变量仍以小写形式保留，因此使用`summary`。

1. 执行查询并观察`biography`字段包含的`html`结果要短得多。

## 查询多个内容片段模型{#query-multiple-models}

还可以将单独的查询合并为单个查询。 这对于最大限度地减少为应用程序供电所需的HTTP请求数很有用。 例如，应用程序的&#x200B;*Home*&#x200B;视图可以根据&#x200B;**两个**&#x200B;不同的内容片段模型显示内容。 我们可以将这些查询合并为单个请求，而不是执行&#x200B;**两个**&#x200B;单独的查询。

1. 在GraphiQL编辑器中输入以下查询:

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. 执行查询并观察结果集包含来自&#x200B;**Adventures**&#x200B;和&#x200B;**Contributors**&#x200B;的数据：

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## 其他资源

有关GraphQL查询的更多示例，请参阅：[了解如何将GraphQL与AEM一起使用 — 示例内容和查询](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html)。

## 恭喜！{#congratulations}

恭喜您，您刚刚创建并执行了多个GraphQL查询!

## 后续步骤{#next-steps}

在下一章[从React app](./graphql-and-external-app.md)查询AEM中，您将了解外部应用程序如何查询AEM GraphQL端点。 修改示例WKND GraphQL React应用程序以添加筛选GraphQL查询的外部应用程序，允许应用程序的用户按活动筛选冒险。 还将向您介绍一些基本的错误处理。
