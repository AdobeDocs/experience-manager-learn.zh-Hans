---
title: 管理AEM GraphQL的AEM主機
description: 瞭解如何在AEM Headless應用程式中設定AEM主機。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
source-git-commit: ec2609ed256ebe6cdd7935f3e8d476c1ff53b500
workflow-type: tm+mt
source-wordcount: '1669'
ht-degree: 1%

---

# 管理AEM主機

部署AEM Headless應用程式時，需要注意AEM URL的建構方式，以確保使用正確的AEM主機/網域。 要注意的主要URL/請求型別為：

+ HTTP要求至 __[AEM GRAPHQL API](#aem-graphql-api-requests)__
+ __[影像URL](#aem-image-urls)__ 對內容片段中參考且由AEM提供的資產建立影像

AEM Headless應用程式通常會與單一AEM服務互動，以處理GraphQL API和影像請求。 AEM服務會根據AEM Headless應用程式部署而變更：

| AEM Headless部署型別 | AEM環境 | AEM服務 |
|-------------------------------|:---------------------:|:----------------:|
| 生产 | 生产 | 发布 |
| 製作預覽 | 生产 | 预览 |
| 开发 | 开发 | 发布 |

若要處理部署型別排列，每個應用程式部署都是使用指定要連線的AEM服務的設定所建置。 接著會使用已設定的AEM服務主機/網域來建構AEM GraphQL API URL和影像URL。 若要確定管理組建相依設定的正確方法，請參閱AEM Headless應用程式的架構(例如React、iOS、Android™等)檔案，因為方法會依架構而異。

| 使用者端型別 | [單頁應用程式(SPA)](../spa.md) | [Web元件/JS](../web-component.md) | [移动设备](../mobile.md) | [伺服器對伺服器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM主機設定 | ✔ | ✔ | ✔ | ✔ |

以下是建構URL的可能方法範例： [AEM GRAPHQL API](#aem-graphql-api-requests) 和 [影像要求](#aem-image-requests)，適用於幾個熱門的Headless架構和平台。

## AEM GraphQL API請求

從Headless應用程式到AEM GraphQL API的HTTPGET請求必須設定為與正確的AEM服務互動，如中所述 [上表](#managing-aem-hosts).

使用時 [AEM Headless SDK](../../how-to/aem-headless-sdk.md) (適用於瀏覽器式JavaScript、伺服器式JavaScript和Java™)，AEM主機可以初始化AEM Headless使用者端物件，並透過AEM Service連線。

在開發自訂AEM Headless使用者端時，請確定AEM服務的主機可以根據組建引數引數化。

### 示例

以下範例說明AEM GraphQL API要求如何讓AEM主機值可供各種Headless應用程式架構設定。

+++ React範例

此範例大致上取決於 [AEM Headless React應用程式](../../example-apps/react-app.md)，說明如何將AEM GraphQL API要求設定為根據環境變數連線至不同的AEM Services。

React應用程式應使用 [適用於JavaScript的AEM Headless使用者端](../../how-to/aem-headless-sdk.md) 與AEM GraphQL API互動。 適用於JavaScript的AEM Headless使用者端提供的AEM Headless使用者端必須透過其連線的AEM Service主機進行初始化。

#### React環境檔案

React使用 [自訂環境檔案](https://create-react-app.dev/docs/adding-custom-environment-variables/)，或 `.env` 檔案，儲存在專案的根以定義組建特定值。 例如， `.env.development` 檔案包含在開發期間使用的值，而 `.env.production` 包含用於生產組建的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 其他用途的檔案 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 透過後置修正 `.env` 和語意描述項，例如 `.env.stage` 或 `.env.production`. 不同 `.env` 檔案可用於執行或建置React應用程式，方法是設定 `REACT_APP_ENV` 執行之前 `npm` 命令。

例如，React應用程式的 `package.json` 可能包含下列內容 `scripts` 設定：

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### AEM headless使用者端

此 [適用於JavaScript的AEM Headless使用者端](../../how-to/aem-headless-sdk.md) 包含向AEM GraphQL API發出HTTP請求的AEM Headless使用者端。 AEM Headless使用者端必須使用互動的AEM主機來初始化，並使用作用中的值 `.env` 檔案。

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### React useEffect(..) 鉤點

自訂React useEffect掛接會代表React元件呈現檢視，呼叫以AEM主機初始化的AEM Headless使用者端。

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### React元件

自訂useEffect鉤點， `useAdventureByPath` 會匯入，並用來透過AEM Headless使用者端取得資料，最終將內容呈現給一般使用者。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™範例

此範例是根據 [AEM Headless iOS™應用程式範例](../../example-apps/ios-swiftui-app.md)，說明如何將AEM GraphQL API請求設定為根據以下基準連線至不同的AEM主機： [組建專用設定變數](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

iOS™應用程式需要自訂AEM Headless使用者端才能與AEM GraphQL API互動。 必須撰寫AEM Headless使用者端，才能設定AEM服務主機。

#### 建置設定

XCode組態檔案包含預設組態詳細資訊。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 初始化自訂AEM Headless使用者端

此 [AEM Headless iOS應用程式範例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) 使用自訂的AEM headless使用者端，初始化的設定值為 `AEM_SCHEME` 和 `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

自訂AEM Headless使用者端(`api/Aem.swift`)包含方法 `makeRequest(..)` 會以設定的AEM GraphQL API請求作為前置詞AEM `scheme` 和 `host`.

+ `api/Aem.swift`

```swift
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
    
    return request;
}
```

[可以建立新的組建組態檔](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 以連線至不同的AEM服務。 的組建特定值 `AEM_SCHEME` 和 `AEM_HOST` 會根據Xcode中選取的組建來使用，導致自訂AEM Headless使用者端與正確的AEM服務連線。

+++

+++ Android™範例

此範例是根據 [範例AEM Headless Android™應用程式](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，說明如何設定AEM GraphQL API請求，以根據組建特定（或風格）設定變數連線至不同的AEM Services。

Android™應用程式(以Java™撰寫時)應使用 [適用於Java™的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-java) 與AEM GraphQL API互動。 適用於Java™的AEM Headless使用者端提供的AEM Headless使用者端必須透過其連線的AEM Service主機進行初始化。

#### 建置組態檔

Android™應用程式會定義「productFlavors」，以用於建立不同用途的成品。
此範例說明如何定義兩種Android™產品風格，提供不同的AEM服務主機(`AEM_HOST`)開發值(`dev`)和生產(`prod`)使用。

在應用程式的 `build.gradle` 檔案，新 `flavorDimension` 已命名 `env` 「 」已建立。

在 `env` 維度，二 `productFlavors` 已定義： `dev` 和 `prod`. 每個 `productFlavor` 使用 `buildConfigField` ，以設定定義要連線之AEM服務的組建特定變數。

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Android™載入程式

初始化 `AEMHeadlessClient` 產生器，由AEM Headless Client for Java™提供，具有 `AEM_HOST` 值來自 `buildConfigField` 欄位。

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

為不同用途建置Android™應用程式時，請指定 `env` 和對應的AEM主機值一起使用。

+++

## AEM影像URL

從Headless應用程式到AEM的影像要求必須設定為與正確的AEM服務互動，如中所述 [表格上方](#managing-aem-hosts).

Adobe建議使用 [最佳化的影像](../../how-to/images.md) 可透過 `_dynamicUrl` AEM GraphQL API中的欄位。 此 `_dynamicUrl` 欄位會傳回無主機URL，此URL可加上用來查詢AEM GraphQL API的AEM服務主機作為前置詞。 對於 `_dynamicUrl` GraphQL回應中的欄位看起來像這樣：

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### 示例

以下範例說明影像URL如何在可針對各種Headless應用程式架構設定的AEM主機值加上前置詞。 這些範例假設使用GraphQL查詢，這些查詢使用傳回影像參考。 `_dynamicUrl` 欄位。

例如：

#### GraphQL持續查詢

此GraphQL查詢會傳回影像參考的 `_dynamicUrl`. 如所示 [GraphQL回應](#examples-react-graphql-response) 會排除主機。

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### GraphQL回應

此GraphQL回應會傳回影像參考的 `_dynamicUrl` 會排除主機。

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ React範例

此範例是根據 [範例AEM Headless React應用程式](../../example-apps/react-app.md)，說明如何將影像URL設定為根據環境變數連線至正確的AEM Services。

此範例顯示影像參考的前置詞 `_dynamicUrl` 欄位，可設定 `REACT_APP_AEM_HOST` React環境變數。

#### React環境檔案

React使用 [自訂環境檔案](https://create-react-app.dev/docs/adding-custom-environment-variables/)，或 `.env` 檔案，儲存在專案的根以定義組建特定值。 例如， `.env.development` 檔案包含在開發期間使用的值，而 `.env.production` 包含用於生產組建的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 其他用途的檔案 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 透過後置修正 `.env` 和語意描述項，例如 `.env.stage` 或 `.env.production`. 不同 `.env` 檔案可用於執行或建置React應用程式，方法是設定 `REACT_APP_ENV` 執行之前 `npm` 命令。

例如，React應用程式的 `package.json` 可能包含下列內容 `scripts` 設定：

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### React元件

React元件會匯入 `REACT_APP_AEM_HOST` 環境變數，並為影像加上前置詞 `_dynamicUrl` 值，以提供完全可解析的影像URL。

相同 `REACT_APP_AEM_HOST` 環境變數可用來初始化所使用的AEM Headless使用者端 `useAdventureByPath(..)` 用於從AEM擷取GraphQL資料的自訂useEffect鉤點。 使用相同的變數來建構GraphQL API要求作為影像URL，請確定React應用程式會在兩個使用案例中與相同的AEM服務互動。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™範例

此範例是根據 [AEM Headless iOS™應用程式範例](../../example-apps/ios-swiftui-app.md)，說明如何設定AEM影像URL以根據以下專案連線至不同的AEM主機： [組建專用設定變數](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### 建置設定

XCode組態檔案包含預設組態詳細資訊。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 影像URL產生器

在 `Aem.swift`，自訂AEM headless使用者端實作、自訂函式 `imageUrl(..)` 採用中提供的影像路徑 `_dynamicUrl` GraphQL欄位中輸入值，並在前面加上AEM主機。 每當影像呈現時，就會在iOS檢視中叫用此函式。

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### iOS檢視

iOS檢視和影像前置詞 `_dynamicUrl` 值，以提供完全可解析的影像URL。

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[可以建立新的組建組態檔](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 以連線至不同的AEM服務。 的組建特定值 `AEM_SCHEME` 和 `AEM_HOST` 會根據Xcode中選取的組建來使用，導致自訂AEM Headless使用者端與正確的AEM服務互動。

+++

+++ Android™範例

此範例是根據 [範例AEM Headless Android™應用程式](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，說明如何設定AEM影像URL，以根據組建特定（或風格）設定變數連線至不同的AEM Services。

#### 建置組態檔

Android™應用程式會定義「productFlavors」，以用於建立不同用途的成品。
此範例說明如何定義兩種Android™產品風格，提供不同的AEM服務主機(`AEM_HOST`)開發值(`dev`)和生產(`prod`)使用。

在應用程式的 `build.gradle` 檔案，新 `flavorDimension` 已命名 `env` 「 」已建立。

在 `env` 維度，二 `productFlavors` 已定義： `dev` 和 `prod`. 每個 `productFlavor` 使用 `buildConfigField` ，以設定定義要連線之AEM服務的組建特定變數。

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### 正在載入AEM影像

Android™使用 `ImageGetter` 以從AEM擷取影像資料並於本機快取。 在 `prepareDrawableFor(..)` AEM服務主機（定義於作用中建置組態）是用來為影像路徑加上前置詞，以建立AEM的可解析URL。

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Android™檢視

Android™檢視會透過 `RemoteImagesCache` 使用 `_dynamicUrl` GraphQL回應中的值。

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

為不同用途建置Android™應用程式時，請指定 `env` 和對應的AEM主機值一起使用。

+++
