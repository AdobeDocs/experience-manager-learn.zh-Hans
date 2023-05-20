---
title: iOS應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此iOS應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容。
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

# iOS應用程式

範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此iOS應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容。

![iOS SwiftUI應用程式搭配AEM Headless](./assets/ios-swiftui-app/ios-app.png)

檢視 [GitHub上的原始程式碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## 前提条件 {#prerequisites}

下列工具應安裝在本機：

+ [Xcode](https://developer.apple.com/xcode/) (需要macOS)
+ [Git](https://git-scm.com/)

## AEM需求

iOS應用程式可與下列AEM部署選項搭配使用。 所有部署都需要 [WKND網站v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即將安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本機設定，使用 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)
+ [AEM 6.5 SP13+快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans？lang=en#install-local-aem-instances)

iOS應用程式是專為連線至 __AEM發佈__ 不過，如果在iOS應用程式的設定中提供驗證，則它可以從AEM Author取得內容。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) 並開啟資料夾 `ios-app`
1. 修改檔案 `Config.xcconfig` 檔案和更新 `AEM_SCHEME` 和 `AEM_HOST` 以符合您的目標AEM Publish服務。

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   如果連線到AEM Author，請新增 `AEM_AUTH_TYPE` 和支援的驗證屬性 `Config.xcconfig`.

   __基本驗證__

   此 `AEM_USERNAME` 和 `AEM_PASSWORD` 驗證可存取WKND GraphQL內容的本機AEM使用者。

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __權杖驗證__

   此 `AEM_TOKEN` 是 [存取權杖](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) 會向可存取WKND GraphQL內容的AEM使用者進行驗證。

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. 使用Xcode建置應用程式，並將應用程式部署到iOS模擬器
1. WKND網站中的冒險清單應顯示在應用程式上。 選取冒險會開啟冒險細節。 在冒險清單檢視上，提取以從AEM重新整理資料。

## 程式碼

以下是如何建立iOS應用程式、如何連線至AEM Headless以使用GraphQL持續查詢擷取內容，以及資料如何呈現的摘要。 完整程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### 持久查詢

依照AEM Headless最佳實務，iOS應用程式會使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all` 持久查詢，這會傳回AEM中所有冒險和屬性刪節集。 此持續查詢會驅動初始檢視的冒險清單。

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

+ `wknd/adventure-by-slug` 持久查詢，會依據以下條件傳回單一冒險 `slug` （可唯一識別冒險的自訂屬性）和完整的屬性集。 此持續查詢可支援冒險詳細資料檢視。

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

### 執行GraphQL持續查詢

AEM持續性查詢會透過HTTPGET執行，因此，使用HTTPPOST（例如Apollo）的常見GraphQL程式庫無法使用。 請改為建立自訂類別，執行對AEM的持續查詢HTTPGET請求。

`AEM/Aem.swift` 例項化 `Aem` 用於與AEM Headless的所有互動的類別。 模式為：

1. 每個持續查詢都有對應的公用函式(例如： `getAdventures(..)` 或 `getAdventureBySlug(..)`)叫用iOS應用程式的檢視來取得冒險資料。
1. 公用函式呼叫私人函式 `makeRequest(..)` 會叫用對AEM Headless的非同步HTTPGET請求，並傳回JSON資料。
1. 接著，每個公用函式都會解碼JSON資料，並執行任何必要的檢查或轉換，然後才會將Adventure資料傳回檢視。

   + AEM GraphQL JSON資料會使用中定義的結構/類別解碼 `AEM/Models.swift`，對應到傳回我的AEM Headless的JSON物件。

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

### GraphQL回應資料模型

iOS偏好將JSON物件對應至型別的資料模型。

此 `src/AEM/Models.swift` 定義 [可解碼](https://developer.apple.com/documentation/swift/decodable) Swift結構和類別會對應至AEM JSON回應所傳回的AEM JSON回應。

### 视图

SwiftUI用於應用程式中的各種檢視。 Apple提供快速入門教學課程，適用於 [使用SwiftUI建立清單和導覽](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   應用程式的專案，包括 `AdventureListView` 其 `.onAppear` 事件處理常式是用來透過擷取所有冒險資料 `aem.getAdventures()`. 共用的 `aem` 在此初始化物件，並以 [環境物件](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   顯示冒險清單(根據以下來源的資料： `aem.getAdventures()`)和顯示每個冒險的清單專案，使用 `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   顯示冒險清單中的每個專案(`Views/AdventureListView.swift`)。

+ `Views/AdventureDetailView.swift`

   顯示冒險的詳細資訊，包括標題、說明、價格、活動型別和主要影像。 此檢視會使用以下專案查詢AEM的完整冒險詳細資訊 `aem.getAdventureBySlug(slug: slug)`，其中 `slug` 會根據選取清單列傳入引數。

### 遠端影像

冒險內容片段參考的影像由AEM提供。 此iOS應用程式使用路徑 `_path` GraphQL欄位中輸入的「 」字元，並在字首 `AEM_SCHEME` 和 `AEM_HOST` 以建立完整限定的URL。

如果連線到AEM上需要授權的受保護資源，則還必須將憑證新增到影像請求。

[SdwebimageswiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) 和 [Sdwebimage](https://github.com/SDWebImage/SDWebImage) 用於從AEM載入遠端影像，這些影像會填入 `AdventureListItemView` 和 `AdventureDetailView` 檢視。

此 `aem` 類別(在 `AEM/Aem.swift`)可透過兩種方式促進使用AEM影像：

1. `aem.imageUrl(path: String)` 在檢視中使用，在AEM配置的前面加上，並託管到影像的路徑，以建立完全限定的URL。

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. 此 `convenience init(..)` 在 `Aem` 根據iOS應用程式設定，在影像HTTP請求上設定HTTP授權標頭。

   + 若 __基本驗證__ 設定，然後將基本驗證附加至所有影像要求。

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

   + 若 __權杖驗證__ 完成設定後，權杖驗證就會附加至所有影像要求。

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

   + 若 __無驗證__ 設定，則影像要求不會附加任何驗證。



類似的做法可用於SwiftUI原生 [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` 在iOS 15.0+上支援。

## 其他資源

+ [AEM Headless快速入門 — GraphQL教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans)
+ [SwiftUI清單和導覽教學課程](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
