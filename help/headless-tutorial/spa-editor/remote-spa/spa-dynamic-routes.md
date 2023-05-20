---
title: 新增可編輯的元件至遠端SPA動態路由
description: 瞭解如何將可編輯的元件新增至遠端SPA中的動態路由。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 1%

---

# 動態路由和可編輯的元件

在本章中，我們啟用兩個動態冒險詳細資訊路徑來支援可編輯的元件； __巴厘島衝浪營__ 和 __波特蘭貝爾瓦納__.

![動態路由和可編輯的元件](./assets/spa-dynamic-routes/intro.png)

Adventure Detail SPA路徑定義為 `/adventure/:slug` 位置 `slug` 是冒險內容片段上的唯一識別碼屬性。

## 將SPA URL對應至AEM頁面

在前兩章中，我們將可編輯的元件內容從SPA首頁檢視對應至AEM中對應的遠端SPA根目錄頁面 `/content/wknd-app/us/en/`.

為SPA動態路由的可編輯元件定義對映類似，但是我們必須在路由的執行個體與AEM頁面之間進行1:1對映配置。

在本教學課程中，我們將取得WKND冒險內容片段的名稱（此為路徑的最後一個區段），並將其對應至下的簡單路徑 `/content/wknd-app/us/en/adventure`.

| 遠端SPA路由 | AEM頁面路徑 |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__貝爾瓦納 — 波特蘭__ | /content/wknd-app/us/en/home/adventure/__波特蘭貝爾瓦納__ |

因此，根據此對應，我們必須在以下位置建立兩個新的AEM頁面：

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## 遠端SPA對應

離開遠端SPA的請求對應是透過 `setupProxy` 設定完成於 [BootstrapSPA](./spa-bootstrap.md).

## SPA編輯器對應

透過SPA SPA Editor開啟SPA時，AEM要求的對應可透過以下位置完成設定的Sling對應： [設定AEM](./aem-configure.md).

## 在AEM中建立內容頁面

首先，建立中介 `adventure` 頁面區段：

1. 登入AEM Author
1. 導覽至 __網站> WKND應用程式>我們>英文> WKND應用程式首頁__
   + 此AEM頁面會對應為SPA的根目錄，所以我們在這裡開始為其他SPA路由建立AEM頁面結構。
1. 點選 __建立__ 並選取 __頁面__
1. 選取 __遠端SPA頁面__ 範本，然後點選 __下一個__
1. 填寫頁面屬性
   + __標題__：探險
   + __名称__: `adventure`
      + 此值會定義AEM頁面的URL，因此必須符合SPA的路由區段。
1. 點選 __完成__

然後，建立與需要可編輯區域的每個SPA URL相對應的AEM頁面。

1. 瀏覽至新的 __冒險__ 網站管理員中的頁面
1. 點選 __建立__ 並選取 __頁面__
1. 選取 __遠端SPA頁面__ 範本，然後點選 __下一個__
1. 填寫頁面屬性
   + __標題__：巴厘島衝浪營
   + __名称__: `bali-surf-camp`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路由的最後一個區段
1. 點選 __完成__
1. 重複步驟3-6以建立 __波特蘭貝爾瓦納__ 頁面，包含：
   + __標題__：波特蘭的Beervana
   + __名称__: `beervana-in-portland`
      + 此值會定義AEM頁面的URL，因此必須符合SPA路由的最後一個區段

這兩個AEM頁面會保留其相符SPA路由的個別編寫內容。 如果其他SPA路由需要編寫，則必須在其SPA SPA URL的遠端AEM頁面的根頁面(`/content/wknd-app/us/en/home`AEM )。

## 更新WKND應用程式

讓我們放置 `<ResponsiveGrid...>` 在中建立的元件 [最後一章](./spa-container-component.md)，放入我們的 `AdventureDetail` SPA元件，建立可編輯的容器。

### 放置ResponsiveGrid SPA元件

放置 `<ResponsiveGrid...>` 在 `AdventureDetail` 元件會在該路徑中建立可編輯的容器。 訣竅在於多重路由會使用 `AdventureDetail` 要呈現的元件，我們必須動態調整  `<ResponsiveGrid...>'s pagePath` 屬性。 此 `pagePath` 必須衍生為根據路由例項顯示的冒險指向對應的AEM頁面。

1. 開啟並編輯 `react-app-/src/components/AdventureDetail.js`
1. 匯入 `ResponsiveGrid` 元件並放置在上方 `<h2>Itinerary</h2>` 元件。
1. 在 `<ResponsiveGrid...>` 元件。 請注意 `pagePath` 屬性新增目前的 `slug` 會依照上述定義的對應，對應至冒險頁面。
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   這指示 `ResponsiveGrid` 元件以從AEM資源擷取其內容：

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`


更新 `AdventureDetail.js` 包含以下行：

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

此 `AdventureDetail.js` 檔案應如下所示：

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## 在AEM中編寫容器

使用 `<ResponsiveGrid...>` 就位，及其 `pagePath` 我們會根據呈現的冒險動態設定，嘗試在其中製作內容。

1. 登入AEM Author
1. 導覽至 __網站> WKND應用程式>我們>英文__
1. __編輯__ 此 __wknd應用程式首頁__ 頁面
   + 導覽至 __巴厘島衝浪營__ 在SPA中路由以進行編輯
1. 選取 __預覽__ 從右上角的模式選擇器
1. 點選 __巴厘島衝浪營__ SPA中的卡片以導覽至其路由
1. 選取 __編輯__ 從模式選擇器
1. 找到 __配置容器__ 上方的可編輯區域 __行程__
1. 開啟 __頁面編輯器的側欄__，然後選取 __元件檢視__
1. 將部分已啟用的元件拖曳至 __配置容器__
   + 图像
   + 文本
   + 标题

   並建立一些促銷行銷資料。 它看起來可能像這樣：

   ![Bali Adventure Detail製作](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __預覽__ 您在AEM頁面編輯器中進行的變更
1. 重新整理在本機執行的WKND應用程式 [http://localhost:3000](http://localhost:3000)，導覽至 __巴厘島衝浪營__ 檢視所編寫變更的路由！

   ![遠端SPA巴厘島](./assets/spa-dynamic-routes/remote-spa-final.png)

導覽至沒有對應AEM頁面的冒險詳細路徑時，該路徑例項沒有編寫能力。 若要在這些頁面上啟用撰寫功能，只需在「 」下使用相符名稱建立AEM頁面 __冒險__ 頁面！

## 恭喜！

恭喜！您已在SPA中新增編寫動態路由的功能！

+ 將AEM React Editable元件的ResponsiveGrid元件新增至動態路由
+ 建立AEM頁面，以支援在SPA中製作兩個特定路線（Bali Surf Camp和Beervana in Portland）
+ 動態的巴厘島衝浪營路線上的編寫內容！

您現在已經完成探索AEM SPA Editor如何用來將特定可編輯區域新增到遠端SPA的第一步！
