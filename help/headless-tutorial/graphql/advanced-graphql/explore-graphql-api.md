---
title: 探索AEM GraphQL API - AEM无头 — GraphQL的高级概念
description: 使用GraphiQL IDE发送GraphQL查询。 了解如何使用过滤器、变量和指令进行高级查询。 查询片段和内容引用，包括多行文本字段中的引用。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---

# 浏览AEM GraphQL API

AEM中的GraphQL API允许您向下游应用程序公开内容片段数据。 在上一个 [多步GraphQL教程](../multi-step/explore-graphql-api.md)，您探索了GraphiQL集成开发环境(IDE)，在该环境中测试并优化了一些常见的GraphQL查询。 在本章中，您将使用GraphiQL IDE来浏览更高级的查询，以收集您在上一章中创建的内容片段的数据。

## 前提条件 {#prerequisites}

本文档是多部分教程的一部分。 在继续处理本章之前，请确保已完成前几章。

必须先安装GraphiQL IDE，然后才能完成本章。 请按照 [多步GraphQL教程](../multi-step/explore-graphql-api.md) 以了解更多信息。

## 目标 {#objectives}

在本章中，您将学习如何：

* 使用查询变量筛选包含引用的内容片段列表
* 片段引用中的内容过滤器
* 从多行文本字段查询内联内容和片段引用
* 使用指令进行查询
* 查询JSON对象内容类型

## 使用查询变量过滤内容片段列表

在上一个 [多步GraphQL教程](../multi-step/explore-graphql-api.md)，您学习了如何过滤内容片段列表。 在此，您将展开此知识并使用变量进行筛选。

