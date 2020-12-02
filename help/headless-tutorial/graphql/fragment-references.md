---
title: 利用片段引用进行高级数据建模-AEM无头快速入门- GraphQL
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 了解如何使用片段引用功能进行高级数据建模以及创建两个不同内容片段之间的关系。 了解如何修改GraphQL查询以包含引用模型中的字段。
sub-product: 资产
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: null
thumbnail: null
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 1%

---


# 使用片段引用进行高级数据建模

>[!CAUTION]
>
> 针对内容片段投放的AEM GraphQL API将于2021年初发布。
> 相关文档可供预览使用。

可以从其他内容片段中引用内容片段。 这使用户能够构建具有片段之间关系的复杂数据模型。

在本章中，您将使用&#x200B;**片段引用**&#x200B;字段更新冒险模型以包含对参与者模型的引用。 您还将学习如何修改GraphQL查询以包含引用模型中的字段。

## 前提条件

这是一个多部分教程，假定已完成前面部分中概述的步骤。

## 目标

本章将学习如何：

* 更新内容片段模型以使用片段引用字段
* 创建一个GraphQL查询，它返回引用模型中的字段

## 添加片段引用{#add-fragment-reference}

更新冒险内容片段模型以添加对参与者模型的引用。

1. 打开新浏览器并导航到AEM。
1. 从&#x200B;**AEM开始**&#x200B;菜单导航到&#x200B;**工具** > **资产** **内容片段模型** > **WKND站点**。
1. 打开&#x200B;**Adventure**&#x200B;内容片段模型

   ![打开冒险内容片段模型](assets/fragment-references/adventure-content-fragment-edit.png)

1. 在&#x200B;**数据类型**&#x200B;下，将&#x200B;**片段引用**&#x200B;字段拖放到主面板中。

   ![添加片段引用字段](assets/fragment-references/add-fragment-reference-field.png)

1. 使用以下命令更新此字段的&#x200B;**属性**:

   * 呈现为 - `fragmentreference`
   * 字段标签- **Adventure Contributor**
   * 属性名称 - `adventureContributor`
   * 型号类型——选择&#x200B;**参与者**&#x200B;型号
   * 根路径 - `/content/dam/wknd`

   ![片段引用属性](assets/fragment-references/fragment-reference-properties.png)

   属性名称`adventureContributor`现在可用于引用参与者内容片段。

1. 保存对模型的更改。

## 将参与者分配到冒险

现在，冒险内容片段模型已更新，我们可以编辑现有片段并引用参与者。 应当注意，编辑内容片段模型&#x200B;*会影响*&#x200B;任何从其创建的现有内容片段。

1. 导航至&#x200B;**资产**>**文件**>**WKND站点****英语****冒险**>**[巴厘岛冲浪营地](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)&lt;a11/>2/>。**

   ![巴厘岛冲浪营文件夹](assets/setup/bali-surf-camp-folder.png)

1. 单击&#x200B;**Bali Surf Camp**&#x200B;内容片段以打开内容片段编辑器。
1. 更新&#x200B;**Adventure Contributor**&#x200B;字段，并单击文件夹图标选择参与者。

   ![选择Stacey Roswells作为投稿人](assets/fragment-references/stacey-roswell-contributor.png)

   *选择参与者片段的路径*

   ![contributor的填充路径](assets/fragment-references/populated-path.png)

   请注意，只能选择使用&#x200B;**Contributor**&#x200B;模型创建的片段。

1. 保存对片段所做的更改。

1. 重复上述步骤，为[Yosemite Backpacking](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking)和[Colorado Rock Climbing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)等冒险活动分配参与者

## 查询带有GraphiQL的嵌套内容片段

然后，对冒险执行查询，并添加引用的参与者模型的嵌套属性。 我们将使用GraphiQL工具快速验证查询的语法。

1. 导航到AEM中的GraphiQL工具：[http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. 输入以下查询:

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   上述查询只针对一条路径的冒险。 `adventureContributor`属性引用参与者模型，然后我们可以从嵌套内容片段请求属性。

1. 执行查询，您应得到如下结果：

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. 尝试其他查询，如`adventureList`，并在`adventureContributor`下添加引用内容片段的属性。

## 更新React应用程序以显示参与者内容

接下来，更新React Application使用的查询，以包含新的Contributor并显示有关Contributor的信息，作为Adventure详细信息视图的一部分。

1. 在IDE中打开WKND GraphQL React应用程序。

1. 打开文件`src/components/AdventureDetail.js`。

   ![Adventure Detail组件IDE](assets/fragment-references/adventure-detail-ide.png)

1. 查找函数`adventureDetailQuery(_path)`。 `adventureDetailQuery(..)`函数只需包装一个过滤GraphQL查询，它使用AEM `<modelName>ByPath`语法来查询由其JCR路径标识的单个内容片段。

1. 更新查询以包含有关引用的参与者的信息：

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
           item {
               _path
               adventureTitle
               adventureActivity
               adventureType
               adventurePrice
               adventureTripLength
               adventureGroupSize
               adventureDifficulty
               adventurePrice
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   通过此更新，有关`adventureContributor`、`fullName`、`occupation`和`pictureReference`的其他属性将包含在查询中。

1. Inspect嵌入`AdventureDetail.js`文件`function Contributor(...)`的`Contributor`组件。 如果属性存在，此组件将呈现参与者的名称、职位和图片。

   `Contributor`组件在`AdventureDetail(...)` `return`方法中引用：

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. 保存对文件所做的更改。
1. 开始React App（如果尚未运行）:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. 导航到[http://localhost:3000](http://localhost:3000/)并单击具有引用的参与者的冒险。 您现在应当看到&#x200B;**Interinal**&#x200B;下面列出的参与者信息：

   ![应用程序中添加的参与者](assets/fragment-references/contributor-added-detail.png)


## 恭喜！{#congratulations}

恭喜！ 您已使用&#x200B;**片段引用**&#x200B;字段更新了现有内容片段模型以引用嵌套内容片段。 您还学习了如何修改GraphQL查询以包含引用模型中的字段。
