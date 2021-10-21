---
title: iOS SwiftUI应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 提供了一个iOS应用程序，用于演示如何使用AEM的GraphQL API查询内容。 Apollo Client iOS用于生成GraphQL查询，并将数据映射到Swift对象以为应用程序提供动力。 SwiftUI用于呈现内容的简单列表和详细视图。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 3%

---


# iOS SwiftUI应用程序

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此iOS应用程序演示了如何使用AEM的GraphQL API查询内容。 Apollo Client iOS用于生成GraphQL查询，并将数据映射到Swift对象以为应用程序提供动力。 SwiftUI用于呈现内容的简单列表和详细视图。

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

* [Xcode 9.3+](https://developer.apple.com/xcode/)
* [Git](https://git-scm.com/)

## AEM要求

应用程序旨在连接到AEM **发布** 的 [WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest) 已安装。

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=zh-Hans)

我们建议 [将WKND引用站点部署到Cloud Service环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). 使用的本地设置 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 或 [AEM 6.5快速入门Jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) 也可使用。

## 使用方法

1. 克隆 `aem-guides-wknd-graphql` 存储库：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 并打开文件夹 `ios-swiftui-app`
1. 修改文件 `Config.xcconfig` 文件和更新 `AEM_HOST` 以匹配目标AEM发布环境

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. 使用Xcode构建应用程序，并将该应用程序部署到iOS模拟器
1. 应用程序上应显示来自WKND引用站点的历险列表。

## 代码

以下是用于为应用程序提供支持的重要文件和代码的简短摘要。 完整代码可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### 阿波罗·iOS

的 [阿波罗·iOS](https://www.apollographql.com/docs/ios/) 应用程序使用客户端对AEM执行GraphQL查询。 官员 [《阿波罗教程》](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) 提供了有关如何安装和使用的详细信息。

`schema.json` 是一个文件，表示安装了WKND引用站点的AEM环境中的GraphQL模式。 `schema.json` 从AEM下载并添加到项目中。 Apollo客户端会检查扩展名为的所有文件 `.graphql` 作为自定义生成阶段的一部分。 阿波罗客户随后使用 `schema.json` 文件和任何 `.graphql` 查询以自动生成文件 `API.swift`.

这为应用程序提供了一个强类型模型，用于执行查询和表示结果的模型。

![Xcode自定义内部版本阶段](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` 包含用于查询历险的查询：

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` 构造 `ApolloClient`. 的 `endpointURL` 使用的值是通过读取 `Config.xcconfig` 文件。 如果您想连接到AEM **作者** 实例和需要添加其他标头以进行身份验证，您需要修改 `ApolloClient` 这里。

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### 冒险数据

该应用程序旨在显示历险列表，然后显示每个历险的详细视图。

`AdventuresDataModel.swift` 是包含函数的类 `fetchAdventures()`. 此函数使用 `ApolloClient` 以执行查询。 成功查询后，结果数组将为类型 `AdventureListQuery.Data.AdventureList.Item`，由 `API.swift` 文件。

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

可以使用 `AdventureListQuery.Data.AdventureList.Item` 直接为应用程序供电。 但是，很可能有些数据不完整，因此某些属性可能为空。

`Adventure.swift` 是引入的自定义模型，充当由阿波罗生成的模型的包装。 `Adventure` 初始化为 `AdventureListQuery.Data.AdventureList.Item`. A `typealias` 用于缩短以使代码更易读：

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

的 `Adventure` 结构已初始化为 `AdventureData` 对象：

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

这样，我们便可以提供默认值并对不完整数据执行其他检查。 然后，我们可以使用 `Adventure` 模型可安全地为各种UI元素提供电源，并且不必不断检查空值。

在AEM中，内容块由 `_path`. 在 `Adventure.swift` 我们在 `id` 值为的属性 `_path`. 这允许 `Adventure` 实施 `Identifiable` 界面，从而更便于在数组或列表上进行迭代。

### 视图

SwiftUI用于应用程序中的各种视图。 一个适用于 [构建列表和导航](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) 可在Apple的开发人员网站上找到。 此应用程序的代码是松散派生的。

`WKNDAdventuresApp.swift` 是应用程序的条目。 包括 `AdventureListView` 和 `.onAppear` 事件用于获取冒险数据。

`AdventureListView.swift`  — 创建 `NavigationView` 和由 `AdventureRowView`. 导航到 `AdventureDetailView` 设置在此处。

`AdventureRowView`  — 连续显示冒险的主图像和冒险标题。

`AdventureDetailView`  — 显示单个冒险的完整详细信息，包括标题、描述、价格、活动类型和主图像。

阿波罗CLI运行并重新生成 `API.swift` 会导致预览停止。 要使用“自动预览”功能，您需要更新 **阿波罗CLI** 生成阶段并检查以运行脚本 **仅用于安装内部版本**.

![仅检查是否安装内部版本](assets/ios-swiftui-app/update-build-phases.png)

### 远程映像

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 和 [SDWEbImage](https://github.com/SDWebImage/SDWebImage) 用于从AEM加载远程图像，这些图像会在“行”和“详细信息”视图中填充Adventure主图像。

的 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) 是一个本机SwiftUI视图，也可供使用。 `AsyncImage` 仅支持iOS 15.0及更高版本。

## 其他资源

* [AEM Headless入门 — GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [SwiftUI列表和导航教程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Apollo iOS客户端教程](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)