在开发客户端应用程序时，在大多数情况下，您需要根据动态参数筛选内容片段。 AEM GraphQL API允许您将这些参数作为变量在查询中传递，以避免运行时客户端上的字符串构建。 有关GraphQL变量的更多信息，请参阅 [GraphQL文档](https://graphql.org/learn/queries/#variables).

在本例中，查询所有具有特定技能的讲师。

1. 在GraphiQL IDE中，将以下查询粘贴到左侧面板中：

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
         fullName
         contactInfo {
           phone
           email
         }
         profilePicture {
           ... on ImageRef {
             _path
           }
         }
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   的 `listPersonBySkill` 上述查询接受一个变量(`skillFilter`) `String`. 此查询会针对所有人员内容片段执行搜索，并根据 `skills` 字段和传入的字符串 `skillFilter`.

   请注意 `listPersonBySkill` 包括 `contactInfo` 属性，这是对在前几章中定义的联系信息模型的片段引用。 “联系信息”模型包含 `phone` 和 `email` 字段。 要使查询能够正确执行，必须至少在查询中包含其中一个字段。

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. 接下来，让我们定义 `skillFilter` 让所有精通滑雪的教官。 将以下JSON字符串粘贴到GraphiQL IDE的“查询变量”面板中：

   ```json
   {
   	    "skillFilter": "Skiing"
   }
   ```

1. 执行查询。 结果应类似于以下内容：

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer.\nBorn in Baltimore, Maryland, Stacey is the youngest of six children. Her father was a lieutenant colonel in the US Navy and her mother was a modern dance instructor. Her family moved frequently with her father’s duty assignments, and she took her first pictures when he was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

## 片段引用中的内容过滤器

AEM GraphQL API允许您查询嵌套内容片段。 在上一章中，您为冒险内容片段添加了三个新片段引用： `location`, `instructorTeam`和 `administrator`. 现在，让我们过滤所有具有特定名称的管理员的历险。

>[!CAUTION]
>
>只有一个模型才能作为此查询的引用才能正确执行。

1. 在GraphiQL IDE中，将以下查询粘贴到左侧面板中：

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         adventureTitle
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. 接下来，将以下JSON字符串粘贴到“查询变量”面板中：

   ```json
   {
   	    "name": "Jacob Wester"
   }
   ```

   的 `getAdventureAdministratorDetailsByAdministratorName` 查询过滤任何 `administrator` of `fullName` “Jacob Wester”，从两个嵌套内容片段中返回信息：冒险和教师。

1. 执行查询。 结果应类似于以下内容：

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "adventureTitle": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for 3 years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## 从多行文本字段查询内联引用 {#query-rte-reference}

AEM GraphQL API允许您查询多行文本字段中的内容和片段引用。 在上一章中，您向 **描述** “约塞米蒂团队内容片段”的字段。 现在，让我们检索这些引用。

1. 在GraphiQL IDE中，将以下查询粘贴到左侧面板中：

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   的 `getTeamByAdventurePath` 查询按路径过滤所有历险项，并返回 `instructorTeam` 特定冒险的片段引用。

   `_references` 是系统生成的字段，用于显示引用，包括插入到多行文本字段中的引用。

   的 `getTeamByAdventurePath` 查询可检索多个引用。 首先，它使用内置 `ImageRef` 要检索的对象 `_path` 和 `__typename` 作为内容引用插入到多行文本字段中的图像。 接下来，它使用 `LocationModel` 用于检索插入到同一字段中的位置内容片段的数据。

   请注意，该查询还包含 `_metadata` 字段。 这允许您检索团队内容片段的名称，并稍后在WKND应用程序中显示该名称。

1. 接下来，将以下JSON字符串粘贴到“查询变量”面板中，以获取Yosemite Backpacking Adventure:

   ```json
   {
   	    "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. 执行查询。 结果应类似于以下内容：

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   请注意， `_references` 字段会显示徽标图像和插入到中的Yosemite Valley Lodge内容片段 **描述** 字段。


## 使用指令进行查询

有时，在开发客户端应用程序时，您需要有条件地更改查询的结构。 在这种情况下，AEM GraphQL API允许您使用GraphQL指令，以便根据提供的条件更改查询的行为。 有关GraphQL指令的更多信息，请参阅 [GraphQL文档](https://graphql.org/learn/queries/#directives).

在 [上一部分](#query-rte-reference)，您学习了如何在多行文本字段中查询内联引用。 请注意，内容是从 `description` 在 `plaintext` 格式。 接下来，让我们展开该查询并使用指令有条件地检索 `description` 在 `json` 格式。

1. 在GraphiQL IDE中，将以下查询粘贴到左侧面板中：

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   上述查询接受一个以上变量(`includeJson`) `Boolean`，也称为查询的指令。 指令可用于有条件地包括来自 `description` 字段 `json` 格式，基于传入的布尔值 `includeJson`.

1. 接下来，将以下JSON字符串粘贴到“查询变量”面板中：

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. 执行查询。 您应获得与上一节中相同的结果 [如何在多行文本字段中查询内联引用](#query-rte-reference).

1. 更新 `includeJson` 指令 `true` 并再次执行查询。 结果应类似于以下内容：

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## 查询JSON对象内容类型

请记住，在上一章中有关创作内容片段的内容，您向 **各季天气** 字段。 现在，让我们在位置内容片段中检索该数据。

1. 在GraphiQL IDE中，将以下查询粘贴到左侧面板中：

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
           json
         }
         contactInfo {
           phone
           email
         }
         locationImage {
           ... on ImageRef {
             _path
           }
         }
         weatherBySeason
         address {
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   ```

1. 接下来，将以下JSON字符串粘贴到“查询变量”面板中：

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. 执行查询。 结果应类似于以下内容：

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California’s Sierra Nevada mountains. It’s famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   请注意， `weatherBySeason` 字段包含在上一章中添加的JSON对象。

## 同时查询所有内容

迄今为止，已执行多个查询来说明AEM GraphQL API的功能。 只能通过一个查询检索相同的数据：

```graphql
query getAllAdventureDetails($fragmentPath: String!) {
  adventureByPath(_path: $fragmentPath){
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
      adventurePrimaryImage{
        ...on ImageRef{
          _path
          mimeType
          width
          height
        }
      }
      adventureDescription {
        html
        json
      }
      adventureItinerary {
        html
        json
      }
      location {
        _path
        name
        description {
          html
          json
        }
        contactInfo{
          phone
          email
        }
        locationImage{
          ...on ImageRef{
            _path
          }
        }
        weatherBySeason
        address{
            streetAddress
            city
            state
            zipCode
            country
        }
      }
      instructorTeam {
        _metadata{
            stringMetadata{
                name
                value
            }
        }        
        teamFoundingDate
        description {
            json
        }
        teamMembers {
            fullName
            contactInfo {
                phone
                email
            }
            profilePicture{
                ...on ImageRef {
                    _path
                }
            }
            instructorExperienceLevel
            skills
            biography {
                html
            }
        }       
     }
      administrator {
            fullName
            contactInfo {
                phone
                email
            }
            biography {
                html
            }
        }
    }
    _references {
        ...on ImageRef {
            _path
        mimeType
        }
        ...on LocationModel {
            _path
                __typename
        }
    }
  }
}

# in Query Variables
{
  "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
}
```

## 有关WKND应用程序的其他查询

下面列出了用于检索WKND应用程序中所需的所有数据的查询。 这些查询不演示任何新概念，仅作为引用提供，以帮助您构建实施。

1. **为团队成员安排特定冒险活动**:

   ```graphql
   query getTeamMembersByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath ) {
       item {
         instructorTeam {
           teamMembers{
             fullName
             contactInfo{
               phone
               email
             }
           profilePicture {
               ... on ImageRef {
                 _path
               }
           }
             instructorExperienceLevel
             skills
             biography{
               plaintext
             }
           }
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **获取特定冒险的位置路径**

   ```graphql
   query getLocationPathByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath){
       item {
         location{
           _path  
         } 
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **按路径获取团队位置**

   ```graphql
   query getTeamLocationByLocationPath ($fragmentPath: String!){
     locationByPath (_path: $fragmentPath) {
       item {
         name
         description{
           json
         }
         contactInfo{
           phone
           email
         }
           address{
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge"
   }
   ```

## 恭喜！

恭喜！ 您现在已测试高级查询，以收集您在上一章中创建的内容片段的数据。

## 下面的步骤

在 [下一章](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md)，您将了解如何保留GraphQL查询，以及在应用程序中使用持久查询的最佳实践原因。