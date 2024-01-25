---
title: iOS应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此iOS应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。
version: Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headlessas a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 356
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# iOS应用程序

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此iOS应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。

![带有AEM Headless的iOS SwiftUI应用程序](./assets/ios-swiftui-app/ios-app.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [Xcode](https://developer.apple.com/xcode/) (需要macOS)
+ [Git](https://git-scm.com/)

## AEM要求

iOS应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND站点v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即将安装。

+ [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用进行本地设置 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)

iOS应用程序旨在连接到 __AEM发布__ 但是，如果在AEM应用程序的配置中提供身份验证，则它可以从iOS Author源内容。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 并打开文件夹 `ios-app`
1. 修改文件 `Config.xcconfig` 文件和更新 `AEM_SCHEME` 和 `AEM_HOST` 以匹配您的Target AEM发布服务。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   如果连接到AEM Author，请添加 `AEM_AUTH_TYPE` 并支持身份验证属性到 `Config.xcconfig`.

   __基本身份验证__

   此 `AEM_USERNAME` 和 `AEM_PASSWORD` 验证有权访问WKND GraphQL内容的本地AEM用户。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __令牌身份验证__

   此 `AEM_TOKEN` 是 [访问令牌](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) 该证书向有权访问WKND GraphQL内容的AEM用户进行身份验证。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode构建应用程序并将应用程序部署到iOS模拟器
1. 应用程序上应显示WKND站点中的冒险列表。 选择冒险会打开冒险详细信息。 在冒险列表视图中，拉取以从AEM刷新数据。

## 代码

以下摘要介绍了iOS应用程序的构建方式、它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及这些数据的呈现方式。 完整代码可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### 持久查询

遵循AEM Headless最佳实践，iOS应用程序使用AEM GraphQL持久查询来查询冒险数据。 该应用程序使用两个持久查询：

+ `wknd/adventures-all` 持久查询，该查询返回在AEM中使用一组删节的属性进行的所有冒险。 此持久查询驱动初始视图的冒险列表。

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

+ `wknd/adventure-by-slug` 持久查询，返回一次冒险 `slug` （唯一标识冒险的自定义属性）和一组完整的属性。 此持久查询为冒险详细信息视图提供支持。

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

AEM持久查询通过HTTPGET执行，因此，无法使用使用HTTPPOST（例如Apollo）的常用GraphQL库。 而是创建一个自定义类，用于执行对AEM的持久查询HTTPGET请求。

`AEM/Aem.swift` 实例化 `Aem` 用于与AEM Headless的所有交互的类。 模式是：

1. 每个持久查询都有一个相应的公共函数(例如 `getAdventures(..)` 或 `getAdventureBySlug(..)`)调用iOS应用程序的视图以获取冒险数据。
1. 公共基金称为私人基金 `makeRequest(..)` 它会调用对AEM Headless的异步HTTPGET请求，并返回JSON数据。
1. 然后，每个公共基金都会对JSON数据进行解码，并执行任何所需的检查或转换，然后将冒险数据返回到视图。

   + AEM GraphQL JSON数据使用中定义的结构/类进行解码 `AEM/Models.swift`，该ID映射到返回我的AEM Headless的JSON对象。

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

此 `src/AEM/Models.swift` 定义 [可解码](https://developer.apple.com/documentation/swift/decodable) Swift结构和类映射到由AEM JSON响应返回的AEM JSON响应。

### 视图

SwiftUI用于应用程序中的各种视图。 Apple提供了入门教程，适用于 [使用SwiftUI构建列表和导航](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  应用程序的条目，包括 `AdventureListView` 其 `.onAppear` 事件处理程序用于通过获取所有冒险数据 `aem.getAdventures()`. 共享 `aem` 在此初始化对象，并将对象作为对象向其他视图公开 [环境对象](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  显示冒险列表（基于来自的数据） `aem.getAdventures()`)，并使用显示每个冒险的列表项 `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  显示冒险列表中的每个项目(`Views/AdventureListView.swift`)。

+ `Views/AdventureDetailView.swift`

  显示冒险的详细信息，包括标题、描述、价格、活动类型和主图像。 此视图使用以下方式查询AEM以获取完整的冒险详细信息 `aem.getAdventureBySlug(slug: slug)`，其中 `slug` 根据选择列表行传入参数。

### 远程图像

冒险内容片段引用的图像由AEM提供。 此iOS应用程序使用路径 `_dynamicUrl` GraphQL字段，并在其前缀 `AEM_SCHEME` 和 `AEM_HOST` 以创建完全限定的URL。 如果针对AE SDK进行开发， `_dynamicUrl` 返回null，因此对于开发，回退到图像的 `_path` 字段。

如果连接到AEM上需要授权的受保护资源，则还必须将凭据添加到图像请求。

[SdwebimageswiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 和 [Sdwebimage](https://github.com/SDWebImage/SDWebImage) 用于从填充Adventure图像的AEM加载远程图像 `AdventureListItemView` 和 `AdventureDetailView` 视图。

此 `aem` 类(在 `AEM/Aem.swift`)便于以两种方式使用AEM图像：

1. `aem.imageUrl(path: String)` 在视图中使用以追加AEM方案并托管到图像的路径，从而创建完全限定的URL。

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. 此 `convenience init(..)` 在 `Aem` 根据iOS应用程序配置，在图像HTTP请求上设置HTTP授权标头。

   + 如果 __基本身份验证__ 配置，然后将基本身份验证附加到所有图像请求。

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

   + 如果 __令牌身份验证__ 配置，然后将令牌身份验证附加到所有图像请求。

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

   + 如果 __无身份验证__ 配置，则不会将身份验证附加到图像请求。

类似的方法可用于SwiftUI原生 [异步图像](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` 在iOS 15.0及更高版本上受支持。

## 其他资源

+ [AEM Headless快速入门 — GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans)
+ [SwiftUI列表和导航教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
