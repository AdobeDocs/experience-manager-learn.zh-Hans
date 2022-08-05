---
title: 为AEM GraphQL管理AEM主机
description: 了解如何在AEM Headless应用程序中配置AEM主机。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# 管理AEM主机

部署AEM Headless应用程序时，需要注意AEM URL的构建方式，以确保使用正确的AEM主机/域。 需要注意的主要URL/请求类型包括：

+ HTTP请求 __[AEM GraphQL API](#aem-graphql-api-requests)__
+ __[图像URL](#aem-image-urls)__ 到内容片段中引用并由AEM交付的图像资产

通常，AEM无头应用程序会与用于GraphQL API和图像请求的单个AEM服务交互。 AEM服务会根据AEM Headless应用程序部署进行更改：

| AEM无头部署类型 | AEM环境 | AEM服务 |
|-------------------------------|:---------------------:|:----------------:|
| 生产 | 生产 | 发布 |
| 创作预览 | 生产 | 预览 |
| 开发 | 开发 | 发布 |

要处理部署类型排列，每个应用程序部署都使用指定要连接的AEM服务的配置来构建。 然后，将使用配置的AEM服务的主机/域来构建AEM GraphQL API URL和图像URL。 要确定管理内部版本相关配置的正确方法，请参考AEM Headless应用程序的框架(例如React、iOS、Android™等)文档，因为方法因框架而异。

| 客户端类型 | [单页应用程序(SPA)](../spa.md) | [Web组件/JS](../web-component.md) | [移动设备](../mobile.md) | [服务器到服务器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM主机配置 | ✔ | ✔ | ✔ | ✔ |

以下是构建URL的可能方法示例 [AEM GraphQL API](#aem-graphql-api-requests) 和 [图像请求](#aem-image-requests)，适用于多种常用的无头框架和平台。

## AEM GraphQL API请求

必须将无头应用程序到AEM GraphQL API的HTTPGET请求配置为与正确的AEM服务进行交互，如 [上表](#managing-aem-hosts).

使用 [AEM Headless SDK](../../how-to/aem-headless-sdk.md) (适用于基于浏览器的JavaScript、基于服务器的JavaScript和Java™),AEM主机可以通过AEM服务初始化AEM Headless客户端对象以与之连接。

在开发自定义AEM Headless客户端时，请确保AEM服务的主机可基于构建参数进行参数化。

### 示例

以下是如何为各种无头应用程序框架配置AEM GraphQL API请求的AEM主机值的示例。

+++ React示例

此示例大致基于 [AEM Headless React应用程序](../../example-apps/react-app.md)，说明如何根据环境变量将AEM GraphQL API请求配置为连接到不同的AEM服务。

React应用程序应使用 [AEM Headless Client for JavaScript](../../how-to/aem-headless-sdk.md) 与AEM GraphQL API交互。 AEM Headless客户端由AEM Headless Client for JavaScript提供，必须使用它连接到的AEM Service主机进行初始化。

#### React环境文件

React用例 [自定义环境文件](https://create-react-app.dev/docs/adding-custom-environment-variables/)或 `.env` 文件，存储在项目的根中以定义特定于内部版本的值。 例如， `.env.development` 文件中包含用于开发期间的值，而 `.env.production` 包含用于生产内部版本的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 供其他用途使用的文件 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 邮戳 `.env` 和语义描述符，例如 `.env.stage` 或 `.env.production`. 不同 `.env` 运行或构建React应用程序时，可通过设置 `REACT_APP_ENV` 执行 `npm` 命令。

例如， React应用程序的 `package.json` 可能包含以下内容 `scripts` 配置：

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

#### AEM headless client

的 [AEM Headless Client for JavaScript](../../how-to/aem-headless-sdk.md) 包含一个AEM无头客户端，该客户端对AEM GraphQL API发出HTTP请求。 AEM Headless客户端必须使用与其交互的AEM主机使用活动中的值进行初始化 `.env` 文件。

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

#### React useEffect(..) 钩钩

自定义React useEffect挂接调用AEM Headless客户端，并代表呈现视图的React组件使用AEM主机进行初始化。

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

自定义useEffect挂接， `useAdventureByPath` 会导入，用于使用AEM Headless客户端获取数据，并最终向最终用户呈现内容。

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

此示例基于 [示例AEM Headless iOS™应用程序](../../example-apps/ios-swiftui-app.md)，说明如何将AEM GraphQL API请求配置为基于 [特定于生成的配置变量](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

iOS™应用程序需要自定义AEM Headless客户端才能与AEM GraphQL API进行交互。 必须编写AEM Headless客户端，以便可以配置AEM服务主机。

#### 生成配置

Xcode配置文件包含默认配置详细信息。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 初始化自定义AEM无头客户端

的 [AEM Headless iOS应用程序示例](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) 使用使用的配置值初始化的自定义AEM无头客户端 `AEM_SCHEME` 和 `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

自定义AEM无头客户端(`api/Aem.swift`)包含方法 `makeRequest(..)` 前缀为AEM GraphQL API请求的AEM `scheme` 和 `host`.

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

[可以创建新的内部版本配置文件](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 连接到不同的AEM服务。 的特定于内部版本的值 `AEM_SCHEME` 和 `AEM_HOST` 会根据XCode中选定的内部版本使用，从而导致自定义AEM Headless客户端与正确的AEM服务连接。

+++

+++ Android™示例

此示例基于 [示例AEM Headless Android™应用程序](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，说明如何根据特定于构建（或风格）的配置变量将AEM GraphQL API请求配置为连接到不同的AEM Services。

Android™应用程序(当以Java™编写时)应使用 [AEM Headless Client for Java™](https://github.com/adobe/aem-headless-client-java) 与AEM GraphQL API交互。 AEM Headless客户端(由AEM Headless Client for Java™提供)必须使用其连接到的AEM Service主机进行初始化。

#### 生成配置文件

Android™应用程序定义“productFlavors”，用于为不同用途构建工件。
此示例展示了如何定义两种Android™产品风格，从而提供不同的AEM服务主机(`AEM_HOST`)开发值(`dev`)和生产(`prod`)。

在应用程序的 `build.gradle` 文件，新 `flavorDimension` 已命名 `env` 创建时。

在 `env` 维度，二 `productFlavors` 已定义： `dev` 和 `prod`. 每个 `productFlavor` 使用 `buildConfigField` 以设置特定于生成的变量，这些变量定义要连接的AEM服务。

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

#### Android™加载器

初始化 `AEMHeadlessClient` 生成器，由AEM Headless Client for Java™随 `AEM_HOST` 值 `buildConfigField` 字段。

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

在构建适用于不同用途的Android™应用程序时，请指定 `env` 风味，并使用相应的AEM主机值。

+++

## AEM图像URL

必须将无头应用程序向AEM发送的图像请求配置为与正确的AEM服务进行交互，如 [上表](#managing-aem-hosts).

而AEM GraphQL的 `... on ImageRef` 提供字段 `_authorUrl` 和 `_publishUrl` 包含到各个AEM服务的绝对URL，则通常使用 `_path` 字段和为用于查询AEM GraphQL API的AEM服务主机添加前缀。

使用 `_path` 如果无头应用程序可以根据部署上下文连接到AEM作者或AEM发布，则此应用程序会特别有用。

如果无头应用程序仅与AEM作者或发布交互， `_authorUrl` 或 `_publishUrl` 字段可用于简化实施过程，并且可以忽略以下示例中的指导。

### 示例

以下示例说明了图像URL如何为可用于各种无头应用程序框架配置的AEM主机值添加前缀。 这些示例假定使用GraphQL查询，该查询使用 `_path` 字段。

例如：

#### GraphQL持久查询

此GraphQL查询返回图像引用的 `_path`. 如 [GraphQL响应](#examples-react-graphql-response) 不包括主机。

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

#### GraphQL响应

此GraphQL响应返回图像引用的 `_path` 不包括主机。

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

+++ React示例

此示例基于 [示例AEM Headless React应用程序](../../example-apps/react-app.md)，说明如何根据环境变量将图像URL配置为连接到正确的AEM服务。

此示例显示图像引用的前缀 `_path` 字段，可配置 `REACT_APP_AEM_HOST` React环境变量。

#### React环境文件

React用例 [自定义环境文件](https://create-react-app.dev/docs/adding-custom-environment-variables/)或 `.env` 文件，存储在项目的根中以定义特定于内部版本的值。 例如， `.env.development` 文件中包含用于开发期间的值，而 `.env.production` 包含用于生产内部版本的值。

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` 供其他用途使用的文件 [可以指定](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) 邮戳 `.env` 和语义描述符，例如 `.env.stage` 或 `.env.production`. 不同 `.env` 运行或构建React应用程序时，可通过设置 `REACT_APP_ENV` 执行 `npm` 命令。

例如， React应用程序的 `package.json` 可能包含以下内容 `scripts` 配置：

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

React组件会导入 `REACT_APP_AEM_HOST` 环境变量，并为图像添加前缀 `_path` 值，以提供完全可解析的图像URL。

相同 `REACT_APP_AEM_HOST` 环境变量用于初始化使用的AEM Headless客户端 `useAdventureByPath(..)` 自定义useEffect挂接，用于从AEM中获取GraphQL数据。 对于这两种用例，请使用相同的变量来构建与图像URL相同的GraphQL API请求，确保React应用程序与相同的AEM服务进行交互。

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._path }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™示例

此示例基于 [示例AEM Headless iOS™应用程序](../../example-apps/ios-swiftui-app.md)，说明如何根据 [特定于生成的配置变量](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### 生成配置

Xcode配置文件包含默认配置详细信息。

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### 图像URL生成器

在 `Aem.swift`，自定义AEM无头客户端实施，自定义函数 `imageUrl(..)` 采用中提供的图像路径 `_path` 字段，并为其预置AEM主机。 然后，每当渲染图像时，都会在iOS视图中调用此函数。

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
    /// Prefixes AEM image paths wit the AEM scheme/host
    func imageUrl(path: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(path)")!
    }
    ...
}
```

#### iOS视图

iOS视图并为图像添加前缀 `_path` 值，以提供完全可解析的图像URL。

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image path to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(path: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[可以创建新的内部版本配置文件](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) 连接到不同的AEM服务。 的特定于内部版本的值 `AEM_SCHEME` 和 `AEM_HOST` 基于XCode中选定的内部版本使用，从而导致自定义AEM Headless客户端与正确的AEM服务进行交互。

+++

+++ Android™示例

此示例基于 [示例AEM Headless Android™应用程序](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)，说明了如何根据特定于构建（或风格）的配置变量将AEM图像URL配置为连接到不同的AEM服务。

#### 生成配置文件

Android™应用程序定义“productFlavors”，用于为不同用途构建工件。
此示例展示了如何定义两种Android™产品风格，从而提供不同的AEM服务主机(`AEM_HOST`)开发值(`dev`)和生产(`prod`)。

在应用程序的 `build.gradle` 文件，新 `flavorDimension` 已命名 `env` 创建时。

在 `env` 维度，二 `productFlavors` 已定义： `dev` 和 `prod`. 每个 `productFlavor` 使用 `buildConfigField` 以设置特定于生成的变量，这些变量定义要连接的AEM服务。

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

Android™使用 `ImageGetter` 从AEM获取图像数据并将其本地缓存。 在 `prepareDrawableFor(..)` 在活动内部版本配置中定义的AEM服务主机，用于为创建可解析URL的图像路径添加前缀。

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
    public Drawable getDrawable(String path) {
        // Get the image data from the cache using the path as the key
        Drawable drawable = drawablesByPath.get(path);
        return drawable;
    }
}
```

#### Android™视图

Android™视图通过 `RemoteImagesCache` 使用 `_path` 值。

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImagePath()));
        ...
    }
...
}
```

在构建适用于不同用途的Android™应用程序时，请指定 `env` 风味，并使用相应的AEM主机值。

+++