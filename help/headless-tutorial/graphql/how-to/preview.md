---
title: 內容片段預覽
description: 瞭解如何使用內容片段預覽來預覽所有作者，以快速瞭解內容變更如何影響您的AEM Headless體驗。
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# 內容片段預覽

AEM Headless應用程式支援整合式撰寫預覽。 預覽體驗可將AEM作者的內容片段編輯器連結至您的自訂應用程式（可透過HTTP定址），允許應用程式中的深層連結，以產生正在預覽的內容片段。

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

若要使用內容片段預覽，必須符合幾個條件：

1. 應用程式必須部署至作者可存取的URL
1. 應用程式必須設定為連線至AEM作者服務（而非AEM Publish服務）
1. 應用程式在設計時必須搭配可使用的URL或路由 [內容片段路徑或ID](#url-expressions) 以選取要在應用程式體驗中預覽而顯示的內容片段。

## 預覽URL

預覽URL，使用 [URL運算式](#url-expressions)，是在內容片段模型的屬性上設定。

![內容片段模型預覽URL](./assets/preview/cf-model-preview-url.png)

1. 以管理員身分登入AEM Author服務
1. 導覽至 __「工具」>「一般」>「內容片段模型」__
1. 選取 __內容片段模型__ 並選取 __屬性__ 形成頂端動作列。
1. 使用以下專案輸入內容片段模型的預覽URL [URL運算式](#url-expressions)
   + 預覽URL必須指向連線到AEM Author服務的應用程式部署。

### URL運算式

每個內容片段模式都可以設定預覽URL。 您可以使用下表所列的URL運算式，針對每個內容片段引數化預覽URL。 可在單一預覽URL中使用多個URL運算式。

|  | URL運算式 | 价值 |
| --------------------------------------- | ----------------------------------- | ----------- |
| 內容片段路徑 | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| 內容片段ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| 內容片段變數 | `${contentFragment.variation}` | `main` |
| 內容片段模型路徑 | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| 內容片段模型名稱 | `${contentFragment.model.name}` | `adventure` |

預覽URL範例：

+ 上的預覽URL __冒險__ 模型可能顯示為 `https://preview.app.wknd.site/adventure${contentFragment.path}` 解析為 `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ 上的預覽URL __文章__ 模型可能顯示為 `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` 解析的 `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## 應用程式內預覽

使用已設定內容片段模式的任何內容片段都有「預覽」按鈕。 「預覽」按鈕會開啟內容片段模型的預覽URL，並將開啟的內容片段的值插入 [URL運算式](#url-expressions).

![“预览”按钮](./assets/preview/preview-button.png)

預覽應用程式中的內容片段變更時，執行硬重新整理（清除瀏覽器的本機快取）。

## React範例

讓我們來探索WKND應用程式，這是一個簡單的React應用程式，可顯示使用AEM Headless GraphQL API的AEM冒險。

範常式式碼位於 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL和路由

用於預覽內容片段的URL或路由必須使用來組合 [URL運算式](#url-expressions). 在這個啟用預覽的WKND應用程式版本中，冒險內容片段會透過 `AdventureDetail` 繫結至繞線的元件 `/adventure<CONTENT FRAGMENT PATH>`. 因此，WKND Adventure模型的預覽URL必須設定為 `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` 以解析此路由。

內容片段預覽僅適用於應用程式具有可定址路由（可填入）的情況 [URL運算式](#url-expressions) 以可預覽的方式在應用程式中呈現內容片段。

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### 顯示編寫的內容

此 `AdventureDetail` 元件只會剖析內容片段路徑，透過以下方式插入預覽URL： `${contentFragment.path}` [URL運算式](#url-expressions)，並用來收集及轉譯WKND Adventure。

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
