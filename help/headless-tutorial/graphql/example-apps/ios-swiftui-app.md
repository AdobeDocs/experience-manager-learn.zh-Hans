---
title: iOS应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此iOS应用程序演示了如何使用持久化查询来使用AEM GraphQL API查询内容。
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 4%

---

# iOS应用程序

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此iOS应用程序演示了如何使用持久化查询来使用AEM GraphQL API查询内容。

![iOS带AEM Headless的SwiftUI应用程序](./assets/ios-swiftui-app/ios-app.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [Xcode](https://developer.apple.com/xcode/) (需要macOS)
+ [Git](https://git-scm.com/)

## AEM要求

iOS应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安装。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用进行本地设置 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)
+ [AEM 6.5 SP13+快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans?lang=en#install-local-aem-instances)

iOS应用程序旨在连接到 __AEM发布__ 环境中，但是，如果在iOS应用程序配置中提供了身份验证，则可以从AEM Author中源内容。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 并打开文件夹 `ios-app`
1. 修改文件 `Config.xcconfig` 文件和更新 `AEM_SCHEME` 和 `AEM_HOST` 以匹配目标AEM发布服务。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   如果连接到AEM作者，请添加 `AEM_AUTH_TYPE` 和支持 `Config.xcconfig`.

   __基本身份验证__

   的 `AEM_USERNAME` 和 `AEM_PASSWORD` 验证本地AEM用户对WKND GraphQL内容的访问权限。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __令牌身份验证__

   的 `AEM_TOKEN` 是 [访问令牌](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) 向具有WKND GraphQL内容访问权限的AEM用户进行身份验证。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode构建应用程序，并将该应用程序部署到iOS模拟器
1. 应用程序上应显示WKND网站的历险列表。 选择冒险可打开探险详情。 在冒险列表视图中，提取以从AEM刷新数据。

## 代码

以下是如何构建iOS应用程序、如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及如何显示该数据的摘要。 完整代码可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### 持久化查询

遵循AEM Headless最佳实践，iOS应用程序使用AEM GraphQL持久查询来查询冒险数据。 应用程序使用两个持久查询：

+ `wknd/adventures-all` 持久查询，该查询会返回具有一组简略属性的AEM中的所有冒险。 此持久查询驱动初始视图的探险列表。

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 持久查询，返回单个冒险项 `slug` （一个自定义属性，用于唯一标识冒险），具有一组完整的属性。 此持久查询支持探险详细信息视图。

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

AEM持久化查询是通过HTTPGET执行的，因此，使用HTTPPOST（如Apollo）的常用GraphQL库将无法使用。 而是创建一个自定义类，以执行对AEM的持久查询HTTPGET请求。

`AEM/Aem.swift` 实例化 `Aem` 类，用于与AEM Headless的所有交互。 模式是：

1. 每个持久化查询都具有相应的公共函数(例如， `getAdventures(..)` 或 `getAdventureBySlug(..)`)调用iOS应用程序的视图以获取冒险数据。
1. 公共函数调用一个专用函数 `makeRequest(..)` 会调用AEM Headless的异步HTTPGET请求，并返回JSON数据。
1. 然后，每个公共函数都会解码JSON数据，并执行任何必需的检查或转换，然后再将Adventure数据返回到视图。

   + AEM GraphQL JSON数据使用 `AEM/Models.swift`，映射到JSON对象时返回了我的AEM Headless。

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
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

iOS首选将JSON对象映射到键入的数据模型。

的 `src/AEM/Models.swift` 定义 [可解析](https://developer.apple.com/documentation/swift/decodable) 映射到由AEM JSON响应返回的AEM JSON响应的Swift结构和类。

### 视图

SwiftUI用于应用程序中的各种视图。 Apple提供了 [使用SwiftUI构建列表和导航](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   应用程序的条目，包括 `AdventureListView` 谁 `.onAppear` 事件处理程序用于通过获取所有历险数据 `aem.getAdventures()`. 共享 `aem` 在此处初始化对象，并将其作为 [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   显示历险列表(基于 `aem.getAdventures()`)，并使用 `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   显示历险列表(`Views/AdventureListView.swift`)。

+ `Views/AdventureDetailView.swift`

   显示冒险的详细信息，包括标题、描述、价格、活动类型和主图像。 此视图会使用查询AEM以了解有关冒险的完整详细信息 `aem.getAdventureBySlug(slug: slug)`，其中 `slug` 参数将根据选择列表行传递。

### 远程映像

冒险内容片段引用的图像由AEM提供。 此iOS应用程序使用路径 `_path` 字段，并为 `AEM_SCHEME` 和 `AEM_HOST` 创建完全限定的URL。

如果连接到AEM上需要授权的受保护资源，则还必须将凭据添加到图像请求中。

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 和 [SDWebImage](https://github.com/SDWebImage/SDWebImage) 用于从AEM加载远程图像，该图像会填充 `AdventureListItemView` 和 `AdventureDetailView` 视图。

的 `aem` 类(在 `AEM/Aem.swift`)可通过以下两种方式方便使用AEM图像：

1. `aem.imageUrl(path: String)` 用于在视图中将AEM方案添加到前面，并将主机托管到图像的路径，从而创建完全限定的URL。

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. 的 `convenience init(..)` in `Aem` 根据iOS应用程序配置，在图像HTTP请求中设置HTTP授权标头。

   + 如果 __基本身份验证__ ，则会将基本身份验证附加到所有图像请求。

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

   + 如果 __令牌身份验证__ ，则令牌身份验证会附加到所有图像请求。

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

   + 如果 __无验证__ 配置，则不会将身份验证附加到图像请求。



在SwiftUI本机中使用类似方法 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` 在iOS 15.0及更高版本上受支持。

## 其他资源

+ [AEM Headless快速入门 — GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans)
+ [SwiftUI列表和导航教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
