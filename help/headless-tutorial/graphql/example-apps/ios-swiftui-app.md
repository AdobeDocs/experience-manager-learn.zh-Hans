---
title: iOS应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的好方法。 此iOS应用程序演示了如何使用AEM的GraphQL API通过持久查询来查询内容。
version: Experience Manager as a Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# iOS应用程序

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的好方法。 此iOS应用程序演示了如何使用AEM的GraphQL API通过持久查询来查询内容。

使用AEM Headless的![iOS SwiftUI应用程序](./assets/ios-swiftui-app/ios-app.png)

在GitHub[&#128279;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)上查看源代码

## 先决条件 {#prerequisites}

应在本地安装以下工具：

+ [Xcode](https://developer.apple.com/xcode/)&#x200B;(需要macOS)
+ [Git](https://git-scm.com/)

## AEM要求

iOS应用程序可与以下AEM部署选项配合使用。 所有部署都需要安装[WKND站点v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)进行本地设置

iOS应用程序设计用于连接到&#x200B;__AEM Publish__&#x200B;环境，但是，如果在AEM应用程序的配置中提供身份验证，则它可以从iOS Author获取内容。

## 使用方法

1. 克隆`adobe/aem-guides-wknd-graphql`存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 打开[Xcode](https://developer.apple.com/xcode/)并打开文件夹`ios-app`
1. 修改文件`Config.xcconfig`并更新`AEM_SCHEME`和`AEM_HOST`以匹配您的目标AEM发布服务。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   如果连接到AEM Author，请将`AEM_AUTH_TYPE`和支持身份验证属性添加到`Config.xcconfig`。

   __基本身份验证__

   `AEM_USERNAME`和`AEM_PASSWORD`验证本地AEM用户是否有权访问WKND GraphQL内容。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __令牌身份验证__

   `AEM_TOKEN`是一个[访问令牌](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)，它向有权访问WKND GraphQL内容的AEM用户进行身份验证。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode构建应用程序并将应用程序部署到iOS模拟器
1. 应用程序上应显示WKND站点中的冒险列表。 选择冒险会打开冒险详细信息。 在冒险列表视图中，拉取以从AEM刷新数据。

## 代码

以下摘要介绍了iOS应用程序的构建方式、它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及这些数据的呈现方式。 可在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)上找到完整代码。

### 持久查询

遵循AEM Headless最佳实践，iOS应用程序使用AEM GraphQL持久查询来查询冒险数据。 该应用程序使用两个持久查询：

+ `wknd/adventures-all`持久查询，该查询返回AEM中的所有冒险，并包含属性删节集。 此持久查询驱动初始视图的冒险列表。

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug`持久查询，该查询返回由`slug`（唯一标识冒险的自定义属性）通过一组完整属性进行的单次冒险。 此持久查询为冒险详细信息视图提供支持。

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### 执行GraphQL持久查询

AEM的持久查询通过HTTP GET执行，因此，无法使用Apollo等使用HTTP POST的常用GraphQL库。 而是创建一个自定义类，用于执行对AEM的持久查询HTTP GET请求。

`AEM/Aem.swift`实例化用于与AEM Headless的所有交互的`Aem`类。 模式是：

1. 每个持久查询都有一个相应的公共函数(例如 `getAdventures(..)`或`getAdventureBySlug(..)`)调用iOS应用程序的视图以获取冒险数据。
1. 公共函数调用一个专用函数`makeRequest(..)`，该函数会向AEM Headless调用异步HTTP GET请求，并返回JSON数据。
1. 然后，每个公共基金都会对JSON数据进行解码，并执行任何所需的检查或转换，然后将冒险数据返回到视图。

   + AEM的GraphQL JSON数据使用`AEM/Models.swift`中定义的结构/类进行解码，这些结构/类映射到返回我的AEM Headless的JSON对象。

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }
    
    ...

    /// #makeRequest(..)
    /// Generic method for constructing and executing AEM GraphQL persisted queries
    private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
        // Encode optional parameters as required by AEM
        let persistedQueryParams = params.map { (param) -> String in
            encode(string: ";\(param.key)=\(param.value)")
        }.joined(separator: "")
        
        // Construct the AEM GraphQL persisted query URL, including optional query params
        let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

        var request = URLRequest(url: URL(string: url)!);

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### GraphQL响应数据模型

iOS更喜欢将JSON对象映射到类型化数据模型。

`src/AEM/Models.swift`定义了[可解码的](https://developer.apple.com/documentation/swift/decodable) Swift结构和类，这些结构和类映射到由AEM JSON响应返回的AEM JSON响应。

### 视图

SwiftUI用于应用程序中的各种视图。 Apple提供了[使用SwiftUI构建列表和导航的快速入门教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)。

+ `WKNDAdventuresApp.swift`

  应用程序的条目并包含`AdventureListView`，其`.onAppear`事件处理程序用于通过`aem.getAdventures()`获取所有冒险数据。 在此初始化共享`aem`对象，并作为[EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject)向其他视图公开。

+ `Views/AdventureListView.swift`

  显示冒险列表（基于来自`aem.getAdventures()`的数据），并使用`AdventureListItemView`显示每个冒险的列表项。

+ `Views/AdventureListItemView.swift`

  显示冒险列表(`Views/AdventureListView.swift`)中的每个项目。

+ `Views/AdventureDetailView.swift`

  显示冒险的详细信息，包括标题、描述、价格、活动类型和主图像。 此视图使用`aem.getAdventureBySlug(slug: slug)`查询AEM以获取完整的冒险详细信息，其中`slug`参数是基于选择列表行传入的。

### 远程图像

冒险内容片段引用的图像由AEM提供。 此iOS应用在GraphQL响应中使用路径`_dynamicUrl`字段，并在`AEM_SCHEME`和`AEM_HOST`前添加前缀以创建完全限定的URL。 如果针对AE SDK进行开发，则`_dynamicUrl`返回null，因此对于开发，回退到图像的`_path`字段。

如果连接到AEM上需要授权的受保护资源，则还必须将凭据添加到图像请求。

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI)和[SDWebImage](https://github.com/SDWebImage/SDWebImage)用于从AEM加载在`AdventureListItemView`和`AdventureDetailView`视图上填充Adventure图像的远程图像。

`aem`类（在`AEM/Aem.swift`中）通过两种方式便于使用AEM映像：

1. `aem.imageUrl(path: String)`在视图中使用以在AEM的方案前面添加并托管到图像的路径，从而创建了完全限定的URL。

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. 根据iOS应用程序配置，`Aem`中的`convenience init(..)`在图像HTTP请求中设置HTTP授权标头。

   + 如果配置了&#x200B;__基本身份验证__，则基本身份验证将附加到所有图像请求。

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + 如果配置了&#x200B;__令牌身份验证__，则令牌身份验证已附加到所有图像请求。

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + 如果未配置&#x200B;__身份验证__，则不会将身份验证附加到图像请求。

对于SwiftUI本机[AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage)，也可以使用类似的方法。 iOS 15.0+支持`AsyncImage`。

## 其他资源

+ [AEM Headless快速入门 — GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-Headless/graphql/multi-step/overview.html?lang=zh-Hans)
+ [SwiftUI列表和导航教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
