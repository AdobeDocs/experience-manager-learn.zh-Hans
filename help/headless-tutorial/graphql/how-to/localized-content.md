---
title: 搭配AEM Headless使用當地語系化內容
description: 瞭解如何使用GraphQL查詢AEM的本地化內容。
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 3%

---

# AEM Headless本地化內容

AEM提供 [翻譯整合框架](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) 針對Headless內容，可讓您輕鬆翻譯內容片段和支援資產，以便用於各種地區設定。 這是用於翻譯其他AEM內容(例如頁面、體驗片段、資產和Forms)的相同架構。 一次 [Headless內容已翻譯](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=zh-Hans)和發佈後，即可供Headless應用程式使用。

## 資產資料夾結構{#assets-folder-structure}

確保AEM中的本地化內容片段遵循 [建議的本地化結構](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![本地化的AEM資產資料夾](./assets/localized-content/asset-folders.jpg)

地區設定資料夾必須是同層級，而且資料夾名稱（而不是標題）必須是有效的 [ISO 639-1代碼](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 代表資料夾中包含之內容的地區設定。

地區設定代碼也是用於篩選GraphQL查詢傳回的內容片段的值。

| 地區代碼 | AEM路徑 | 內容地區設定 |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | 德文內容 |
| en | /content/dam/.../**en**/... | 英文內容 |
| es | /content/dam/.../**es**/... | 西班牙文內容 |

## GraphQL持續查詢

AEM提供 `_locale` 可依地區設定代碼自動篩選內容的GraphQL篩選器。 例如，查詢 [wknd網站專案](https://github.com/adobe/aem-guides-wknd) 可以使用新的持久查詢完成 `wknd-shared/adventures-by-locale` 定義為：

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

此 `$locale` 變數用於 `_locale` 篩選器需要地區設定代碼(例如 `en`， `en_us`，或 `de`)中指定的 [AEM資產資料夾基礎本地化慣例](#assets-folder-structure).

## React範例

讓我們建立一個簡單的React應用程式，該應用程式使用地區設定選擇器，控制要從AEM查詢的Adventure內容。 `_locale` 篩選。

時間 __英文__ 在地區設定選擇器中選取，然後在「 」下選取「英文冒險內容片段」 `/content/dam/wknd/en` 會傳回，當 __西班牙文__ 選取「 」，然後選取「 」下的西班牙文內容片段 `/content/dam/wknd/es`，以此類推。

![本地化React範例應用程式](./assets/localized-content/react-example.png)

### 建立 `LocaleContext`{#locale-context}

首先，建立 [React內容](https://reactjs.org/docs/context.html) 允許在React應用程式的元件間使用地區設定。

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### 建立 `LocaleSwitcher` React元件{#locale-switcher}

接下來，建立地區設定切換器React元件，將 [LocaleContext](#locale-context) 使用者選取的值。

此地區設定值用於驅動GraphQL查詢，確保它們僅傳回符合所選地區設定的內容。

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### 使用查詢內容 `_locale` 篩選{#adventures}

Adventures元件會依地區設定查詢AEM的所有冒險並列出其標題。 這是透過使用將儲存在React內容中的地區設定值傳遞給查詢來達成 `_locale` 篩選。

此方法可以延伸至您應用程式中的其他查詢，確保所有查詢僅包含使用者地區設定選擇所指定的內容。

在自訂React勾點中執行針對AEM的查詢 [getAdventuresByLocale，詳情請參閱查詢AEM GraphQL檔案](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### 定義 `App.js`{#app-js}

最後，使用將React應用程式包住以連結在一起 `LanguageContext.Provider` 並設定地區設定值。 這允許其他React元件 [地區切換程式](#locale-switcher)、和 [冒險](#adventures) 以共用地區設定選擇狀態。

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
