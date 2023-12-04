---
title: 管理AEM GraphQL的AEM主机
description: 了解如何在AEM Headless应用程序中配置AEM主机。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 714
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# 管理AEM主机

部署AEM Headless应用程序时，需要注意AEM URL的构建方式，以确保使用正确的AEM主机/域。 要了解的主要URL/请求类型为：

+ HTTP请求到 __[AEM GRAPHQL API](#aem-graphql-api-requests)__
+ __[图像URL](#aem-image-urls)__ 用于生成内容片段中引用、由AEM交付的资产的图像

通常，AEM Headless应用程序会与单个AEM服务交互，以用于GraphQL API和图像请求。 AEM服务根据AEM Headless应用程序部署发生更改：

| AEM Headless部署类型 | AEM环境 | AEM服务 |
|-------------------------------|:---------------------:|:----------------:|
| 生产 | 生产 | 发布 |
| 创作预览 | 生产 | 预览 |
| 开发 | 开发 | 发布 |

为了处理部署类型排列，每个应用程序部署都是使用指定要连接的AEM服务的配置构建的。 随后使用配置的AEM服务的主机/域来构建AEM GraphQL API URL和图像URL。 要确定管理生成相关配置的正确方法，请参阅AEM Headless应用程序的框架(例如，React、iOS、Android™等)文档，因为该方法因框架而异。

| 客户端类型 | [单页应用程序(SPA)](../spa.md) | [Web组件/JS](../web-component.md) | [移动设备](../mobile.md) | [服务器到服务器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM主机配置 | ✔ | ✔ | ✔ | ✔ |

以下是构建URL的可能方法示例 [AEM GRAPHQL API](#aem-graphql-api-requests) 和 [图像请求](#aem-image-requests)，适用于多种流行的Headless框架和平台。

## AEM GraphQL API请求

必须配置从Headless应用程序到AEM GraphQL API的HTTPGET请求，以便与正确的AEM服务交互，如中所述 [上表](#managing-aem-hosts).

使用时 [AEM Headless SDK](../../how-to/aem-headless-sdk.md) (可用于基于浏览器的JavaScript、基于服务器的JavaScript和Java™)，AEM主机可以使用AEM服务初始化AEM Headless客户端对象以与之连接。

在开发自定义AEM Headless客户端时，请确保AEM服务的主机可以根据构建参数进行参数化。

### 示例

以下示例用于说明AEM GraphQL API请求如何能够让针对各种Headless应用程序框架配置AEM主机值。

+++ React示例

此示例大致基于 [AEM Headless React应用程序](../../example-apps/react-app.md)，说明了如何将AEM GraphQL API请求配置为根据环境变量连接到不同的AEM Services。

React应用程序应使用 [适用于JavaScript的AEM Headless客户端](../../how-to/aem-headless-sdk.md) 以与AEM GraphQL API交互。 由AEM Headless Client for JavaScript提供的AEM Headless客户端必须使用它连接到的AEM Service主机进行初始化。

#### React环境文件

React使用 [自定义环境文件](https://create-react-app.dev/docs/adding-custom-environment-variables/)，或 `.env` 文件，存储在项目的根中以定义特定于内部版本的值。 例如， `.env.development` 文件包含在开发期间使用的值，而 `.env.production` 包含用于生产内部版本的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 其他用途的文件 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 通过后缀 `.env` 和语义描述符，例如 `.env.stage` 或 `.env.production`. 不同 `.env` 通过设置，可在运行或构建React应用程序时使用文件 `REACT_APP_ENV` 执行 `npm` 命令。

例如，React应用程序的 `package.json` 可能包含以下内容 `scripts` 配置：

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

#### AEM headless客户端

此 [适用于JavaScript的AEM Headless客户端](../../how-to/aem-headless-sdk.md) 包含一个AEM Headless客户端，该客户端向AEM GraphQL API发出HTTP请求。 必须使用来自活动的值通过与其交互的AEM主机初始化AEM Headless客户端 `.env` 文件。

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

#### React useEffect(..) 挂钩

自定义React useEffect挂接调用AEM Headless客户端，通过AEM主机初始化，以代表呈现视图的React组件。

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

#### React组件

自定义useEffect挂钩， `useAdventureByPath` 导入并用于通过AEM Headless客户端获取数据，最终向最终用户呈现内容。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™示例

此示例基于 [AEM Headless iOS™应用程序示例](../../example-apps/ios-swiftui-app.md)，说明了如何将AEM GraphQL API请求配置为根据以下主机连接到其他AEM主机： [特定于内部版本的配置变量](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

iOS™应用程序需要自定义AEM Headless客户端才能与AEM GraphQL API交互。 必须编写AEM Headless客户端，以便配置AEM服务主机。

#### 生成配置

XCode配置文件包含默认配置详细信息。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 初始化自定义AEM headless客户端

此 [AEM Headless iOS应用程序示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) 使用通过配置值初始化的自定义AEM headless客户端 `AEM_SCHEME` 和 `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

自定义AEM headless客户端(`api/Aem.swift`)包含方法 `makeRequest(..)` 它会使用配置的AEM来为AEM GraphQL API请求添加前缀 `scheme` 和 `host`.

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

[可以创建新的生成配置文件](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 以连接到其他AEM服务。 特定于内部版本的值 `AEM_SCHEME` 和 `AEM_HOST` 基于XCode中的所选内部版本使用，从而生成自定义AEM Headless客户端以与正确的AEM服务连接。

+++

+++ Android™示例

此示例基于 [AEM Headless Android™应用程序示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，说明了如何将AEM GraphQL API请求配置为根据特定于内部版本（或风格）的配置变量连接到不同的AEM Services。

Android™应用程序(使用Java™编写时)应使用 [适用于Java™的AEM Headless客户端](https://github.com/adobe/aem-headless-client-java) 以与AEM GraphQL API交互。 由AEM Headless Client for Java™提供的AEM Headless客户端必须使用它连接到的AEM Service主机进行初始化。

#### 生成配置文件

Android™应用程序定义“productFlavors”，用于为不同用途生成工件。
此示例说明如何定义两种Android™产品风格，提供不同的AEM服务主机(`AEM_HOST`)值(`dev`)和生产(`prod`)使用。

在应用程序的 `build.gradle` 文件，新 `flavorDimension` 已命名 `env` 创建。

在 `env` 维度，二 `productFlavors` 定义了： `dev` 和 `prod`. 每个 `productFlavor` 用途 `buildConfigField` 设置特定于内部版本的变量，这些变量定义要连接到的AEM服务。

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

#### Android™加载程序

初始化 `AEMHeadlessClient` 生成器，由AEM Headless Client for Java™提供，带有 `AEM_HOST` 值来自 `buildConfigField` 字段。

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

为不同用途构建Android™应用程序时，请指定 `env` 风味，并使用相应的AEM主机值。

+++

## AEM图像URL

必须将从Headless应用程序向AEM发出的图像请求配置为与正确的AEM服务交互，如中所述 [上表](#managing-aem-hosts).

Adobe建议使用 [优化的图像](../../how-to/images.md) 通过以下网站提供： `_dynamicUrl` AEM GraphQL API中的字段。 此 `_dynamicUrl` 字段返回一个无主机URL，该URL可以带有用于查询AEM GraphQL API的AEM服务主机的前缀。 对于 `_dynamicUrl` GraphQL响应中的字段如下所示：

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### 示例

以下示例介绍了图像URL如何为AEM主机值添加前缀，该主机值可针对各种Headless应用程序框架进行配置。 这些示例假定使用GraphQL查询，这些查询使用 `_dynamicUrl` 字段。

例如：

#### GraphQL持久查询

此GraphQL查询返回图像引用的 `_dynamicUrl`. 如所示 [GraphQL响应](#examples-react-graphql-response) 不包括主机。

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

#### GraphQL响应

此GraphQL响应返回图像引用的 `_dynamicUrl` 不包括主机。

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

+++ React示例

此示例基于 [示例AEM Headless React应用程序](../../example-apps/react-app.md)，说明了如何根据环境变量将图像URL配置为连接到正确的AEM Services。

此示例说明如何为图像引用添加前缀 `_dynamicUrl` 字段，带可配置 `REACT_APP_AEM_HOST` React环境变量。

#### React环境文件

React使用 [自定义环境文件](https://create-react-app.dev/docs/adding-custom-environment-variables/)，或 `.env` 文件，存储在项目的根中以定义特定于内部版本的值。 例如， `.env.development` 文件包含在开发期间使用的值，而 `.env.production` 包含用于生产内部版本的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 其他用途的文件 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 通过后缀 `.env` 和语义描述符，例如 `.env.stage` 或 `.env.production`. 不同 `.env` 通过设置，可在运行或构建React应用程序时使用文件 `REACT_APP_ENV` 执行 `npm` 命令。

例如，React应用程序的 `package.json` 可能包含以下内容 `scripts` 配置：

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

#### React组件

React组件导入 `REACT_APP_AEM_HOST` 环境变量，并为图像添加前缀 `_dynamicUrl` 值，以提供完全可解析的图像URL。

相同 `REACT_APP_AEM_HOST` 环境变量用于初始化使用的AEM Headless客户端。 `useAdventureByPath(..)` 用于从AEM获取GraphQL数据的自定义useEffect挂接。 使用相同的变量将GraphQL API请求构造为图像URL，请确保React应用程序在两个用例中与相同的AEM服务交互。

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

+++ iOS™示例

此示例基于 [AEM Headless iOS™应用程序示例](../../example-apps/ios-swiftui-app.md)，说明了如何将AEM图像URL配置为根据以下主机连接到不同的AEM主机： [特定于内部版本的配置变量](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### 生成配置

XCode配置文件包含默认配置详细信息。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 图像URL生成器

在 `Aem.swift`，自定义AEM headless客户端实施，自定义函数 `imageUrl(..)` 采用提供的图像路径 `_dynamicUrl` GraphQL字段，并在其前面加上AEM主机。 每当渲染图像时，都会在iOS视图中调用此函数。

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

#### iOS视图

iOS视图和图像前缀 `_dynamicUrl` 值，以提供完全可解析的图像URL。

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

[可以创建新的生成配置文件](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 以连接到其他AEM服务。 特定于内部版本的值 `AEM_SCHEME` 和 `AEM_HOST` 基于Xcode中的所选内部版本使用，从而生成自定义AEM Headless客户端以与正确的AEM服务交互。

+++

+++ Android™示例

此示例基于 [AEM Headless Android™应用程序示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，说明了如何将AEM图像URL配置为根据特定于内部版本（或风格）的配置变量连接到不同的AEM Services。

#### 生成配置文件

Android™应用程序定义“productFlavors”，用于为不同用途生成工件。
此示例说明如何定义两种Android™产品风格，提供不同的AEM服务主机(`AEM_HOST`)值(`dev`)和生产(`prod`)使用。

在应用程序的 `build.gradle` 文件，新 `flavorDimension` 已命名 `env` 创建。

在 `env` 维度，二 `productFlavors` 定义了： `dev` 和 `prod`. 每个 `productFlavor` 用途 `buildConfigField` 设置特定于内部版本的变量，这些变量定义要连接到的AEM服务。

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

#### 加载AEM图像

Android™使用 `ImageGetter` 从AEM获取并本地缓存图像数据。 在 `prepareDrawableFor(..)` AEM服务主机（在活动的生成配置中定义）用于为创建指向AEM的可解析URL的图像路径添加前缀。

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

#### Android™视图

Android™视图通过 `RemoteImagesCache` 使用 `_dynamicUrl` GraphQL响应中的值。

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

为不同用途构建Android™应用程序时，请指定 `env` 风味，并使用相应的AEM主机值。

+++
